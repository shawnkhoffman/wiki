---
title: Ambassador Container Pattern
authors: Shawn Hoffman
editors: 
summary: "When you would use the Ambassador Container Pattern on a Pod, with examples"
tags: [Kubernetes, Pods]
keywords: Kubernetes, Pods
references: https://en.wikipedia.org/wiki/Reflective_programming, https://cloud.google.com/blog/products/databases/running-redis-on-gcp-four-deployment-scenarios
permalink: /k8s-ambassador-container-pattern

folder: pods
sidebar: k8s_sidebar
---

An **Ambassador container** is a form of *sidecar container* that serves as a *proxy* to the primary container on a pod. It is used as a proxy between the application container and one or more outside services; this is ideal when you don't want the primary container on the pod to be exposed and you instead want routing to go through the ambassador, allowing it to be a *representative* of your primary container. It also forms an abstraction layer so that the application developer can focus on the application itself without worrying about the infrastructure connectivity details.

An example use case for this is when there are other tasks that require the application's function in order to work correctly and you direct those tasks over to a sidecar container.

For instance, you know that most applications use database connections. You also know that when you have multiple deployment environments, you might have a database for dev, as well as a test or staging database and then a live production database. Moreover, a ODBC/JDBC connection string can be easily changed through an environment variable or even a ConfigMap, but you could also use an Ambassador container that proxies the database connection to the appropriate server depending on where it runs. Therefore, you don't need to make changes to your code to update the connection string – instead, you can leave the database server running at `localhost`, and when deployed to a different environment, the Ambassador container will detect which environment that it is running in (e.g., *through the Reflection pattern*), and make the correct connection.

## Connecting to multiple Redis servers

Another well-known use case for the Ambassador container is when your application needs to connect to a caching server such as Redis.

Let's say in your production environment you have more than one Redis instance setup for high availability, and so you need to configure your application to select a healthy node and then connect to it. You can configure an Ambassador container that proxies those connections, and your configuration may look something like this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ambassador
spec:
  containers:
  - name: redis-client
    image: redis

  - name: ambassador-container
    image: malexer/twemproxy
    env:
    - name: REDIS_SERVERS
      value: redis-st-0.redis-svc.default.svc.cluster.local:6379:1 redis-st-1.redis-svc.default.svc.cluster.local:6379:1
    ports:
    - containerPort: 6380
```

The above Pod definition defines two containers: the application container running the Redis image as a client to other Redis servers, and the Ambassador container running the `malexer/twemproxy` image, which is an open source proxy server that provides a way to evenly distribute cached data between multiple Redis instances to improve the performance, reliability, and resilience of a distributed system, and it is one of Google's recommended solutions for managing a cluster of Memorystore instances on GCP. You can also see that the Ambassador container is listening to `localhost` because it shares the same Pod as the application container, it listens to port 6380 as a default configuration for this image, and it has the Redis instances passed in as environment variables – in the form of `address:port:weight`, as required.

At this point, we only need the two Redis instances that the Ambassador container is configured to communicate with. The following StatefulSet definition creates them:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-svc
  labels:
    app: redis
spec:
  ports:
  - port: 6379
    name: redis-port
  clusterIP: None
  selector:
    app: redis
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: redis-st
spec:
  serviceName: "redis-svc"
  replicas: 2
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis
        ports:
        - containerPort: 6379
          name: redis
        volumeMounts:
        - name: redis-data
          mountPath: /data
  volumeClaimTemplates:
  - metadata:
      name: redis-data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

## Interacting with your Ambassador container

If you want to run commands on your Ambassador container or just see how things are operating, you could simply run the following command:

<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#baseCommand" data-toggle="tab">Base command</a></li>
    <li><a href="#example" data-toggle="tab">Example</a></li>
</ul>
  <div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="baseCommand">
    <p><b>$ kubectl exec POD_NAME -c CONTAINER_NAME [flags] -- COMMAND [args...] [options] </b></p><br>
</div>

<div role="tabpanel" class="tab-pane" id="example">
    <p><b>$ kubectl exec ambassador -c ambassador-container -it -- sh </b></p></div><br>
</div>
