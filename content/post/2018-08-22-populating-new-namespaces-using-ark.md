---
author: slowe
categories: Education
comments: true
date: 2018-08-22T19:00:00Z
tags:
- Ark
- CLI
- Kubernetes
- Backup
title: Populating New Namespaces Using Heptio Ark
url: /2018/08/22/populating-new-namespaces-using-heptio-ark/
---

[Heptio Ark][link-1] is a tool designed to backup and restore [Kubernetes][link-2] cluster resources and persistent volumes. As such, it enables users to do a bunch of very useful things like copy cluster resources across cloud providers or replicate environments for development, staging, testing, QA, etc. In this post, I'll share a slightly different use case for Ark: populating resources into new Kubernetes namespaces.<!--more-->

Kubernetes namespaces, if you're not familiar, are a way to scope resource names and provide a way to divide cluster resources between multiple resources via resource quotas (see [the Kubernetes documentation on namespaces][link-3] for more details). As such, when you create a new Kubernetes namespace, it's empty. However, you may have a need or desire to have certain things present in every namespace within a cluster---for example, perhaps you have a set of ExternalName Services that point to resources outside the cluster to make it easier for applications and developers to integrate with external resources. Maybe you have a ConfigMap that developers can use to configure their applications. It could be that you want a particular secret to be present in all new namespaces so that developers don't need to worry about managing certain credentials. In such cases, someone (or something) would need to re-create those items every time a new namespace gets created. Or...you could use Ark!

Here are some details on the environment I'll use to walk you through how this works:

* Kubernetes 1.11.2 cluster running on AWS
* A namespace called `foo`
* In the `foo` namespace, three ExternalName Services: `extname-1` (points to "aws.amazon.com"), `extname-2` (points to "google.com"), and `extname-3` (points to "www.microsoft.com")
* Ark 0.9.3 configured and installed

To verify this, I'll run a container in the `foo` namespace and test that the ExternalName Services are working as expected:

    kubectl -n foo run -it --rm --restart=Never debug \
    --image=quay.io/mauilion/debug /bin/bash

This will, after a minute, open a prompt inside a container running in the `foo` namespace, at which point I can run `host extname-1` and see these results:

```
extname-1.foo.svc.cluster.local is an alias for aws.amazon.com.
aws.amazon.com is an alias for 0.aws-lbr.amazonaws.com.
0.aws-lbr.amazonaws.com is an alias for aws-us-east-1.amazon.com.
aws-us-east-1.amazon.com has address 52.46.133.33
```

Repeating this process for `extname-2` and `extname-3`, I see similar results---this tells me that the ExternalName Services are, in fact, created and working as expected, and that the cluster's DNS service is functioning as expected.

Now, let's create a new namespace (named `bar`, of course) and repeat this process:

    kubectl create namespace bar
    kubectl -n bar run -it --rm --restart=Never debug \
    --image=quay.io/mauilion/debug /bin/bash

When I run `host extname-1` in this container in this new namespace, I instead get `Host extname-1 not found: 3(NXDOMAIN)`---indicating the ExternalName Services are _not_ in this namespace, as fully expected.

Here's where we bring in Ark. I'll create a backup of the Services in the `foo` namespace:

    ark backup create foo-svc --include-namespaces foo \
    --include-resources services

As suggested in the output of this command, I'll run `ark backup describe foo-svc` to see the status of the backup job:

```
Name:         foo-svc
Namespace:    heptio-ark
Labels:       <none>
Annotations:  <none>

Phase:  Completed

Namespaces:
  Included:  foo
  Excluded:  <none>

Resources:
  Included:        services
  Excluded:        <none>
  Cluster-scoped:  auto

Label selector:  <none>

Snapshot PVs:  auto

TTL:  720h0m0s

Hooks:  <none>

Backup Format Version:  1

Started:    2018-08-21 15:52:31 -0600 MDT
Completed:  2018-08-21 15:52:32 -0600 MDT

Expiration:  2018-09-20 15:52:31 -0600 MDT

Validation errors:  <none>

Persistent Volumes: <none included>
```

Now comes the neat trick---I'll restore this backup into the `bar` namespace, using the `--namespace-mappings` option to Ark:

    ark restore create bar-svc --from-backup foo-svc \
    --namespace-mappings foo:bar

Now, I could run `ark restore describe bar-svc`, but let's just jump directly to see if the services have been restored. Running `kubectl -n bar get services` shows they're present:

```
NAME       TYPE          CLUSTER-IP  EXTERNAL-IP        PORT(S)  AGE
extname-1  ExternalName  <none>      aws.amazon.com     <none>   18m
extname-2  ExternalName  <none>      httpbin.org        <none>   18m
extname-3  ExternalName  <none>      www.microsoft.com  <none>   18m
```

Neat! With the `--namespace-mappings` parameter, users can backup from one namespace and restore into a different namespace, thus giving users the ability to "populate" new namespaces with a default set of objects that should be present in all new namespaces. Using Ark is, of course, not the _only_ way to do this, and I'll explore some other options in a future post.

This post just barely scratches the surface of what Ark is capable of doing; I highly encourage you to have a look at [the Ark documentation][link-4] for more details.

[link-1]: https://github.com/heptio/ark
[link-2]: https://kubernetes.io
[link-3]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[link-4]: https://heptio.github.io/ark/v0.9.0/
