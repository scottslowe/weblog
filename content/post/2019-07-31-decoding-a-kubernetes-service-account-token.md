---
author: slowe
categories: Tutorial
comments: true
date: 2019-07-31T12:00:00Z
tags:
- Kubernetes
- CLI
- JSON
- Security
title: Decoding a Kubernetes Service Account Token
url: /2019/07/31/decoding-a-kubernetes-service-account-token/
---

Recently, while troubleshooting a separate issue, I had a need to get more information about the token used by Kubernetes Service Accounts. In this post, I'll share a quick command-line that can fully decode a Service Account token.<!--more-->

Service Account tokens are stored as Secrets in the "kube-system" namespace of a Kubernetes cluster. To retrieve just the token portion of the Secret, use `-o jsonpath` like this (replace "sa-token" with the appropriate name for your environment):

    kubectl -n kube-system get secret sa-token \
    -o jsonpath='{.data.token}'

The output is Base64-encoded, so just pipe the output into `base64`:

    kubectl -n kube-system get secret sa-token \
    -o jsonpath='{.data.token}' | base64 --decode

The result you're seeing is a JSON Web Token (JWT). You could use [the JWT web site][link-2] to decode the token, but given that I'm a fan of the CLI I decided to use [this JWT CLI utility][link-1] instead:

    kubectl -n kube-system get secret sa-token \
    -o jsonpath='{.data.token}' | base64 --decode | \
    jwt decode -

The final `-`, for those who may not be familiar, is the syntax to tell the `jwt` utility to look at STDIN for the JWT it needs to decode (this functionality was added in a very recent release of `jwt`, by the way).

The output will look something like this (note that these credentials were taken from an ephemeral Kubernetes cluster that has since been decommissioned):

```text
Token header
------------
{
  "alg": "RS256",
  "kid": ""
}

Token claims
------------
{
  "iss": "kubernetes/serviceaccount",
  "kubernetes.io/serviceaccount/namespace": "kube-system",
  "kubernetes.io/serviceaccount/secret.name": "calico-node-token-bvwbn",
  "kubernetes.io/serviceaccount/service-account.name": "calico-node",
  "kubernetes.io/serviceaccount/service-account.uid": "530a0377-b34a-11e9-9553-020f8bca7f56",
  "sub": "system:serviceaccount:kube-system:calico-node"
}
```

There you have it---from Kubernetes Secret all the way to fully-decoded JSON Web Token. I don't know if you'll ever have the need for this process, but in the event you do, here it is. Hopefully it proves useful to someone! [Hit me up on Twitter][link-3] if you have any comments, questions, suggestions, or corrections.

[link-1]: https://github.com/mike-engel/jwt-cli
[link-2]: https://jwt.io
[link-3]: https://twitter.com/scott_lowe
