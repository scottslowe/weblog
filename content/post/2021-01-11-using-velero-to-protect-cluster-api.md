---
author: slowe
categories: Tutorial
comments: true
date: 2021-01-11T10:25:00-07:00
tags:
- AWS
- Backup
- CAPI
- Kubernetes
- Velero
title: Using Velero to Protect Cluster API
url: /2021/01/11/using-velero-to-protect-cluster-api/
---

[Cluster API][link-3] (also known as CAPI) is, as you may already know, an effort within the upstream [Kubernetes][link-1] community to apply Kubernetes-style APIs to cluster lifecycle management---in short, to use Kubernetes to manage the lifecycle of Kubernetes clusters. If you're unfamiliar with CAPI, I'd encourage you to check out [my introduction to Cluster API][xref-1] before proceeding. In this post, I'm going to show you how to use [Velero][link-2] (formerly Heptio Ark) to backup and restore Cluster API objects so as to protect your organization against an unrecoverable issue on your Cluster API management cluster.<!--more-->

To be honest, this process is so straightforward it almost doesn't need to be explained. In general, the process for backing up the CAPI management cluster looks like this:

1. Pause CAPI reconciliation on the management cluster.
2. Back up the CAPI resources.
3. Resume CAPI reconciliation.

In the event of catastrophic failure, the recovery process looks like this:

1. Restore from backup onto another management cluster.
2. Resume CAPI reconciliation.

Let's look at these steps in a bit more detail.

## Pausing and Resuming Reconciliation

The process for pausing and resuming reconciliation of CAPI resources is outlined in [this separate blog post][xref-2]. To summarize that post here for convenience, the Cluster API spec includes a `paused` field that causes the Cluster API controllers to stop reconciliation when the field is set to `true` (and resume reconciliation when the field is `false` or absent). Setting this field allows you, the cluster operator, to pause or resume reconciliation.

## Backing up CAPI Resources

Once you've paused reconciliation for Cluster API, you can then run a backup using Velero. Based on my testing, I didn't see anything unusual or odd about running a backup; generally speaking, it looks to be as simple as `velero backup create` (with appropriate flags). Given the large number of custom resources used by Cluster API (Clusters, Machines, MachineDeployments, KubeadmConfigs, etc.) it may be challenging to include only Cluster API resources using Velero's `--include-resources` functionality. It's probably easier to either a) not use any of Velero's filtering functionality and catch everything, or b) make sure you are either using namespaces or labels comprehensively for CAPI objects and then use Velero's `--include-namespaces` and/or `--selector` filtering options for selecting things to be included in the backup. Refer to [Velero's resource filtering documentation][link-6] for more details.

## Restoring from Backup

As with creating the backup using Velero, restoring from the Velero backup follows the standard Velero procedures (i.e., run `velero restore create` with appropriate flags/options). Naturally, the cluster to which you are restoring should be an appropriately-configured Cluster API management cluster with the appropriate Cluster API components already installed.

Since this article is more focused on the "Oh no my management cluster is dead" scenario, [all of the information on disaster recovery in the Velero docs][link-4] is appropriate.

After the restore is complete, you'll then want to resume reconciliation on the target/destination cluster, as outlined above.

## Backup and Restore Versus Moving

The `clusterctl` utility used by CAPI for initializing management clusters (among other things) also has a `move` subcommand that can be used to move CAPI resources from one cluster to another cluster. Some readers may be wondering why we should bother with Velero, and if they could use `clusterctl move` instead.

`clusterctl move` is a viable option for moving CAPI objects between two clusters _as long as both the source and target clusters are up and running_. Using Velero, on the other hand, only requires that the source cluster is up and running when a backup needs to be taken; users can then restore this backup to another cluster even if the source cluster has completely failed. I'm also of the opinion that Velero will provide more fine-grained control over what can be backed up and restored, although I have yet to test that directly.

## Additional Resources

Readers may find the following resources useful as well:

[Disaster recovery use case with Velero][link-4]

[Cluster migration use case with Velero][link-5]

I hope that readers find this article helpful. If there's anything I've discussed here that you'd like to see examined/explained in greater detail, feel free to let me know. You can find me on [the Kubernetes Slack instance][link-7], or find [me on Twitter][link-8]. I'd love to hear from you!

[link-1]: https://kubernetes.io/
[link-2]: https://velero.io
[link-3]: https://cluster-api.sigs.k8s.io/
[link-4]: https://velero.io/docs/v1.5/disaster-case/
[link-5]: https://velero.io/docs/v1.5/migration-case
[link-6]: https://velero.io/docs/v1.5/resource-filtering/
[link-7]: https://kubernetes.slack.com/
[link-8]: https://twitter.com/scott_lowe/
[xref-1]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
[xref-2]: {{< relref "2020-11-25-pausing-cluster-api-reconciliation.md" >}}
