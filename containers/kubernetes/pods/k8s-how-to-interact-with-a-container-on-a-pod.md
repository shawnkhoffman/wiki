---
title: "How to: Interact With a Specific Container on a Pod"
keywords: Kubernetes, Pods
summary: "When you would use the Sidecar Container Pattern on a Pod, with examples"
sidebar: k8s_sidebar
permalink: /k8s-interacting-with-a-container-on-a-pod
folder: pods
tags: [Kubernetes, Pods]
---

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
