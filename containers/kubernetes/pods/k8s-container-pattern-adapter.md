---
title: Adapter Container Pattern
authors: Shawn Hoffman
editors: 
summary: "When you would use the Adapter Container Pattern on a Pod, with examples"
tags: [Kubernetes, Pods, Kubernetes Patterns]
keywords: Kubernetes, Pod
references: https://hub.docker.com/r/nginx/nginx-prometheus-exporter
permalink: /k8s-adapter-container-pattern

folder: pods
sidebar: k8s_sidebar
---

An **Adapter container** is one that *adapts* the output of the primary container on a pod. An example use case for this is when you need to standardize and normalize the data output from the primary container into a specification that fits the standards across your applications without any change to application code or logic.

## nginx webserver with Prometheus monitoring

A very common use case for this design pattern is for the Prometheus monitoring service, which requires diagnostic data from applications in a specific format that Prometheus expects.

Imagine you have a pod with a primary container running a web server on nginx, and you need to convert the output from the endpoint that nginx provides for querying the web server's status to the Prometheus standard format. Assuming you have enabled this endpoint on nginx in the `default.conf` file, your Pod configuration might look something like this:

{% highlight yaml linenos %}

apiVersion: v1
kind: Pod
metadata:
  name: adapter
spec:
  volumes:
  - name: nginx-conf
    configMap:
      name: nginx-conf
      items:
      - key: default.conf
        path: default.conf

  containers:
  - name: webserver-container
    image: nginx
    ports:
    - containerPort: 80
    volumeMounts:
    - mountPath: /etc/nginx/conf.d
      name: nginx-conf
      readOnly: true

  - name: adapter-container
    image: nginx/nginx-prometheus-exporter
    args: ["-nginx.scrape-uri","http://localhost/nginx_status"]
    ports:
    - containerPort: 9113

{% endhighlight %}

You can see two containers configured to this pod. The adapter container uses the `nginx/nginx-prometheus-exporter` image to handle the data conversion of the telemetry that nginx exposes on `/nginx_status` to the Prometheus format.

## Interacting with your Adapter container

If you want to run commands on your Adapter container or just see how things are operating, you could simply run the following command:

<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#baseCommand" data-toggle="tab">Base command</a></li>
    <li><a href="#example" data-toggle="tab">Example</a></li>
</ul>
  <div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="baseCommand">
    <p><b>$ kubectl exec POD_NAME -c CONTAINER_NAME [flags] -- COMMAND [args...] [options] </b></p><br>
</div>

<div role="tabpanel" class="tab-pane" id="example">
    <p><b>$ kubectl exec adapter -c adapter-container -it -- sh </b></p></div><br>
</div>
