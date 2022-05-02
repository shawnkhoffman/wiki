---
title: Dockerfile Best Practices for Security
keywords: Docker, Container Security
summary: "Recommendations to improve the security of your Docker images from the Dockerfile, thereby reducing the attack surface of containers running from those images"
sidebar: docker_sidebar
permalink: /dockerfile-best-practices-for-security
folder: docker
tags: [Docker, Container Security]
---

There are a few things you should consider when you think of what might be scaled or distributed on your containers in addition to the applications that you have running on them, including any attack vectors that could be exploited.

These recommendations address only how you might consider building your Dockerfiles with a methodical security approach, respectively.

## Evalutate your Base Images

The first line of a Dockerfile is a `FROM` stanza, indicating a base image that the new image is built from.

It is highly advised that you refer to an image from a trusted registry. Arbitrary third-party base images could include malicious code, so you may want to consider mandating the use of preapproved or "golden" base images.

The smaller the base image, the less likely that it includes unnecessary code, and hence the smaller the attack surface. Consider building from `SCRATCH` or using a minimal base image such as `alpine` or `distroless`. Smaller images also have the benefit of being quicker to ship and send over a network.

Carefully consider using a tag or a digest to reference the base image. The build will be more reproducible if you use a digest, but it means you are less likely to pick up new versions of a base image that might include security updates. Furthermore, you should pick up missing updates through a vulnerability scan of your completed image.

## Use multi-stage builds

The Docker [multi-stage build](/docker-multi-stage-build) is a way of eliminating unnecessary contents in the final image. An initial stage can include all the packages and toolchain required to build an image, but many of these tools are not needed at runtime. For example, if you write an executable in Go, it needs the Go compiler in order to create an executable program. The container that runs the program doesn't need to have access to the Go compiler. In this example, it would be a good idea to break the build into a multi-stage build: one stage for compiling and creating a binary executable, and the next stage to package only the standalone executable. The image that gets deployed has a much smaller attack surface; a nonsecurity benefit is that the image itself will also be smaller, so the time to pull the image is reduced.

## Use Non-root USER

The `USER` stanza in a Dockerfile specifies that the default user identity for running containers based on this image isn't the root user. You don't want all your containers running as root, so specify a non-root user in your Dockerfiles.

## Restrict RUN commands

The Dockerfile `RUN` stanza lets you run any arbitrary command. If an attacker can compromise the Dockerfile with the default security settings, that attacker can run any code of their choosing. Following the principle of least privilege, only enable the permissions to run arbitrary container builds on your system to people who require them. Also, make sure that privileges to edit Dockerfiles are limited to trusted members of your team, and pay close attention to code reviewing these changes. You might even want to institute a check or an audit log when any new or modified `RUN` commands are introduced in your Dockerfiles.

## Considerations for Volume mounts

Particularly for demos or tests, we often mount host directories into a container through volume mounts. It's important to check that Dockerfiles don't mount sensitive directories like the host's root directory, `/etc`, or `/bin` into a container, and mounting the entire filesystem would do this inherently.

Here are a few examples of the security issues in doing so:

- Mounting `/etc` would permit modifying the host's `/etc/passwd` file from within the container, or would allow tampering with `cron` jobs, or `init`, or `systemd`.

- Mounting `/bin` or similar directories such as `/usr/bin` or `/usr/sbin` would allow the container to write executables into the host directory — including overwriting existing executables with malicious ones.

- Mounting host log directories into a container could enable an attacker to modify the logs to erase their tracks on that host.

- In a Kubernetes environment, mounting `/var/log` can give access to the entire host filesystem to any user who has access to `kubectl logs`. This is because container log files are symlinks from `/var/log` to elsewhere in the filesystem, but there is nothing to stop the container from pointing the symlink at any other file.

## Abstract sensitive data away from Dockerfiles

Application code often needs certain credentials to do its job. For example, it may need a password to access a database, or a token giving it permission to access a particular API.

Don't store secrets or sensitive data into Dockerfiles. Adding credentials, passwords, or other secret data makes it easier for those secrets to be exposed. You could encrypt a secret in the source code, but then you'll need to add another step to decrypt it (e.g., if you're using a tool like *SOPS*), or you would need to pass in another secret so that the container can decrypt it which means the secret can't change unless you rebuild the container image. Furthermore, a centralized automated system for managing secrets (like CyberArk or Hashicorp Vault) can't control the life cycle of a secret that is hardcoded in the source.

You *could* pass secrets over a network using encryption in-transit, but the best option is to write them into files located in a temporary directory held in memory that the container can access through a mounted volume. Because the file is mounted from the host into the container, it can be updated from the host side at any time without having to restart the container. Provided the application knows to retrieve a new secret from the file if the old secret stops working, this means you can rotate secrets without having to restart containers.

## Avoid setuid binaries

Normally, when you execute a file, the process that gets started inherits your user ID. If the file has the `setuid` bit set, the process will have the user ID of the file's owner.

The `setuid` bit dates from a time when privileges were much simpler; either your process had root privileges or it didn't. The `setuid` bit provided a mechanism for granting extra privileges to non-root users. Version 2.2 of the Linux kernel introduced more granular control over these extra privileges through *capabilities.*

It's a good idea to avoid including executable files with the `setuid` bit, as these could potentially lead to privilege escalation.

## Avoid unnecessary code

The less code you have in a container, the smaller its attack surface will be. Avoid adding packages, libraries, and executables into an image unless they are absolutely necessary. Likewise, if you can base your image from `SCRATCH`, or one of the `alpine` or `distroless` options, you're likely to have significantly less code and, therefore, fewer vulnerabilities in your image.

## Package everything that your container needs to run properly

If the previous point exhorted you to exclude superfluous code from a build, this point is a corollary: do include everything that your application needs to operate. If you allow for the possibility of a container installing additional packages at runtime, how will you check that those packages are legitimate? It's far better to do all the installation and validation when the container image is built and create an immutable image.

---

Authored by: Shawn Hoffman

<br>

**References:**

- [Docker Hub. Alpine container image](https://hub.docker.com/_/alpine)
- [GoogleContainerTools on GitHub. distroless container image](https://github.com/GoogleContainerTools/distroless)
- [Mozilla on GitHub. SOPS](https://github.com/mozilla/sops)
- [Linux manual page. Capabilities](https://man7.org/linux/man-pages/man7/capabilities.7.html)
