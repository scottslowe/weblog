---
author: slowe
categories: Tutorial
comments: true
date: 2022-10-06T13:30:00-06:00
tags:
- Azure
- CLI
- Kubernetes
- Microsoft
- Pulumi
title: Streamlining the User Experience for Accessing AKS Clusters
url: /2022/10/06/streamlining-the-user-experience-for-accessing-aks-clusters/
---

Lately I've been spending a little bit of time building [Pulumi][link-1] programs to assist with standing up Azure Kubernetes Service (AKS) clusters. I've learned a pretty fair amount about Azure and AKS along the way, as expected, but I was taken aback by the poor user experience (in my opinion) when it came to accessing the AKS clusters once they'd been established. In this post, I'll share a small tweak you can make that will, in most cases, make accessing your AKS clusters a great deal smoother.<!--more-->

What do I mean by "poor user experience"? In the same vein as comparable offerings from AWS (EKS) and Google Cloud (GKE), AKS leverages Azure's identity and access management (IAM) functionality, so that users have a single place to manage user and group entities. This makes perfect sense! What doesn't make sense to me, though, is the requirement that users must perform a separate login process to gain access to the cluster, _even if the user is already authenticated via the Azure CLI._ This is very counter to both EKS and GKE, where---if you are already authenticated via their CLI tools---no additional steps are necessary to access appropriately-configured managed Kubernetes clusters on their platforms. This change I'll show you will fix that, and bring Azure's experience back in line with AWS and Google Cloud.

There are some limitations/caveats to this tweak I'm going to show you:

* This tweak assumes that you are (or will be) authenticated/logged in with the Azure CLI. If you're using Pulumi, then this _typically_ going to be the case, although there are [other authentication options][link-4] available.
* The AKS cluster you wish to connect to using this method must be configured to use AKS-managed Azure AD integration, also referred to as "managed AAD". (More information [here][link-2].)

To access a Kubernetes cluster, you need a Kubeconfig file which provides the information on the cluster, the user, and the context (combination of cluster and user). When using Pulumi, there are (at least) two ways of getting a Kubeconfig for an AKS cluster you created:

1. If you're using Pulumi's Azure Native provider, use the `listManagedClusterUserCredentials` function to retrieve this information (more information [here][link-5]).
2. After the cluster has been created, use the Azure CLI `az aks get-credentials` command to retrieve the Kubeconfig.

In both cases, you'll get a Kubeconfig in which the `users` section includes a single user. The configuration for that user leverages the `exec` functionality of the Kubernetes client authentication API to execute a binary named `kubelogin` (GitHub repository [here][link-3]). It will look something like this (I've omitted some lines for the purposes of brevity and clarity):

```yaml
users:
- name: clusterUser_resourceGroup8355ff6e_aks-clusterad642fc1
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      args:
      - get-token
      - --environment
      - AzurePublicCloud
      # The --server-id, --client-id, and --tenant-id lines have been omitted
      - --login
      - devicelogin
      command: kubelogin
      env: null
      provideClusterInfo: false
```

It's the very last element in the `args` list that triggers the separate authentication process; this is outlined in the `kubelogin` repository (see the "Device Code Flow (default)" section).

On that same page, you'll also see a "Azure CLI token login" that shows you how to change the Kubeconfig to leverage the Azure CLI. **This is the change you need to make.**

Simply remove the `--client-id` and `--tenant-id` lines, and then change `devicelogin` to `azurecli`. Once you've made that change, when you use `kubectl` to access the Kubernetes cluster referenced by this Kubeconfig file, you won't be prompted for a separate authentication process---it will just leverage the existing authentication credentials you've obtained via the Azure CLI. This makes the experience on par with EKS and GKE, and far more seamless than the default configuration that uses `devicelogin`.

Kudos to Pete Cook ([@blindpete][link-6] on Twitter), who pointed me in the direction of [the `kubelogin` GitHub repository][link-3] after I complained about the user experience with AKS. Thank you, Pete!

I hope this helps some folks out there. I'm not an Azure/AKS expert, so if you are and I've misrepresented something here, please let me know so I can correct it. You can reach out to [me on Twitter][link-7] (DMs are open), or find me in any one of a number of Slack communities (I frequent both the Kubernetes Slack community and the Pulumi Slack community). Thanks for reading!

[link-1]: https://www.pulumi.com/
[link-2]: https://learn.microsoft.com/en-us/azure/aks/managed-aad
[link-3]: https://github.com/Azure/kubelogin
[link-4]: https://www.pulumi.com/registry/packages/azure-native/installation-configuration/
[link-5]: https://www.pulumi.com/registry/packages/azure-native/api-docs/containerservice/listmanagedclusterusercredentials/
[link-6]: https://twitter.com/blindpete
[link-7]: https://twitter.com/scott_lowe
