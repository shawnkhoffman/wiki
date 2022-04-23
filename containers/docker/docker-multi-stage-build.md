---
title: Using Multi-Stage builds
keywords: Docker, Builds
summary: "How to use Docker Multi-Stage builds"
sidebar: docker_sidebar
permalink: /docker-multi-stage-build
folder: docker
tags: [Docker, Builds]
---

## What is a Multi-Stage Build?

A Multi-Stage Build is a method that allows you to use multiple images to build a single image, all from a single Dockerfile.

## The advantages of Multi-Stage Builds

Before Multi-Stage Builds, it was common to see multiple Dockerfiles on a single project, one for each stage of a build. Each stage also needed to be orchestrated by manually written shell scripts.

Also, Docker images can become much larger than they really need to be. The reason for this varies, but in some scenarios an image contains more than it actually needs to run the application that runs on the container. It's inefficient to have a ~700 MB image if you have a small application written in a language that compiles to a single binary.

Both of the points above imply inherent quality and security issues; the more steps your build process consists of, and the larger your image is, the bulkier your deployments, and perhaps the larger your attack surface on the container, will be.

Furthermore, you may be able to significantly reduce the size of an image by using a Multi-Stage Build defined in your Dockerfile.

## A Multi-Stage Dockerfile

In some example code below, I have a tiny HTTP server written in Go. It contains only a single API handler that listens on `localhost` at port `8080`:

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, World!")
}

func main() {
    http.HandleFunc("/", handler)
    log.Fatal(http.ListenAndServe("0.0.0.0:8080", nil))
}
```

If I put this into a Docker image using the base `golang` image from Docker Hub, it would probably sit at around ~700 MB. I *could* use the `alpine` version of the image, but that would only reduce the image size down to about 50% which is still over 1/4 of a GB in size — inefficient for a single container image.

In a compiled language like Go, you might rely on several dependencies. And if you handle dependencies with something like Go `dep`, you will need a Git client which then requires SSH to clone any needed source code, all just to end up with a tiny binary. But instead of building the process of dependency management into your image, it would be more efficient to build an image with just that tiny binary.

In my Dockerfile example below, I have defined a Multi-Stage Build for my Go API handler:

```dockerfile
FROM golang:alpine AS builder

WORKDIR /go/src/app
COPY main.go .

RUN go build -o webserver .

FROM alpine
WORKDIR /app
COPY --from=builder /go/src/app/ /app/

CMD ["./webserver"]
```

Note the `--from` argument next to the second `COPY` stanza; the result of this command is that the binary that exists in `/go/src/app` once the first image is built will be copied over to the second image.

I could then build an image from this single Dockerfile to end up with an image of ~10 MB in size — a *significant* difference — and then test the image in a container using `docker run -p "8080:8080" webserver`.

Also, when you run `docker build` with a Multi-Stage Dockerfile, you can observe during the build process that the old image is discarded once the new image is being built at each stage of your build. Thus, you only end up with one image instead of two.

---

Authored by: Shawn Hoffman

<br>

**References:**

- [Docker. Using multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/)
