---
author: slowe
categories: Tutorial
comments: true
date: 2020-11-25T10:45:00-07:00
tags:
- CAPI
- CLI
- Kubernetes
title: Pausing Cluster API Reconciliation
url: /2020/11/25/pausing-cluster-api-reconciliation/
---

Cluster API is a topic I've discussed here in a number of posts. If you're not already familiar with Cluster API (also known as CAPI), I'd encourage you to check out [my introductory post on Cluster API][xref-1] first; you can also visit [the official Cluster API site][link-1] for more details. In this short post, I'm going to show you how to pause the reconciliation of Cluster API cluster objects, a task that may be necessary for a variety of reasons (including backing up the Cluster API objects in your management cluster).<!--more-->

Since CAPI leverages [Kubernetes][link-2]-style APIs to manage Kubernetes cluster lifecycle, the idea of _reconciliation_ is very important---it's a core Kubernetes concept that isn't at all specific to CAPI. [This article on level triggering and reconciliation in Kubernetes][link-3] is a great article that helps explain reconciliation, as well as a lot of other key concepts about how Kubernetes works.

When reconciliation is active, the controllers involved in CAPI are constantly evaluating desired state and actual state, and then reconciling differences between the two. There may be times when you need to pause this reconciliation loop. Fortunately, CAPI makes this pretty easy: there is a `paused` field that allows users to pause the reconciliation loop (see [here in the v1alpha3 CAPI reference][link-4]).

This field is optional, which you can observe using `kubectl` with an existing workload cluster. For example, here is the relevant output from `kubectl get cluster workload1` against a CAPI management cluster; note the `paused` field is not present (i.e., reconciliation is currently active):

```yaml
spec:
  clusterNetwork:
    pods:
      cidrBlocks:
      - 192.168.0.0/16
  controlPlaneEndpoint:
    host: workload1-apiserver-1234567890.us-east-2.elb.amazonaws.com
    port: 6443
  controlPlaneRef:
    apiVersion: controlplane.cluster.x-k8s.io/v1alpha3
    kind: KubeadmControlPlane
    name: workload1-control-plane
    namespace: workload1
  infrastructureRef:
    apiVersion: infrastructure.cluster.x-k8s.io/v1alpha3
    kind: AWSCluster
    name: workload1
    namespace: workload1
```

Reconcilation is active when the field isn't present; setting the field to false is essentially the same as removing the field. To pause reconciliation, set it to true. One way of doing this is using `kubectl patch`, like this:

```
kubectl patch cluster workload1 --type merge \
-p '{"spec":{"paused": true}}'
```

Naturally, you'd want to change `workload1` to the name of the workload cluster for which you want reconciliation paused. After running the command, you can then run `kubectl get cluster <cluster-name> -o yaml` and verify the `paused` field is present:

```yaml
spec:
  clusterNetwork:
    pods:
      cidrBlocks:
      - 192.168.0.0/16
  controlPlaneEndpoint:
    host: workload1-apiserver-1234567890.us-east-2.elb.amazonaws.com
    port: 6443
  controlPlaneRef:
    apiVersion: controlplane.cluster.x-k8s.io/v1alpha3
    kind: KubeadmControlPlane
    name: workload1-control-plane
    namespace: workload1
  infrastructureRef:
    apiVersion: infrastructure.cluster.x-k8s.io/v1alpha3
    kind: AWSCluster
    name: workload1
    namespace: workload1
  paused: true
```

If you check the logs for the CAPI controllers, you'll see messages like this:

```
I1125 17:31:40.287717       1 machine_controller.go:170] controllers/Machine "msg"="Reconciliation is paused for this object" "machine"="workload1-md-0-58c6df55bc-cqt86" "namespace"="workload1"
```

When reconciliation is paused, the CAPI controllers **will not** make changes to the workload cluster in order to reconcile differences between actual state and desired state.

When you're ready to resume reconciliation again, just change the field to false:

```
kubectl patch cluster workload1 --type merge \
-p '{"spec":{"paused": false}}'
```

This will remove the field from the spec, allowing the CAPI controllers to resume reconciliation of the CAPI-related objects (like Clusters, Machines, and MachineDeployments).

As I mentioned at the start of this post, pausing CAPI reconciliation may be necessary in a few different situations. (Backing up Cluster API is one situation.)

If anyone has any questions, or if you spot an error in this post, please let me know. Hit [me on Twitter][link-99]; I'd love to hear from you!

[link-1]: https://cluster-api.sigs.k8s.io/
[link-2]: https://kubernetes.io/
[link-3]: https://hackernoon.com/level-triggering-and-reconciliation-in-kubernetes-1f17fe30333d
[link-4]: https://godoc.org/sigs.k8s.io/cluster-api/api/v1alpha3#ClusterSpec
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
