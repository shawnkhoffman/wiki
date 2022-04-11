---
title: Managing Pods in Namespaces
keywords: Kubernetes, Pods
summary: "When you would use the Sidecar Container Pattern on a Pod, with examples"
sidebar: k8s_sidebar
permalink: /k8s-managing-pods-in-namespaces
folder: pods
tags: [Kubernetes, Pods]
---

The concept of the Kubernetes Namespace is based on the Linux namespace, and even runs on Linux namespaces, which are used to partition kernel resources so that defined processes can view and interact with a limited set of resources, though Kubernetes can support other implementations as well. Namespaces are used for the resource isolation for both resource utilization and security purposes.

A few use cases for Kubernetes Namespaces include multi-tenant architectures and strictly separating customer resources, deploying different applications or application components depending on security or resource requirements, and having multiple environments to deploy to (e.g., *dev, test, and prod*).

kubectl create namespace NAMESPACE_NAME
kubectl ... -n namespace
kubectl get ns
kubectl get pods -n NAMESPACE_NAME

<!-- Gets all resources in all of your namespaces -->
kubectl get all -A

In Kubernetes there is not an easy way to rename your namespace.
