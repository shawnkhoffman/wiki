---
title: Current user or role does not have access to Kubernetes objects
summary: "<br>
When you install EKS for the first time, you might receive the following error from the AWS Console: </br>
</br>
Your current user or role does not have access to Kubernetes objects on this EKS cluster. This may be due to the current user or role not having Kubernetes RBAC permissions to describe cluster resources or not having an entry in the cluster’s auth config map."
tags: [eks, kubernetes, containers]
keywords: eks, kubernetes, containers
sidebar: eks_sidebar
permalink: /eks-current-user-or-role-does-not-have-access-to-k8s-objects.html
# simple_map: true
# map_name: usermap
# box_number: 1
# folder: product2
---

## Why this happens

As the error states clearly, this happens when your AWS user account doesn't have access to the control plane within Kubernetes. This is not to be confused with the control plane on the AWS platform – this is more specific to the Kubernetes API.

EKS creates a ConfigMap called `aws-auth` in the namespace `kube-system` to configure a relationship between your AWS user accounts and the Kubernetes API.

## How to solve this problem

The proper configuration really depends on how you implement authorization.

### Option 1: modify mapRoles or mapUsers

If the user needs full administrator access to the EKS cluster, you can bind their AWS user account to the Kubernetes group called `system:masters` – this is a special built-in group on Kubernetes with unrestricted rights to the Kubernetes API (by default, the group is bound with the Kubernetes ClusterRole `cluster-admin`).

If you handle authorization at the role-level, the respective role needing access would need to be to added to `mapRoles` inside the ConfigMap `aws-auth`. However, if authorization is handled at the user-level, you can pass in the AWS account's ARN into `mapUsers`.

```yaml
---
kind: ConfigMap
metadata:
  name: aws-auth
  namespace: kube-system
apiVersion: v1
data:
  mapUsers: |
    - userarn: arn:aws:iam::123456789012:user/obishawnkenobi
      username: obishawnkenobi
      groups:
        - system:masters
  mapRoles: |
    - rolearn: arn:aws:iam::123456789012:role/devops
      username: devops
      groups:
        - system:masters
```

### Option 2 (the most secure): Create a new ClusterRole

There is another option that is more secure and allows you to follow the principle of least privilege. This option is to create a Kubernetes ClusterRole that allows access only to the resources that it needs. An example use case for this solution is to provision read-only access to a dev team, so that they can view resources in the AWS console, and provision full administrator access to DevOps. I'll illustrate this below.

1.) First, I create the ClusterRole:

```yaml
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
name: eks-console
rules:
- apiGroups:
    - ""
    resources:
    - nodes
    - namespaces
    - pods
    verbs:
    - get
    - list
- apiGroups:
    - apps
    resources:
    - deployments
    - daemonsets
    - statefulsets
    - replicasets
    verbs:
    - get
    - list
- apiGroups:
    - batch
    resources:
    - jobs
    verbs:
    - get
    - list
```

2.) Next, I bind the ClusterRole with a group.

```yaml
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
name: eks-console
subjects:
- kind: Group
    name: eks-console
    apiGroup: rbac.authorization.k8s.io
roleRef:
kind: ClusterRole
name: eks-console
apiGroup: rbac.authorization.k8s.io
```

3.) And finally, I edit the Kubernetes ConfigMap `aws-auth` and pass in the AWS role's ARN with the Kubernetes group `eks-console`.

```yaml
---
kind: ConfigMap
metadata:
  name: aws-auth
  namespace: kube-system
apiVersion: v1
data:
  mapUsers: |
    - userarn: arn:aws:iam::123456789012:user/obishawnkenobi
      username: obishawnkenobi
      groups:
        - system:masters
  mapRoles: |
    - rolearn: arn:aws:iam::123456789012:role/developers
      username: developers
      groups:
        - eks-console
    - rolearn: arn:aws:iam::123456789012:role/devops
      username: devops
      groups:
        - system:masters
```

---

Authored by: Shawn Hoffman

<br>

**References:**

- [AWS Knowledge Center: "How do I resolve the 'Your current user or role does not have access to Kubernetes objects on this EKS cluster' error in Amazon EKS?"](https://aws.amazon.com/premiumsupport/knowledge-center/eks-kubernetes-object-access-error/)