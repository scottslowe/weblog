---
aliases: /2020/09/01/updating-aws-credentials-in-cluster-api/
author: slowe
categories: Tutorial
comments: true
date: 2020-09-02T14:30:00-07:00
tags:
- AWS
- CAPI
- Kubernetes
- Security
title: Updating AWS Credentials in Cluster API
url: /2020/09/02/updating-aws-credentials-in-cluster-api/
---

I've written a bit here and there about [Cluster API][link-1] (aka CAPI), mostly focusing on the [Cluster API Provider for AWS][link-2] (CAPA). If you're not yet familiar with CAPI, have a look at [my CAPI introduction][xref-1] or check [the Introduction section of the CAPI site][link-3]. Because CAPI interacts directly with infrastructure providers, it typically has to have some way of authenticating to those infrastructure providers. The AWS provider for Cluster API is no exception. In this post, I'll show how to update the AWS credentials used by CAPA.<!--more-->

Why might you need to update the credentials being used by CAPA? Security professionals recommend that users rotate credentials on a regular basis, and when those credentials get rotated you'll need to update what CAPA is using. There are other reasons, too; perhaps you started with one set of credentials but now want to move to a different set of credentials. Fortunately, the process for updating the CAPA credentials isn't too terribly tedious.

CAPA stores the credentials it uses as a Secret in the "capa-system" namespace. You can use `kubectl -n capa-system get secrets` and you'll see the "capa-manager-bootstrap-credentials" Secret. The credentials themselves are stored as a key named `credentials`; you can use this command to retrieve the credentials and decode them (if you're using macOS, change the `-d` to `-D`):

    kubectl -n capa-system get secret capa-manager-bootstrap-credentials \
    -o jsonpath="{.data.credentials}" | base64 -d

The command will return something like this (but with valid access key ID, secret access key, and region values, obviously):

    [default]
    aws_access_key_id = <access-key-id-value-here>
    aws_secret_access_key = <secret-access-key-value-here>
    region = <aws-region-here>

There's a couple different ways to update this information. What I'll describe below is _one way_ to do it.

First, you'll need to encode a correct/working set of credentials into a Base64-encoded string. Fortunately, the `clusterawsadm` command can do this for you. Before running `clusterawsadm`, be sure to set---as needed---the AWS_PROFILE, AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, and the AWS_REGION environment variables. If you're using version 0.5.4 or earlier of `clusterawsadm`, you can use this `clusterawsadm` command to generate the necessary Secret materials:

    clusterawsadm alpha bootstrap encode-aws-credentials

If you're using `clusterawsadm` 0.5.5 or later, the command changes to this:

    clusterawsadm bootstrap credentials encode-as-profile

Keep the output of this command handy; you'll need it shortly.

Next, use `kubectl -n capa-system edit secret capa-manager-bootstrap-credentials` to edit the Secret. Replace the existing value of the `data.credentials` field with the new value created above using `clusterawsadm`. Save your changes.

For the CAPA controller manager to pick up the new credentials in the Secret, restart it with this command:

    kubectl -n capa-manager rollout restart \
    deployment capa-controller-manager

The AWS infrastructure provider in your CAPI management cluster should now be good to go with the updated credentials.

It also appears that if you need to upgrade the CAPI components on your management cluster (using `clusterctl upgrade plan` and `clusterctl upgrade apply`), that operation will _also_ ensure that updated credentials are embedded into the "capa-manager-bootstrap-credentials" Secret.

If you have any questions about this process, if I've explained something incorrectly, or if you have any suggestions for how I can improve this article, please feel free to reach out to [me on Twitter][link-5] or find me on [the Kubernetes Slack community][link-6]. All constructive comments and feedback are welcome!

[link-1]: https://cluster-api.sigs.k8s.io/
[link-2]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/
[link-3]: https://cluster-api.sigs.k8s.io/introduction.html
[link-4]: https://cluster-api.sigs.k8s.io/user/quick-start.html
[link-5]: https://twitter.com/scott_lowe
[link-6]: https://kubernetes.slack.com
[xref-1]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
