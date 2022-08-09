---
author: slowe
categories: Explanation
comments: true
date: 2022-08-09T08:55:00-06:00
tags:
- CLI
- Kubernetes
- YAML
title: Revisiting X.509 Certificates in Kubeconfig Files
url: /2022/08/09/revisiting-x509-certificates-in-kubeconfig-files/
---

In 2018, I wrote an article on [examining X.509 certificates embedded in Kubeconfig files][xref-1]. In that article, I showed one way of extracting client certificate data from a Kubeconfig file and looking at the properties of the client certificate data. While there's nothing technically wrong with that article, since then I've found another tool that makes the process a tad easier. In this post, I'll revisit the topic of examining embedded X.509v3 certificates in Kubeconfig files.<!--more-->

The [tool that I've found is `yq`][link-1], which is an _incredibly_ useful tool when it comes to parsing YAML (much in the same way that [`jq` is an _incredibly_ useful tool][xref-2] when it comes to parsing JSON). I should probably write some sort of introductory post on `yq`.

In any case, you can use `yq` to replace the `grep` plus `awk` combo outlined in [my earlier article][xref-1] on examining certificate data in Kubeconfig files. Instead, to pull out _only_ the client certificate data, just use this `yq` command (you _did_ know that Kubeconfig files are YAML, right?):

    yq '.users[0].user.client-certificate-data' < ~./kube/config

(Of course, this command assumes your Kubeconfig file is named `config` in the `~/.kube` directory; adjust the command as necessary based on your specific environment.)

The `.users[0]` portion of the `yq` command refers to the first user in the list of users in the referenced Kubeconfig file. If there's more than one and you're interested in seeing client certificate data for a different user, you'll need to adjust that index.

From there, you can decode the Base64-encoded content and then pipe it to OpenSSL, just as described in the other post, to get a look at the actual certificate encoded within the Kubeconfig file. Here's the full command:

    yq '.users[0].user.client-certificate-data' < ~/.kube/config | base64 -D | openssl x509 -text

(Note that this command is for macOS; I believe you'll need to use a `base64 -d` on GNU/Linux systems.)

I hope this is helpful to someone. Feel free to reach out to [me on Twitter][link-2] if you have any questions or any feedback. You can also find me in a number of different Slack communities, and you're welcome to contact me there as well.

[link-1]: https://github.com/mikefarah/yq
[link-2]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-06-12-examining-x509-certificates-embedded-in-kubeconfig-files.md" >}}
[xref-2]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
