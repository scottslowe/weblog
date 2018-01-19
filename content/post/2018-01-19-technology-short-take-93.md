---
author: slowe
categories: Information
comments: true
date: 2018-01-19
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- OpenStack
- VMware
- NSX
- Ansible
- Linux
- AWS
- Microsoft
- Productivity
title: 'Technology Short Take 93'
url: /2018/01/19/technology-short-take-93/
---

Welcome to Technology Short Take 93! Today I have another collection of data center technology links, articles, thoughts, and rants. Here's hoping you find something useful!<!--more-->

## Networking

* Matt Klein [breaks down service mesh components][link-12], using terms networking professionals know well, like "data plane" and "control plane." Those familiar with networking concepts will identify well with the ideas in this article, I think.
* This is pretty cool: [a WYSIWYG topology designer for VMware NSX][link-15]. Neat!
* Daniel Hertzberg provides [an overview of Salt integration in Arista EOS][link-18].
* Barry Peddycord shares [an update on Ansible support for the Cumulus Linux NCLU][link-26].

## Servers/Hardware

Nothing this time around. Feel free to [hit me up on Twitter][link-29] if you have links you think I should include next time!

## Security

* Detectify has [a nice, in-depth write-up on AWS S3 access controls][link-28]. Given the recent plethora of data breaches that have been attributed to misconfigured S3 buckets, now might be a good time to read this post.

## Cloud Computing/Cloud Management

* The VMware Cloud-Native Apps group has [a breakdown of some of the new features/functionality][link-1] found in the 1.9 release of Kubernetes.
* Via Maish Saidel-Keesing, I saw [this post][link-4] about 10 open source Kubernetes tools for highly effective SRE and Ops teams.
* Richard Boswell has a series going on VMware Integrated OpenStack (VIO) and resource schedulers that is (so far) pretty good. Check out [part 1][link-6] and [part 2][link-7].
* Daniel Sanche has [a "Kubernetes 101" post][link-11] that might be worth reviewing if you're (somewhat) new to Kubernetes.
* And while we're on the topic of getting started with Kubernetes, you might find [Chris Short's post on that topic][link-22] helpful as well.
* Robin Vasan (via The New Stack) [talks a bit][link-20] about how various forces are shaping the software industry. There are a number of useful observations and takeaways from this article; one that jumps to mind is Vasan's recommendation that "architects should actively consider finding abstractions that give them portability" when it comes to selecting/utilization cloud services. (I could insert a rant here about lock-in, but I'll abstain...)
* Kynan Rilee has a nice post on [node affinity in Kubernetes scheduling][link-27].

## Operating Systems/Applications

* PowerShell Core 6.0 is [now GA and supported by Microsoft][link-2].
* Consul is a distributed key-value store that I've discussed a few times here on the site. One issue that has been difficult to address is boostrapping Consul in a cloud environment; [this slightly older article][link-9] addresses how new functionality (as of v0.7.1) can use cloud metadata to handle boostrapping a Consul cluster---and help address scaling that cluster up or down. And while I'm on the topic of Consul, you may also find [this article on using Consul with a service mesh][link-10] to be somewhat helpful as well (although this one isn't as in-depth as the previous article).
* Ariya Hidayat shows [how to run Debian under WSL][link-13] (Windows Subsystem for Linux).
* [This is a tool][link-21] I'd like to take for a spin as soon as I get a chance.
* I'm glad to see someone tackle the "microservices are the answer" mindset that seems to be going around these days. Check out [Dave Kerr's post][link-23]. (He also includes links to more in-depth reading on related topics.)
* Mark Peek [talks about Dispatch][link-25], the open source serverless framework VMware recently released.

## Storage

* Klaus Aschenbrenner shares [how he designed his VMware vSAN-based home lab][link-14].

## Virtualization

* William Lam [talks a bit about cross-vCenter cloning][link-5] with vSphere 6.0 (or higher). He points out that this also works to/from VMware Cloud on AWS.
* Brandon Lee discusses [rolling back the VMware Meltdown and Spectre patches][link-3].
* I like Vagrant, but it's nice to see [a bit of competition in this space][link-8].
* Karim Boumedhel [takes a look at KubeVirt][link-17], which aims to bring management of VMs into Kubernetes. This is an interesting project that I've been tracking for a while now.
* Although [this blog post][link-19] name-drops NSX and is published on VMware's Network Virtualization blog, it's really more about VMware Cloud on AWS (which is OK, that's an interesting topics to lots of folks these days).

## Career/Soft Skills

* I really appreciated Jérôme Petazzoni's frank look at [how he reclaimed his productivity][link-16] in the face of various challenges. There are some good tips here.
* I found this article on [five trends to avoid when founding a startup][link-24] (no, I'm not planning to found a startup!) interesting reading. The article helps me see companies in a different light and judge/evaluate them by different criteria, which (IMHO) is generally pretty helpful.

OK, that's all I have this time around. Check back in a couple weeks for the next Technology Short Take!



[link-1]: https://blogs.vmware.com/cloudnative/2017/12/18/kubernetes-1-9-expanded-ecosystem-workloads-api-storage-visibility/
[link-2]: https://blogs.msdn.microsoft.com/powershell/2018/01/10/powershell-core-6-0-generally-available-ga-and-supported/
[link-3]: https://www.virtualizationhowto.com/2018/01/roll-back-vmware-meltdown-and-spectre-microcode-patch/
[link-4]: https://abhishek-tiwari.com/10-open-source-tools-for-highly-effective-kubernetes-sre-and-ops-teams/
[link-5]: https://www.virtuallyghetto.com/2018/01/cross-vcenter-clone-with-vsphere-6-0.html
[link-6]: http://www.revolutionlabs.net/2017/10/30/vio_resource_schedulers_part1.html
[link-7]: http://www.revolutionlabs.net/2018/01/17/vio_resource_schedulers_part2.html
[link-8]: https://blog.kchung.co/mech-vagrant-with-vmware-integration-for-free/
[link-9]: https://www.hashicorp.com/blog/consul-auto-join-with-cloud-metadata
[link-10]: https://www.hashicorp.com/blog/smart-networking-with-consul-and-service-meshes
[link-11]: https://medium.com/google-cloud/kubernetes-101-pods-nodes-containers-and-clusters-c1509e409e16
[link-12]: https://blog.envoyproxy.io/service-mesh-data-plane-vs-control-plane-2774e720f7fc
[link-13]: https://ariya.io/2017/03/debian-on-windows-via-wsl
[link-14]: https://www.sqlpassion.at/archive/2018/01/15/how-i-designed-my-vmware-vsan-based-home-lab/
[link-15]: https://www.codensx.com/single-post/2018/01/05/Autopology---WYSIWYG-Designer-for-NSX
[link-16]: http://jpetazzo.github.io/2017/12/24/productivity-depression-kanban-emoji/
[link-17]: https://blog.openshift.com/a-first-look-at-kubevirt/
[link-18]: https://eos.arista.com/arista-salt-integration/
[link-19]: https://blogs.vmware.com/networkvirtualization/2017/12/vmware-sddc-nsx-expands-aws.html/
[link-20]: https://thenewstack.io/creative-software-destruction-new-presentation-layer/
[link-21]: https://github.com/GoogleCloudPlatform/container-diff
[link-22]: https://chrisshort.net/kubernetes-getting-started/
[link-23]: http://www.dwmkerr.com/the-death-of-microservice-madness-in-2018/
[link-24]: http://www.fast.ai/2018/01/08/startups/
[link-25]: https://blogs.vmware.com/opensource/2018/01/12/dispatch-project-open-source-serverless-framework/
[link-26]: https://cumulusnetworks.com/blog/cumulus-linux-ansible-now-easier-ever/
[link-27]: https://medium.com/kokster/scheduling-in-kubernetes-part-1-node-affinity-b77c97556424
[link-28]: https://labs.detectify.com/2017/07/13/a-deep-dive-into-aws-s3-access-controls-taking-full-control-over-your-assets/
[link-29]: https://twitter.com/scott_lowe
