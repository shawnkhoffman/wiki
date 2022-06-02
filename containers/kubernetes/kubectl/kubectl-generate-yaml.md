---
title: Generating YAML from kubectl
authors: Shawn Hoffman
editors: 
summary: "How to generate YAML for objects on Kubernetes"
tags: [Kubernetes, kubectl]
keywords: Kubernetes, kubectl
references: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#run, https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create, https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply, https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#replace
permalink: /kubectl-generate-yaml

folder: kubectl
sidebar: k8s_sidebar
---

Let's say you need some YAML to make use of Kubernetes' powerful declarative nature, but you don't exactly know where to start. This is where the kubectl flags `--dry-run` and `-o` come in to help you generate your own YAML. Generating YAML from kubectl offers you minimal margin for error with ensuring your YAML configuration has the correct format when you go to deploy Kubernetes objects.

<br>

Using `--dry-run`, you can specify whether you want to use the `client` option or the `server` option; whichever one you should use depends entirely on how you strategize your deployments. The `client` option simply outputs the declared items without publishing them to the Kubernetes API Server and without communicating with the Kubernetes control plane whatsoever, whereas the `server` option sends data to both the API Server as well as the cluster for authentication and validation but it does not actually publish those changes.

For the purposes of demonstration, I will only use the `client` option.

The `-o` (output) flag allows you to tell kubectl which formatting you want the output to be in – in this case, it will be YAML.

## YAML for Pods

{{site.data.alerts.note}} It is not advised to use Naked Pods in most cases. The examples below are for demonstration purposes only.{{site.data.alerts.end}}


<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#yaml-for-pods-baseCommand" data-toggle="tab">Base command</a></li>
    <li><a href="#yaml-for-pods-example1" data-toggle="tab">Example 1</a></li>
    <li><a href="#yaml-for-pods-example2" data-toggle="tab">Example 2</a></li>
</ul>
  <div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="yaml-for-pods-baseCommand">
    <p><b>kubectl run [POD NAME] --image=[CONTAINER IMAGE] --dry-run=client -o yaml</b></p>
</div>

<div role="tabpanel" class="tab-pane" id="yaml-for-pods-example1">
    <p><b>kubectl run test-nginx --image=nginx --dry-run=client -o yaml</b></p>
    </div>

<div role="tabpanel" class="tab-pane" id="yaml-for-pods-example2">
    <p><b>kubectl run test-busybox --image=busybox --dry-run=client -o yaml -- sleep 3600</b></p>
    <br>
    <p><i># Note the command 'sleep 3600', and the double hyphens '--', is provided after '-o yaml'. This is done intentionally because anything provided after the double hyphens `--` will be understood by kubectl as a container image command to be started with the pod.</i></p>
    </div>
</div>

<br>

The output will look something like this:

{% highlight yaml linenos %}

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: test-busybox
  name: test-busybox
spec:
  containers:
  - args:
    - sleep
    - "3600"
    image: busybox
    name: test-busybox
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

{% endhighlight %}

## Send YAML output to a file

You can send the output of YAML you have generated to a file, which you can then use to deploy your object(s).

<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#send-output-to-yaml-example1" data-toggle="tab">Example 1</a></li>
    <li><a href="#send-output-to-yaml-example2" data-toggle="tab">Example 2</a></li>
</ul>
  <div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="send-output-to-yaml-example1">
    <p><b>kubectl run test-nginx --image=nginx --dry-run=client -o yaml > test-nginx.yaml</b></p>
    </div>

<div role="tabpanel" class="tab-pane" id="send-output-to-yaml-example2">
    <p><b>kubectl run test-busybox --image=busybox --dry-run=client -o yaml -- sleep 3600 > test-busybox.yaml</b></p>
    </div>
</div>

## Deploying Kubernetes objects from YAML file

Using a YAML file you have generated, you can deploy the Kubernetes objects by using the `-f` (file) flag.

<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#deploy-with-yaml-example1" data-toggle="tab">Example 1</a></li>
    <li><a href="#deploy-with-yaml-example2" data-toggle="tab">Example 2</a></li>
    <li><a href="#deploy-with-yaml-example3" data-toggle="tab">Example 3</a></li>
</ul>
  <div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="deploy-with-yaml-example1">
    <p><i># Create resources from YAML</i></p>
    <p><b>kubectl create -f test-nginx.yaml</b></p>
    </div>

<div role="tabpanel" class="tab-pane" id="deploy-with-yaml-example2">
    <p><i># Create resources from YAML if they don't exist yet, and modify them if they already exist and were previously created with this command</i></p>
    <p><b>kubectl apply -f test-nginx.yaml</b></p>
    </div>

<div role="tabpanel" class="tab-pane" id="deploy-with-yaml-example3">
    <p><i># Replaces resources with the new configuration defined in the YAML file</i></p>
    <p><b>kubectl replace -f test-nginx.yaml</b></p>
    </div>
</div>
