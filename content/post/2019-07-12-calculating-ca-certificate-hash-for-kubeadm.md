---
author: slowe
categories: Tutorial
comments: true
date: 2019-07-12T12:00:00Z
tags:
- Ansible
- CLI
- Encryption
- Kubeadm
- Kubernetes
- Linux
title: Calculating the CA Certificate Hash for Kubeadm
url: /2019/07/12/calculating-ca-certificate-hash-for-kubeadm/
---

When using `kubeadm` to set up a new [Kubernetes][link-1] cluster, the output of the `kubeadm init` command that sets up the control plane for the first time contains some important information on joining additional nodes to the cluster. One piece of information in there that (until now) I hadn't figured out how to replicate was the CA certificate hash. (Primarily I hadn't figured it out because I hadn't tried.) In this post, I'll share how to calculate the CA certificate hash for `kubeadm` to use when joining additional nodes to an existing cluster.<!--more-->

When looking to figure this out, I first started with the `kubeadm` documentation. My searches led me [here][link-3], which states:

>The hash is calculated over the bytes of the Subject Public Key Info (SPKI) object (as in RFC7469). This value is available in the output of “kubeadm init” or can be calculated using standard tools.

That's useful information, but what are the "standard tools" being referenced? I knew that a lot of work had been put into `kubeadm init phase` (for breaking down the `kubeadm init` workflow), but a quick review of that documentation didn't reveal anything. Reviewing [the referenced RFC][link-4] also didn't provide any suggestions on how the hash might be calculated. I'd messed around with `openssl x509` with Kubernetes before (see [this post][xref-1] or [this post][xref-2]), so how about some trial-and-error?

Starting with what I knew, I started with decoding the certificate using `openssl`. After reading some `openssl` man pages, I arrived at this command to extract _only_ the public key of the certificate:

``` bash
openssl x509 -in /etc/kubernetes/pki/ca.crt -pubkey -noout
```

(Side note: the use of `-noout` threw me for a bit; it says "No output, just status", but what it really means is don't output anything more than what I've already told you to output with `-pubkey` or other flags.)

From there it was experimenting with `openssl dgst` to see if I could get results that matched against some known CA certificate hashes I'd captured for clusters I'd built. I wasn't having much success until I stumbled on [this Stack Overflow post][link-5], which pointed me in the direction of `openssl pkey`. That was the missing link I needed.

So, the final command is this:

``` bash
openssl x509 -in /etc/kubernetes/pki/ca.crt -pubkey -noout |
openssl pkey -pubin -outform DER |
openssl dgst -sha256
```

I shared this with my team members (sharing is caring!), and my teammate Naadir Jeewan promptly responded with an [Ansible][link-6] filter to perform the same task. This is helpful when `openssl` isn't present on the system where you need to calculate the hash. Naadir's Ansible filter is found [here][link-2] (great work, Naadir!).

There you have it. Next time you find yourself needing to calculate the CA certificate hash to use with `kubeadm`, you now have two ways of getting there (either using `openssl` or using Naadir's Ansible filter).

If you have any questions, don't hesitate [to reach out to me on Twitter][link-7]. Thanks!

**UPDATE:** Apparently, I overlooked a portion of [the official documentation for `kubeadm join`][link-3], which _does_ provide a mechanism for using `openssl` to calculate the CA cert hash. I checked the documentation back to version 1.15, and the information is there. (No idea how I missed this!) See the section titled "Token-based discovery with CA pinning" for another `openssl` command you can use to calculate the CA cert hash. Thanks to Dan Webster for pointing this out!

[link-1]: https://kubernetes.io/
[link-2]: https://gist.github.com/randomvariable/e4c43f89afec52fec0dbef6c08621249
[link-3]: https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-join/
[link-4]: https://tools.ietf.org/html/rfc7469
[link-5]: https://stackoverflow.com/questions/36163093/how-do-we-generate-a-base64-encoded-sha256-hash-of-subjectpublickeyinfo-of-an-x
[link-6]: https://www.ansible.com/
[link-7]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-06-12-examining-x509-certificates-embedded-in-kubeconfig-files.md" >}}
[xref-2]: {{< relref "2018-08-20-troubleshooting-tls-certificates.md" >}}
