---
author: slowe
categories: Information
comments: true
date: 2019-12-27T16:41:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- OVS
- AWS
- SSH
- Kubernetes
- Terraform
- macOS
- CLI
- Productivity
title: 'Technology Short Take 122'
url: /2019/12/27/technology-short-take-122/
---

Welcome to Technology Short Take #122! Luckily I _did_ manage to get another Tech Short Take squeezed in for 2019, just so all my readers could have some reading materials for the holidays. I'm kidding! No, I mean I really am kidding---don't read stuff over the holidays. Spend time with your family instead. The investment in your family will pay off in later years, trust me.<!--more-->

## Networking

* Teammate Alex Brand has [a look at VMware's new OVS-based CNI plugin, Antrea][link-6].

## Servers/Hardware

* Adam Leventhal [takes a closer look at the financial side of AWS Outposts][link-14], which was announced as generally available at this year's re:Invent.

## Security

* Bruce Schneier writes about [how some Chinese hackers are bypassing RSA software token authentication][link-19] (the title is a bit more broad, implying other forms of two-factor authentication are affected, but the article focuses on attacks against the use of RSA software tokens).
* Robin Moffatt [discusses detecting and analysing SSH attacks with ksqlDB][link-21].

## Cloud Computing/Cloud Management

* Ahmet Alp Balkan talks about [how the Knative Serving component addresses some key networking challenges within Kubernetes][link-8].
* Teri Radichel has a post with [some structural tips to better understand and utilize AWS CloudFormation][link-11].
* Geert Baeke walks readers through [deploying AKS with Nginx, External DNS, the Helm Operator, and Flux][link-12] (all using an Azure DevOps pipeline, if I read the article correctly).
* Seth Vargo walks through [how to use Terraform to setup and configure a Google Cloud Run service][link-17].
* Hashicorp recently announced a new Kubernetes integration with Vault aimed at making secrets injection easier; read about it [here][link-18].
* Here's [a list of 35 advanced tutorials to help learn Kubernetes][link-20], courtesy of Aymen El Amri (and the authors of the various tutorials, of course).

## Operating Systems/Applications

* Continuing his focus on `cloud-init`, Luc Dekens has added _four_ more articles to his site discussing this tool. Be sure to check them out ([part 2][link-2], [part 3][link-3], [part 4][link-4], and [part 5][link-5])!
* Cole Atkinson [discusses the fact that `osxfuse` is no longer open source][link-7], and the ramifications of this development on open source in general.
* Wesley David [ran into some challenges with the `UseKeychain` SSH configuration directive on macOS][link-9], and proposed a potential solution (the solution worked for him).
* I'm pretty satisfied with `bash` as my shell, but a lot of folks like `zsh`, and `zsh` is now the default shell on macOS Catalina (apparently). [This article][link-10] has a list of tips and tricks for `zsh`.
* Jessica Joy Kerr [shares her settings for working in Visual Studio Code][link-13].
* Jeff Johnson discusses [an undocumented Catalina file access change][link-16].
* [This combination][link-22] of `fzf` and `kubectl` looks interesting, but requires the use of a server-side component in order to address latency issues---which are a real killer for a CLI auto-completion sort of tool.
* Michael Gasch [shares his `tmux` setup][link-23].

## Storage

* I recently saw an article stating that VMware is going to use Minio as the object storage platform for Kubernetes, but the evidence presented in the article is anything but convincing. In fact, the _one slide_ the article uses as evidence is just an example of how Minio might be deployed on Kubernetes, and the slide also includes examples of deploying MySQL and Redis. I'm not going to link to the article because, quite frankly, I think it's just plain incorrect.

## Virtualization

* Bjoern Brundert has [a collection of resources for Project Pacific][link-1] (announced at VMworld 2019). Give it a look if you're interested in learning more.
* I recently spotted [Multipass][link-15], a virtualization solution for working with Linux instances. I haven't tried it yet, but it looks interesting for sure.

## Career/Soft Skills

* While reading something else, I found a link to James Stuber's site, and---being a productivity geek---I found [this list of his current productivity tech stack][link-24] to be interesting.

That's all for 2019---at least as far as Tech Short Takes go. Have a great weekend and great upcoming holiday, everyone! As always, don't hesitate [to reach out to me on Twitter][link-99] if you have feedback, suggestions for improvement, corrections, or if you just want to say hi!

[link-1]: http://blog.think-v.com/?p=5806
[link-2]: http://www.lucd.info/2019/12/07/cloud-init-part-2-advanced-ubuntu/
[link-3]: http://www.lucd.info/2019/12/08/cloud-init-part-3-photon-os/
[link-4]: http://www.lucd.info/2019/12/09/cloud-init-part-4-running-scripts/
[link-5]: http://www.lucd.info/2019/12/11/cloud-init-part-5-running-containers/
[link-6]: https://alexbrand.dev/post/first-look-at-antrea-a-cni-plugin-based-on-open-vswitch/
[link-7]: https://colatkinson.site/macos/fuse/2019/09/29/osxfuse/
[link-8]: https://ahmet.im/blog/knative-better-kubernetes-networking/
[link-9]: https://wesley.sh/solving-usekeychain-not-working-for-password-protected-ssh-key-logins-on-macos/
[link-10]: https://www.sitepoint.com/zsh-tips-tricks/
[link-11]: https://medium.com/cloud-security/easier-aws-cloudformation-47a30c631963
[link-12]: https://blog.baeke.info/2019/12/06/deploy-aks-with-nginx-external-dns-helm-operator-and-flux/
[link-13]: https://blog.jessitron.com/2019/12/02/do-things-right-in-vscode/
[link-14]: https://blog.usejournal.com/aws-outposts-68e78592c7f8
[link-15]: https://multipass.run/
[link-16]: https://lapcatsoftware.com/articles/macl.html
[link-17]: https://www.sethvargo.com/configuring-cloud-run-with-terraform/
[link-18]: https://www.hashicorp.com/blog/injecting-vault-secrets-into-kubernetes-pods-via-a-sidecar/
[link-19]: https://www.schneier.com/blog/archives/2019/12/chinese_hackers_1.html
[link-20]: https://medium.com/faun/35-advanced-tutorials-to-learn-kubernetes-dae5695b1f18
[link-21]: https://rmoff.net/2019/12/18/detecting-and-analysing-ssh-attacks-with-ksqldb/
[link-22]: https://github.com/bonnefoa/kubectl-fzf
[link-23]: https://mh9.codes/posts/my-tmux-setup/
[link-24]: https://jamesstuber.com/productivity-systems/
[link-99]: https://twitter.com/scott_lowe
