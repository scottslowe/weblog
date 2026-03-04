---
author: slowe
categories: Tutorial
comments: true
date: 2026-03-04T17:30:00-05:00
tags:
- CLI
- Linux
- Kubernetes
- Networking
- Security
- Encryption
title: Using Mitmproxy to Observe kubectl Traffic
url: 2026/03/04/using-mitmproxy-to-observe-kubectl-traffic
---

When I first started learning Kubernetes, I had the idea that observing the network traffic between a client system using `kubectl` and the Kubernetes API Server would be a useful thing to do. The source of the idea is unclear; I am unsure why I thought this would be useful as a learning tool. Regardless, I continued on with learning Kubernetes and never really pursued this idea---until this week. I found it can be a useful troubleshooting technique, but I will leave it up to you to determine if it is a useful learning technique. In this post, I will show you how to observe `kubectl` traffic using `mitmproxy`.<!--more-->

This technique is inspired by/informed by [Ahmet Alp Balkan's similarly-named blog post from 2019][link-4]. Unfortunately, I found the instructions there to be incomplete (most likely just due to the passage of time and continued evolution of the tools involved).

I used the following tools and environments in my testing:

* The tests were conducted on a Linux system running [Ubuntu][link-2] 24.04.4. The commands should work similarly on macOS.
* [Mitmproxy][link-1] was installed from the Ubuntu repositories using `apt`.
* `kubectl` version 1.33.3 was used to communicate to a self-managed cluster on AWS (in other words, _not_ Amazon EKS) running [Kubernetes][link-3] 1.32.9. The cluster was bootstrapped using `kubeadm`. I wouldn't expect any major/significant differences with other versions of `kubectl` or Kubernetes.
* I was using a client certificate to authenticate to Kubernetes. It's unclear to me how this might work---if it works at all---with alternate authentication mechanisms.

## Prepare Client Certificates

Before you can start mitmproxy, you'll first need to extract the client certificates from the Kubeconfig file. A couple of ways exist to do this; [a blog post of mine from 2022][xref-1] contains what I believe is the _easiest_ way. The method involves `yq` (to extract information from the Kubeconfig) and `base64` (to decode the client certificate and client key). Refer to the linked blog post for full details.

1. First, extract the client certificate (adjust the `users[0]` as needed based on the Kubeconfig file):

    ```bash
    yq '.users[0].user.client-certificate-data' < kubeconfig | base64 -d >> client-cert.pem
    ```

2. Next, extract the client key:

    ```bash
    yq 'users[0].user.client-key-data'< kubeconfig | base64 -d >> client-cert.pem
    ```

## Running Mitmproxy and Watching Traffic

Now that you have the client certificate and key in hand, you're ready to launch mitmproxy and observe some traffic. Mitmproxy requires a few specific configuration flags for this use case:

* You'll need to disable HTTP/2 support with `--set http2=false`.
* Mitmproxy must be configured with the client certificate you extracted above (using `--set client_certs=client-cert.pem`).
* In this particular case, since the cluster CA isn't trusted, mitmproxy also has to be told not to verify the upstream certificate with `--set ssl_insecure=true`.

Putting all this together, the full command looks like this:

```bash
mitmproxy -p 5000 --set ssl_insecure=true --set http2=false --set client_certs=client-cert.pem
```

Then, in a separate window, run your `kubectl` command:

```bash
HTTPS_PROXY=:5000 kubectl get po -A --insecure-skip-tls-verify
```

You should see the traffic pop up in the window running mitmproxy, and can review the traffic flow(s) in detail.

As explanation: the `HTTPS_PROXY` part redirects traffic through the instance of mitmproxy you just launched, and `--insecure-skip-tls-verify` tells `kubectl` not to verify the upstream certificate---which it can't do because it's seeing mitmproxy's certificate, _not_ the cluster's certificate.

There you go---now you can observe traffic between `kubectl` and the Kubernetes API server. As I mentioned at the start of this post, this technique is useful for troubleshooting. I did not find it useful as a learning tool, but others might.

I hope you find this useful. Thanks to Ahmet for setting me down the right path! If you have questions, feel free to reach out. I'm available on social platforms ([X/Twitter][link-5], [Mastodon][link-6], [Bluesky][link-7], and [LinkedIn][link-8]), or you can drop me an email. I'd be happy to help if I'm able.

[link-1]: https://www.mitmproxy.org/
[link-2]: https://ubuntu.com/
[link-3]: https://kubernetes.io/
[link-4]: https://ahmet.im/blog/kubectl-man-in-the-middle/
[link-5]: https://x.com/scott_lowe
[link-6]: https://fosstodon.org/@scottslowe
[link-7]: https://bsky.app/profile/scottslowe.bsky.social
[link-8]: https://www.linkedin.com/in/scottslowe
[xref-1]: {{< relref "2022-08-09-revisiting-x509-certificates-in-kubeconfig-files.md" >}}
