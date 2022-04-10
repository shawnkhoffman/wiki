---
title: Init Container Pattern
keywords: Kubernetes, Pods
summary: "When you would use the Init Container Pattern on a Pod, with examples"
sidebar: k8s_sidebar
permalink: /k8s-init-container-pattern
folder: pods
tags: [Kubernetes, Pods]
---

An **Init container** is an additional container in a pod that completes a task before the primary container is started. The purpose of this pattern is to decouple the primary application container from any initialization logic that it needs, similarly to how you would when you instantiate a new object with a constructor in any Object Oriented Programming language.

What makes this type of container different from any sidecar container is that it runs *before* the primary container starts, whereas a sidecar container continuously runs *alongside* the primary container.

Instantiating your pod with an init container allows you to introduce a *separation of concerns,* which can allow you to enforce certain access policies in your RACI matrix. For example, having a separate team define the initialization steps of an application gives your organization greater flexibility when it comes to authorization and access control. Imagine your application requires performing tasks on certain resources that require a security clearance to access, such as modifying firewall rules; this pattern allows you to delegate this task to those individuals who have access.

## A few considerations for Init containers

Init containers shouldn't contain complex logic that takes a long time to complete; if you find that you're adding too much logic to init containers, you should consider moving part of it to the main application on the primary container.

Furthermore, init containers are started and executed in a sequence; so, since the primary container will not start until all init container tasks are completed successfully, you may consider breaking apart a very long set of tasks into individual init containers to handle each task so that you can more easily identify which steps fail.

Moreover, if any of the init containers on a pod fail, the whole pod gets restarted (unless you set the *restartPolicy* to *Never*); therefore, you can only deploy them using the declarative model, and their code *must* be idempotent. Restarting the pod means re-executing all the containers on that pod, including any init containers; therefore, you may want to ensure that the startup logic tolerates being executed multiple times without causing duplication. For instance, if one of  your tasks is a database migration and that task completes during the first iteration, any following executions of that migration should be ignored.

Finally, the Kubernetes Scheduler gives higher precedence to the resources and limits of the init containers. Such behavior must be thoroughly considered as it may present undesired results. For example, if you have one init container and one application container and you set the resources and limits of the init container to be higher than those of the application container, then the entire pod is scheduled only if there's an available node that satisfies the init container's requirements. In other words, even if there's an unused node where the application container can run, the pod will not be deployed to the node if the init container has higher resource requirements that the node can handle. Thus, you should be as strict as possible when defining the requests and limits of an init container. As a best practice, do not set those parameters to higher values than the application container's unless absolutely necessary.

## Use cases of Init containers

An init container is a good candidate for delaying the application initialization until one or more dependencies are available. For example, if your application depends on an API that imposes an API request-rate limit, you may need to wait for a certain time period to be able to receive responses from that API. This logic may be too heavy for the primary container; instead, a much simpler way would be to create an init container that waits until the API is ready before it exits successfully. Then the application container would start only after the init container has successfully completed its task.

Init containers cannot use health and readiness probes like application containers can. The reason is that init containers are meant to start and exit successfully, much like how Jobs and CronJobs behave.

### Seeding a database

Let's say you're working with a MySQL database and you need to seed it with some model data to test that the application can access the database as well as measure the application's query speed. You can use an init container to handle downloading the SQL dump file, which is hosted in another container, and restore it to the database.

Your pod definition might look something like this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: init
  labels:
    app: db
spec:
  initContainers:
    - name: fetch
      image: mwendler/wget
      command: ["wget","--no-check-certificate","https://sample-videos.com/sql/Sample-SQL-File-1000rows.sql","-O","/docker-entrypoint-initdb.d/dump.sql"]
      volumeMounts:
        - mountPath: /docker-entrypoint-initdb.d
          name: dump
  containers:
    - name: mysql
      image: mysql
      env:
        - name: MYSQL_ROOT_PASSWORD
          value: "example"
      volumeMounts:
        - mountPath: /docker-entrypoint-initdb.d
          name: dump
  volumes:
    - emptyDir: {}
      name: dump
```

The above configuration creates a pod that hosts two containers: the init continer, which is defined in `initContainers`, and the primary container which uses the `mysql` database image. The init container uses the `mwendler/wget` image because only the wget command is needed to download the SQL file that contains the database dump. The destination directory for the downloaded SQL file is the default directory used by the MySQL image to execute SQL files: `/docker-entrypoint-initdb.d`.

The init container mounts `/docker-entrypoint-initdb.d` to an `emptyDir` volume, and because both containers are hosted on the same pod, they share the same volume. So, the database container has access to the SQL file placed on the `emptyDir` volume.

### Delaying an application until dependencies are ready

In another scenario, you have an application that needs to wait until another service is available and responding to requests. Let's say the service is called `myservice` and has the following Service configuration:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myservice
spec:
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
```

Therefore, you might have a pod configuration that looks similar to this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: init
  labels:
    app: myapp
spec:
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
```

The application, running on `myapp-container`, does not function correctly except when the `myservice` application is running. You need to delay the `myapp` application until `myservice` is ready. You could handle this by using a simple `nslookup` command on the init container to constantly check for the successful name resolution of `myservice`. If nslookup is able to resolve `myservice`, then the service is started. Therefore, with a success exit status code, the init container terminates, giving way for the application container to start. Otherwise, the container sleeps for two seconds before trying again, delaying the application container start.

## Troubleshooting your Init containers

If you need to troubleshoot your init containers, or you just want to see how things are going, you can run `kubectl describe pods init-pod` and start by looking at information that `Events` will provide to you. If your init container seems to be hanging, look at the output under `Containers` and look for `State/Reason`; if the reason says *PodInitializing,* this means the init container is not finished with its assigned task, and if this is the case you can look at `Init Containers` to see its current status for further troubleshooting.

---

Authored by: Shawn Hoffman

<br>

**References:**

- [Kubernetes documentation. Init containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)
- [mwendler/wget](https://hub.docker.com/r/mwendler/wget)