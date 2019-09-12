---
author: slowe
categories: Explanation
comments: true
date: 2019-08-26T23:00:00Z
tags:
- Kubernetes
- CAPI
title: An Introduction to Kubernetes Cluster API
url: /2019/08/26/an-introduction-to-kubernetes-cluster-api/
---

In this post, I'd like to provide a high-level introduction to [the Kubernetes Cluster API][link-1]. The aim of Cluster API (CAPI, for short) is, as outlined in [the project's GitHub repository][link-2], "a Kubernetes project to bring declarative, Kubernetes-style APIs to cluster creation, configuration, and management". This high-level introduction serves to establish some core terminology and concepts upon which I'll build in future posts about CAPI.<!--more-->

First, let's start with some terminology:

_Bootstrap cluster:_ The bootstrap cluster is a temporary cluster optionally used by CAPI. When it _is_ used, it's used to create a more permanent cluster that is CAPI-enabled (the management cluster). Typically, the bootstrap cluster is created locally using `kind` (other options are possible), and is destroyed once the management cluster is up and running.

_Management cluster:_ The management cluster is a long-lived cluster that has the CAPI Custom Resource Definitions (CRDs) and runs the CAPI controllers. Typically, users would use the management cluster to create and manage the lifecycle of one or more workload clusters. This cluster can be an already running cluster into which users install the CAPI CRDs and controllers, or the management cluster can be an entirely new CAPI-enabled cluster created by the temporary bootstrap cluster.

_Workload cluster:_ This is a cluster whose lifecycle is managed by CAPI via the management cluster, but isn't actually CAPI-enabled itself and it doesn't manage the lifecycle of other clusters. Workload clusters won't have any knowledge of the CAPI CRDs. Typically, users would end up with multiple workload clusters.

There are two basic tools a user uses to interact with Cluster API:

* `clusterctl` is used to create management clusters via the use of a bootstrap cluster (which is typically ephemeral). Users would also use `clusterctl` to tear down a management cluster. The individual providers may have additional tools; for example, the AWS provider also uses a tool named `clusterawsadm`. In the v1alpha1 release of CAPI, `clusterctl` was provider-specific and bundled with the various providers. In the v1alpha2 release of CAPI, `clusterctl` was decoupled from the providers and is no longer provider-specific. Although `clusterctl` works with v1alpha2, it is now deprecated (and is slated to be removed in a future release).
* `kubectl`, on the other hand, is used to create workload clusters (via the management cluster). For example, with a properly-formed manifest containing a CAPI definition of a cluster, `kubectl apply -f cluster.yaml` would create a new workload cluster.

In general, the workflow of using CAPI looks something like this:

1. Users create a management cluster. This can be done by installing the CAPI CRDs and controllers into an existing Kubernetes cluster using `kubectl`, or by using `clusterctl create` to create a temporary bootstrap cluster which then, in turn, creates a new cluster as the management cluster. In the case of applying the CAPI CRDs and controllers to an existing cluster, steps two through five do not happen.
2. (Only when using `clusterctl` and a temporary bootstrap cluster) The bootstrap cluster is enabled for CAPI (the CRDs and controllers are installed), and the definition for the management cluster---which was passed in via parameters to the `clusterctl create` command---is applied to the bootstrap cluster.
3. (Only when using `clusterctl` and a temporary bootstrap cluster) The controllers in the bootstrap cluster see the CAPI definitions applied and begin the reconciliation process. This usually means creating resources using the given CAPI provider.
4. (Only when using `clusterctl` and a temporary bootstrap cluster) Once the management cluster has been bootstrapped, the CAPI CRDs and controllers are "pivoted" (moved) from the bootstrap cluster to the management cluster. From this point forward, the management cluster is responsible for completing the reconciliation of the manifests initially applied to create it.
5. (Only when using `clusterctl` and a temporary bootstrap cluster) The bootstrap cluster---which no longer contains any of the CAPI definitions or state, as all that was moved into the management cluster---is destroyed.
6. Users use `kubectl` to apply CAPI manifests to the management cluster in order to create, manage, and destroy workload clusters. Workload clusters don't have the CAPI components installed.

The end result of all this is users now have a far simpler way (once the management cluster is up and running) to create Kubernetes clusters. Users have the option of creating their own infrastructure and having CAPI consume it, or having CAPI provision infrastructure and then consume it. Both models are supported by CAPI.

In future posts, I'll build on this high-level overview by outlining more detailed steps for using CAPI with various providers/platforms. The first of those posts is available [here for the Cluster API AWS provider][xref-1] (for the CAPI v1alpha1 release). Myles Gray has [a good write-up on the Cluster API vSphere provider][link-6], so I probably won't write an article for vSphere.

If you have any questions or need more information, feel free to find me in [the Kubernetes Slack][link-3] ([sign up here][link-4] if you don't have an account) or [hit me up on Twitter][link-5]. Thanks!

**UPDATE 12 September 2019:** I've updated this post with information from the CAPI v1alpha2 release.

[link-1]: https://cluster-api.sigs.k8s.io/
[link-2]: https://github.com/kubernetes-sigs/cluster-api
[link-3]: https://kubernetes.slack.com/
[link-4]: http://slack.k8s.io/
[link-5]: https://twitter.com/scott_lowe
[link-6]: https://blah.cloud/kubernetes/first-look-automated-k8s-lifecycle-with-clusterapi/
[xref-1]: {{< relref "2019-08-27-bootstrapping-a-kubernetes-cluster-on-aws-with-clusterapi.md" >}}
