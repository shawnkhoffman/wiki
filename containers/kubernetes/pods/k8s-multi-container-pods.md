---
title: Multi-Container Pods
keywords: Kubernetes, Pods
summary: "When you would use multiple containers on a single Pod, and why"
sidebar: k8s_sidebar
permalink: /k8s-multi-container-pods
folder: pods
tags: [Kubernetes, Pods]
---

In most cases, a single-container pod is the gold standard because it is easier to build and maintain than a multi-container pod, and you can more easily define the workload of each pod as well as its respective configurations. Furthermore, single-container pods typically scale better. Remember that, when you scale pods on Kubernetes, you are scaling every container that each pod contains, so running multiple containers on a pod can make your application deployment on Kubernetes quite heavy and even difficult to manage.

Imagine needing to update just one of the containers on a multi-container pod; you would have to bring down the entire pod just to update one container. Now imagine that at scale!

For applications that require multiple containers, you should define microservices in your architecture. One of Kubernetes' greatest strengths is deploying applications with microservice architectures because it provides most of the resources that you would ever need, and each separate Kubernetes resource allows your application architecture and processes to be leaner and much easier to manage.

There *are* a few cases for when you might want to consider deploying multi-container pods, however.

## Sidecar Container

A sidecar container is one that *enhances* or *extends* the primary container on a pod, and this typically requires shared access to the same resources (e.g., *volumes* and the *network interface*) to exchange information. One such enhancement is logging or monitoring on your application. An example of this use case is Istio Service Mesh, which is a product used for monitoring and shaping traffic on Kubernetes. Istio adds a sidecar container to each pod in order to fetch the traffic telemetry that it needs. In this case, these containers belong together on the same pod.

Here's another real world example of the Sidecar Pattern.

Let's say you have a webserver container running on nginx. The access and error logs produced by the webserver are not critical enough to be placed on a persistent volume. However, developers need access to the last 24 hours of logs so that they can trace issues and bugs. Therefore, you need to ship the access and error logs from the webserver to a log aggregation service. Following the *separation of concerns* principle, you implement the sidecar design pattern and your configuration might look something like this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webserver
spec:
  volumes:
    - name: logs
      emptyDir: {}

  containers:
    - name: webserver
      image: nginx
      ports:
        - containerPort: 80
      volumeMounts:
        - name: logs
          mountPath: /var/log/nginx

    - name: sidecar
      image: busybox
      command: ["sh","-c","while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"]
      volumeMounts:
        - name: logs
          mountPath: /var/log/nginx
```

By deploying a second container that ships the error and access logs from nginx, you allow nginx to do what it does well, which is *serving web pages,* and you allow the second container to specialize in its task, which is *shipping logs.* Since containers are running on the same pod, you can share the `emptyDir` volume between them as a bind-mount to read and write logs.

The main container in the configuration is an nginx container that’s instructed to store its logs on the `emptyDir` volume mounted at `/var/log/nginx`. Mounting the volume at that location prevents nginx from outputting its log data to standard output and forces it to write it to the `access.log` and `error.log` files. The sidecar container conventionally comes second so that, when you apply the configuration, you target the main container by default. For the purpose of demonstration, the sidecar container executes the `cat` command to simulate sending the log data to a log aggregator every thirty seconds, but the data is actually being sent to the Kubernetes log which can be viewed by running the `kubectl log` command.

Moreover, if you created multiple pods from this type of configuration on the same host, and you used the same location for storing each nginx container's logs, you could also use a *DaemonSet* to deploy a log-collector container like Filebeat or Logstash to collect those logs and send them to a log aggregator service such as ElasticSearch.

## Ambassador Container

An ambassador container serves as a *proxy* to the primary container on a pod. The main use case for this is a proxy where you don't want the actual container to be exposed, and you instead want routing to the primary container to go through the ambassador, allowing it to be a *representative* of your primary container.

## Adapter Container

This container is one that *adapts* the output of the primary container on a pod. An example use case for this is when you need to standardize and normalize the data output from the primary container into a specification that fits the standards across your applications without any change to application code or logic.

A very common secnario for this design pattern is the Prometheus monitoring service which requires diagnostic data from applications in a specific format that Prometheus expects. Imagine you have a pod with a primary container running a web server on nginx, and you need to convert the output from the endpoint that nginx provides for querying the web server's status to the Prometheus standard format. Assuming you have enabled this endpoint on nginx in the `default.conf` file, your Pod configuration might look something like this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webserver
spec:
  volumes:
  - name: nginx-conf
    configMap:
      name: nginx-conf
      items:
      - key: default.conf
        path: default.conf

  containers:
  - name: webserver
    image: nginx
    ports:
    - containerPort: 80
    volumeMounts:
    - mountPath: /etc/nginx/conf.d
      name: nginx-conf
      readOnly: true

  - name: adapter
    image: nginx/nginx-prometheus-exporter
    args: ["-nginx.scrape-uri","http://localhost/nginx_status"]
    ports:
    - containerPort: 9113
```

You can see two containers configured to this pod. The adapter container uses the `nginx/nginx-prometheus-exporter` image to handle the data conversion of the telemetry that nginx exposes on `/nginx_status` to the Prometheus format.

## Interacting with specific containers on a Pod

If you wanted to run commands, or just see how things are operating, on your sidecar, ambassador, or adapter container, you could simply run the following command:

<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#interacting-with-pods-baseCommand" data-toggle="tab">Base command</a></li>
    <li><a href="#interacting-with-pods-example1" data-toggle="tab">Sidecar</a></li>
    <li><a href="#interacting-with-pods-example2" data-toggle="tab">Ambassador</a></li>
    <li><a href="#interacting-with-pods-example3" data-toggle="tab">Adapter</a></li>
</ul>
  <div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="interacting-with-pods-baseCommand">
    <p><b>$ kubectl exec [POD] [-c CONTAINER] [flags] -- COMMAND [args...] [options]</b></p>
</div>

<div role="tabpanel" class="tab-pane" id="interacting-with-pods-example1">
    <p><b>$ kubectl exec webserver -c sidecar -it -- /bin/bash</b></p>
    </div>

<div role="tabpanel" class="tab-pane" id="interacting-with-pods-example2">
    <p><b>$ kubectl exec webserver -c ambassador -it -- /bin/bash</b></p>
    </div>

<div role="tabpanel" class="tab-pane" id="interacting-with-pods-example3">
    <p><b>$ kubectl exec webserver -c adapter -it -- /bin/bash</b></p>
    </div>
</div>

---

Authored by: Shawn Hoffman

<br>

**References:**

- [nginx/nginx-prometheus-exporter](https://hub.docker.com/r/nginx/nginx-prometheus-exporter)
