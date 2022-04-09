---
title: Sidecar Container Pattern
keywords: Kubernetes, Pods
summary: "When you would use the Sidecar Container Pattern on a Pod, with examples"
sidebar: k8s_sidebar
permalink: /k8s-sidecar-container-pattern
folder: pods
tags: [Kubernetes, Pods]
---

A **Sidecar container** is one that *enhances* or *extends* the primary container on a pod, and this typically requires shared access to the same resources (e.g., *volumes* and the *network interface*) to exchange information. One such enhancement is logging or monitoring on your application. An example of this use case is Istio Service Mesh, which is a product used for monitoring and shaping traffic on Kubernetes. Istio adds a sidecar container to each pod in order to fetch the traffic telemetry that it needs. In this case, these containers belong together on the same pod.

Here's another real world example of the Sidecar Pattern.

## nginx webserver with log shipping

Let's say you have a webserver container running on nginx. The access and error logs produced by the webserver are not critical enough to be placed on a persistent volume. However, developers need access to the last 24 hours of logs so that they can trace issues and bugs. Therefore, you need to ship the access and error logs from the webserver to a log aggregation service. Following the *separation of concerns* principle, you implement the sidecar design pattern and your configuration might look something like this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: sidecar
spec:
  volumes:
    - name: logs
      emptyDir: {}

  containers:
    - name: webserver-container
      image: nginx
      ports:
        - containerPort: 80
      volumeMounts:
        - name: logs
          mountPath: /var/log/nginx

    - name: sidecar-container
      image: busybox
      command: ["sh","-c","while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"]
      volumeMounts:
        - name: logs
          mountPath: /var/log/nginx
```

By deploying a second container that ships the error and access logs from nginx, you allow nginx to do what it does well, which is *serving web pages,* and you allow the second container to specialize in its task, which is *shipping logs.* Since containers are running on the same pod, you can share the `emptyDir` volume between them as a bind-mount to read and write logs.

The main container in the configuration is an nginx container that's instructed to store its logs on the `emptyDir` volume mounted at `/var/log/nginx`. Mounting the volume at that location prevents nginx from outputting its log data to standard output and forces it to write it to the `access.log` and `error.log` files. The sidecar container conventionally comes second so that, when you apply the configuration, you target the main container by default. For the purpose of demonstration, the sidecar container executes the `cat` command to simulate sending the log data to a log aggregator every thirty seconds, but the data is actually being sent to the Kubernetes log which can be viewed by running the `kubectl log` command.

Moreover, if you created multiple pods from this type of configuration on the same host, and you used the same location for storing each nginx container's logs, you could also use a *DaemonSet* to deploy a log-collector container like Filebeat or Logstash to collect those logs and send them to a log aggregator service such as ElasticSearch.

## Interacting with your Sidecar container

If you want to run commands on your Sidecar container or just see how things are operating, you could simply run the following command:

```bash
$ kubectl exec sidecar -c sidecar-container -it -- /bin/bash
```

---

Authored by: Shawn Hoffman

<br>

**References:**

- [NGINX. Configuring logging](https://docs.nginx.com/nginx/admin-guide/monitoring/logging/)
