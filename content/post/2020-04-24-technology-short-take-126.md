---
author: slowe
categories: Information
comments: true
date: 2020-04-24T19:00:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- BGP
- AWS
- Automation
- Ubuntu
- Kubernetes
- CAPI
- Azure
- Pulumi
- Terraform
- SSH
- vSphere
title: 'Technology Short Take 126'
url: /2020/04/24/technology-short-take-126/
---

Welcome to Technology Short Take #126! I meant to get this published last Friday, but completely forgot. So, I added a couple more links and instead have it ready for you today. I don't have any links for servers/hardware or security in today's Short Take, but hopefully there's enough linked content in the other sections that you'll still find something useful. Enjoy!<!--more-->

## Networking

* Here's [a primer][link-2] on hot potato routing vs. cold potato routing. 'Nuff said.
* This isn't necessarily networking-related, but it does pertain to a very well-known content creator who specializes in networking. Ivan Pepelnjak recently announced that he's [migrated his site over to Hugo][link-6].
* Andreas Wittig takes a look at some [advanced AWS networking pitfalls to avoid][link-11].
* Josh VanDeraa shows [how to use Netmiko, NTC-Templates, Telegraf, Prometheus, and Grafana for monitoring VPN infrastructure][link-17] (a more critical piece of infrastructure these days given all the remote work going on).
* James McClay has [a post][link-32] talking about the use of BIND9 on Ubuntu 18.04 in an OSPF-routed lab topology using Cisco IOSv and Free Range Routing (FRR).

## Servers/Hardware

Nothing this time around!

## Security

I don't have anything to include this time, but I'll stay alert for content I can include next time.

## Cloud Computing/Cloud Management

* Caleb Lloyd [discusses][link-3] how he believes CustomResourceDefinitions (CRDs) are responsible for Google removing the free control plane for GKE clusters.
* My colleague Dustin Scott has a post up on his new blog that takes readers through the process of [provisioning an air-gapped Kubernetes cluster using Cluster API for vSphere (CAPV)][link-9]. Dustin has done a good job of including some useful details in this post.
* I recently stumbled across this regular list of [AWS open source news and updates][link-10] by Ricardo Sueiras.
* Steven Wade tackles the challenge of [staying ahead of Kubernetes API deprecations][link-12], leveraging `conftest` and Rego policies.
* Alastair Cooke's [post regarding no VMotion on AWS][link-15] initially struck me as simplistic, but after reading it a second time I caught the deeper meaning that I think Alastair was trying to get across. It is summed up, for me, in this quote: "The crucial architectural difference here is that vSphere wants us to stop thinking about individual physical servers, and AWS wants us to stop thinking about individual VMs." It's not just about VMotion; it's about the bigger architectural shifts that need to be made.
* Matt Bagnara, another colleague on my team, recently wrote a post on [Jenkins X on Kubernetes][link-19] (with a little Cluster API thrown in).
* William Lam has [a list of useful interactive and graphical UI tools for Kubernetes][link-20].
* Nick Korte chronicles [a newcomer's voyage into Azure Functions and Function Apps][link-21].
* Joe Duffy recently [wrote a post][link-22] on Pulumi's new "policy as code" functionality (which was experimental before [the Pulumi 2.0 launch today][link-29]) and using that for enforcing tagging on AWS resources.
* Romain Decker joins the static site club, and has started a four-part series on how he hosts his Hugo-powered site on AWS with S3, CloudFront, and Lambda. Only two posts are live so far ([part 1][link-30] and [part 2][link-31]), but this is shaping up to be a great series.

## Operating Systems/Applications

* Josh Reichardt shows readers a neat trick for [making shell scripts idempotent using Terraform][link-7]. I also really liked [his post on text processing tools][link-8].
* Cody De Arkland [exposes the anatomy of his macOS terminal][link-14].
* Brice Dereims gives a walkthrough of [using Code Stream for continuous deployment][link-16].
* Bryan Culver [explains][link-18] how to use a monorepo (a repository containing multiple applications/services) of Docker images with `make` and GitHub Actions.
* Simon Bernier St-Pierre [talks about `kubie`][link-25], a tool created to---in Simon's words---"improve my day to day life at work". Basically, it's a companion to `kubectl` that helps manage Kubernetes contexts, but in a different way than more well-known tools like `kubectx`. Personally, I prefer [`ktx`][link-26].
* Rahul Joshi has a four-part series on "Docker for Noobs" (start [here][link-27] with part 1; the other parts in the series are linked from there).
* Vincent Bernat talks about [safer SSH agent forwarding][link-28].

## Storage

* Cody Hosterman, of Pure Storage, [takes a look][link-13] at Pure Storage's support of the recently-released vSphere 7.
* Chris Mellor [reports][link-24] on something of a "scandal" wherein Western Digital is selling drives that use shingled magnetic recording (SMR) in use cases where that technology can cause noticeable performance problems.

## Virtualization

* Frank Denneman follows-up his post on initial placement of vSphere Pods with an article on [scheduling vSphere Pods][link-1] (the link to the initial placement article was in [Technology Short Take 125][xref-1]).
* Stijn Vermoesen describes what's involved in installing the vRealize Build Tools fling ([part 1][link-4] and [part 2][link-5]).

## Career/Soft Skills

* I recently learned about [the Johari window][link-23], an aid for understanding the self. It got me thinking about what my blind spots might be.

That's all for now. If you have questions, comments, or suggestions for how I can improve the Technology Short Take series (or this site in general), I welcome all feedback. Feel free to contact [me on Twitter][link-99], hit me up in any one of a number of Slack channels (Kubernetes, CNCF, Pulumi, to name a few), or drop me an e-mail (my address isn't too hard to find).

[link-1]: https://frankdenneman.nl/2020/03/20/scheduling-vsphere-pods/
[link-2]: http://bgphelp.com/2017/04/25/hot-potato-vs-cold-potato-routing/
[link-3]: https://caleblloyd.com/software/crds-killed-free-kubernetes-control-plane/
[link-4]: https://blog.stijnvermoesen.be/2020/03/installing-vrealize-build-tools-part-1/
[link-5]: https://blog.stijnvermoesen.be/2020/03/installing-vrealize-build-tools-part-2/
[link-6]: https://blog.ipspace.net/2020/03/ipspace-blog-runs-on-hugo.html
[link-7]: https://thepracticalsysadmin.com/idempotent-shell-scripts-with-terraform/
[link-8]: https://thepracticalsysadmin.com/untangling-various-text-processing-cli-tools/
[link-9]: https://ihazcloudthings.xyz/posts/2/
[link-10]: https://blog.beachgeek.co.uk/posts/aws-open-source-news-and-updates-no-13-46fg/
[link-11]: https://cloudonaut.io/advanved-aws-networking-pitfalls-that-you-should-avoid/
[link-12]: https://medium.com/@swade1987/deprek8ion-staying-ahead-of-k8s-api-deprecations-a378aed8d0a4
[link-13]: https://www.codyhosterman.com/2020/04/vsphere-7-and-pure-storage-is-here/
[link-14]: https://www.thehumblelab.com/anatomy-of-my-macos-terminal/
[link-15]: http://demitasse.co.nz/2020/03/aws-surprises-no-vmotion/
[link-16]: https://blog.cloud-garage.net/continuous-deployment/
[link-17]: http://blog.networktocode.com/post/using_python_and_telegraf_for_metrics/
[link-18]: https://www.bryanculver.com/2020/03/30/docker-build-monorepo-github.html
[link-19]: https://bagnaram.github.io/blog/2020/03/23/capa
[link-20]: https://www.virtuallyghetto.com/2020/04/useful-interactive-terminal-and-graphical-ui-tools-for-kubernetes.html
[link-21]: http://blog.thenetworknerd.com/2020/02/28/a-first-voyage-into-azure-functions-and-function-apps/
[link-22]: https://www.pulumi.com/blog/automatically-enforcing-aws-resource-tagging-policies/
[link-23]: http://www.xplaner.com/johariwindow/
[link-24]: https://blocksandfiles.com/2020/04/14/wd-red-nas-drives-shingled-magnetic-recording/
[link-25]: https://blog.sbstp.ca/introducing-kubie/
[link-26]: https://github.com/heptiolabs/ktx
[link-27]: https://rjdesignz.com/development/docker-for-noobs-part-1-what-is-docker/
[link-28]: https://vincent.bernat.ch/en/blog/2020-safer-ssh-agent-forwarding
[link-29]: https://www.pulumi.com/blog/pulumi-2-0/
[link-30]: https://cloudmaniac.net/wordpress-to-hugo-migration-1-why/
[link-31]: https://cloudmaniac.net/wordpress-to-hugo-migration-2-hosting/
[link-32]: https://www.questioncomputer.com/bind9-dns-server-on-ubuntu-18-04-linux-w-cisco-routing/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2020-03-20-technology-short-take-125.md" >}}
