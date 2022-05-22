---
title: Get help on Kubernetes object attributes
authors: Shawn Hoffman
editors:
summary: "How to get help on Kubernetes object attributes and work with them"
tags: [Kubernetes, kubectl]
keywords: Kubernetes, kubectl
references: https://kubernetes.io/docs/reference/kubectl/, https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#explain

folder: kubectl
sidebar: k8s_sidebar
---

It is crucial to know how to work with Kubernetes objects, which are what you use to deploy your application architecture; however, unfortunately there are far too many objects in Kubernetes to memorize, and it is impossible to memorize every attribute for every object, so it is a far better use of your time to instead remember *where to look* whenever you need to work with one.

First, let's take a look at how to identify all the different objects that you can work with in Kubernetes.

## What Kubernetes API objects can I work with?

Every object in Kubernetes works with the Kubernetes API. Use the following command to see each object.

{% highlight shell linenos %}
$ kubectl api-resources
{% endhighlight %}

## I've found the object I need – Now what?

Let's say you need to work with a Kubernetes Pod. Run the following command to see the first layer of attributes for pods.

{% highlight shell linenos %}
$ kubectl explain pods
{% endhighlight %}

As you can see, there are quite a few. You can use interpolation to drill down into each attribute to see any relevant sub-attributes they contain. Let's say you're defining a Kubernetes configuration in a YAML file and you need to know what possible parameters you can use for the `spec` attribute. This is the command you would run:

{% highlight shell linenos %}
$ kubectl explain pods.spec
{% endhighlight %}

Now imagine you need to define the *restartPolicy* on one or more pods in your configuration. You could simply run this command to see the expected input into that parameter:

{% highlight shell linenos %}
$ kubectl explain pods.spec.restartPolicy
{% endhighlight %}

But let's say you wanted to make use of labels and namespaces for your pod. You may know that these attributes are defined in the `metadata` attribute for Kubernetes objects; but, what does Kubernetes expect in your configuration for `labels` and `namespaces`? You can quickly figure that out from the following commands:

{% highlight shell linenos %}
$ kubectl explain pods.metadata.labels
$ kubectl explain pods.metadata.namespace
{% endhighlight %}

By the end of it, you might have yourself a really nice configuration file to deploy resources with.

{% highlight yaml linenos %}

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: my-app
  name: my-app
spec:
  containers:
  - image: my-app-image
    name: my-app
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

{% endhighlight %}
