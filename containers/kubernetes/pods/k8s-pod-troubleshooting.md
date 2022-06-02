---
title: Troubleshooting Pods & Containers
keywords: Kubernetes, Pod
summary: "How to troubleshoot pods & containers"
sidebar: k8s_sidebar
permalink: /k8s-troubleshooting-pods-and-containers
folder: pods
tags: [Kubernetes, Pods]
---

## Finding pods

Always start your troubleshooting process with finding your pods.

In the output, you want to see that your pod exists as well as the data under the `READY`, `STATUS`, and `RESTARTS` columns.

<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#get-pods-baseCommand" data-toggle="tab">Base command</a></li>
    <li><a href="#get-pods-single-pod" data-toggle="tab">On a single Pod</a></li>
    <li><a href="#get-pods-namespace" data-toggle="tab">On a single Namespace</a></li>
    <li><a href="#get-pods-all" data-toggle="tab">All pods on the cluster</a></li>
</ul>
<div class="tab-content">
    <div role="tabpanel" class="tab-pane active" id="get-pods-baseCommand">
        <p><b>$ kubectl get pods (POD_NAME) [options] </b></p>
    </div>
    <div role="tabpanel" class="tab-pane" id="get-pods-single-pod">
        <p><b>$ kubectl get pods (POD_NAME) </b></p>
    </div>
    <div role="tabpanel" class="tab-pane" id="get-pods-namespace">
        <p><b>$ kubectl get pods (POD_NAME) -n (NAMESPACE_NAME) </b></p>
    </div>
    <div role="tabpanel" class="tab-pane" id="get-pods-all">
        <p><b>$ kubectl get pods -A </b></p>
    </div><br>
</div><br>

Furthermore, you can output this command to YAML to verify pod configurations.

## Viewing the status of pods

<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#describe-pods-baseCommand" data-toggle="tab">Base command</a></li>
    <li><a href="#describe-single-pod" data-toggle="tab">On a single Pod</a></li>
    <li><a href="#describe-pods-namespace" data-toggle="tab">On a single Namespace</a></li>
    <li><a href="#describe-pods-all" data-toggle="tab">All pods on the cluster</a></li>
</ul>
<div class="tab-content">
    <div role="tabpanel" class="tab-pane active" id="describe-pods-baseCommand">
        <p><b>$ kubectl describe pods (POD_NAME) [options] </b></p><br>
    </div>
    <div role="tabpanel" class="tab-pane" id="describe-single-pod">
        <p><b>$ kubectl describe pod (POD_NAME) </b></p>
    </div>
    <div role="tabpanel" class="tab-pane" id="describe-pods-namespace">
        <p><b>$ kubectl describe pod (POD_NAME) -n (NAMESPACE_NAME) </b></p>
    </div>
    <div role="tabpanel" class="tab-pane" id="describe-pods-all">
        <p><b>$ kubectl describe pods -A </b></p>
    </div><br>
</div>

## Getting logs on pods

<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#kubectl-logs-baseCommand" data-toggle="tab">Base command</a></li>
    <li><a href="#kubectl-logs-single-pod" data-toggle="tab">On a single Pod</a></li>
</ul>
<div class="tab-content">
    <div role="tabpanel" class="tab-pane active" id="kubectl-logs-baseCommand">
        <p><b>$ kubectl logs (POD_NAME) [options] </b></p><br>
    </div>
    <div role="tabpanel" class="tab-pane" id="kubectl-logs-single-pod">
        <p><b>$ kubectl logs (POD_NAME) </b></p>
    </div>
</div>

## Executing commands on pods

<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#kubectl-executingCommands-baseCommand" data-toggle="tab">Base command</a></li>
    <li><a href="#kubectl-executingCommands-example" data-toggle="tab">Example</a></li>
</ul>
<div class="tab-content">
    <div role="tabpanel" class="tab-pane active" id="kubectl-executingCommands-baseCommand">
        <p><b>$ kubectl exec -it (POD_NAME) -- [command] </b></p>
        <br>
    </div>
    <div role="tabpanel" class="tab-pane" id="kubectl-executingCommands-example">
        <p><b>$ kubectl exec -it webserver -- sh </b></p>
        <br>
        <p><i># As containers are minimized environments, sh should always be available.</i></p>
    </div>
</div>

<!-- ## Using Port Forwarding to access pods

This exposes a Pod port on the kubectl host that forwards to the Pod. -->
