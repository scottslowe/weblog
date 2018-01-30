---
author: slowe
categories: Information
comments: true
date: 2017-12-08T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- VMware
- NSX
- OVN
- Automation
- AWS
- OpenStack
- Terraform
- Docker
- vSphere
- Microsoft
- Windows
- HyperV
- Vagrant
title: 'Technology Short Take 91'
url: /2017/12/08/technology-short-take-91/
---

Welcome to Technology Short Take 91! It's been a bit longer than usual since the last Tech Short Take (partly due to the US Thanksgiving holiday, partly due to vacation time, and partly due to business travel), so apologies for that. Still, there's a great collection of links and articles here for you, so dig in and enjoy.<!--more-->

## Networking

* Amanpreet Singh has a two-part series on Kubernetes networking ([part 1][link-7], [part 2][link-8]).
* Anthony Spiteri has [a brief look at NSX-T 2.1][link-9], which recently launched with support for Pivotal Container Service (PKS) and Pivotal Cloud Foundry, further extending the reach of NSX into new areas.
* Jon Benedict has [a brief article][link-13] on OVN and its integration into Red Hat Virtualization; if you're unfamiliar with OVN, it might be worth having a look.
* sFlow is a networking technology that I find quite interesting, but I never seem to have the time to really dig into it. For example, I was recently browsing the sFlow blog and came across two really neat articles. The first was on [RESTful control of Cumulus Linux ACLs][link-14] (this one isn't actually sFlow-related); the second was on [combining sFlow telemetry and RESTful APIs][link-15] for visibility and control in campus networks.
* David Gee's "network automation engineer persona" content continues; this time he tackles [some thoughts around proof-of-concepts (PoCs)][link-21].

## Servers/Hardware

* Frank Denneman (with an admittedly vSphere-focused lens) takes a look at the Intel Xeon Scalable Family in a two-part (so far) series. [Part 1][link-25] covers the CPUs themselves; [part 2][link-26] discusses the memory subsystem. Both articles are worth reviewing if hardware selection is an important aspect of your role.
* Kevin Houston provides some details on [blade server options for VMware vSAN Ready Nodes][link-29].

## Security

* The security of Kubernetes is coming under greater scrutiny, as would be expected as adoption (and attention) increases. Josselin Costanzi discusses [the security problems with Kubernetes as deployed via `kops`][link-6].
* In a somewhat backwards manner, I first stumbled upon [this list of sites that have third-party "session replay" scripts][link-17] that could potentially exfiltrate sensitive customer data. The list was quite surprising, to be honest; there were some sites on the list that I didn't expect to see. It was only after reviewing this list that I decided it would probably be a good idea to read [the associated blog post that explains the list][link-18]. Let's just say this: after reading it, I went back to reconfigure my ad-blocking and tracker-blocking plugins.

## Cloud Computing/Cloud Management

* The Cloud-Native Computing Foundation (CNCF) and the Kubernetes community introduced the Certified Kubernetes Conformance Program, and the first announcements of certification have started rolling in. First, [here's Google's announcement][link-1] of renaming Google Container Engine to Google Kubernetes Engine (making the GKE acronym much more applicable) as a result of its certification. Next, here's [an announcement on the certification of PKS][link-2] (Pivotal Container Service).
* Henrik Schmidt [writes about the `kube-node` project][link-4], an effort to allow Kubernetes to manage worker nodes in a cluster.
* Helm is a great way to deploy applications onto (into?) a Kubernetes cluster, but there are some ways you can improve Helm's security. Check out this article from Matt Butcher on [securing Helm][link-5].
* [This site][link-10] is a good collection of "lessons learned from the trenches" on running Kubernetes on AWS in production.
* I have to be honest: [this blog post][link-20] on using OpenStack Helm to install OpenStack on Kubernetes with Rook sounds like a massive science experiment. That's a lot of moving pieces!
* User "sysadmin1138" (I couldn't find a mapping to a real name, perhaps that's intentional) has a great write-up on her/his [experience with Terraform in production][link-22]. There's some great information here for those of you thinking of (or currently) using Terraform to manage production workloads/configurations.

## Operating Systems/Applications

* Michael Crosby [outlines][link-3] support for multi-client support in containerD.
* Speaking of containerD, it just [recently hit 1.0][link-11].
* This is [a slightly older post][link-12] by Alex Ellis on attachable networks, which (as I understand it) enable interoperability between declarative workloads (deployed via `docker stack deploy`) and imperative workloads (launched via `docker run`).

## Storage

* VMware's Cloud-Native group [discussed some recent updates to Project Hatchway][link-16], an effort to bring expanded support for persistent storage into cloud-native platforms such as Kubernetes. It's nice to see that CSI support is also in the mix.
* J Metz has an outstanding "deep in the weeds" discussion on [how flash memory avoids data loss][link-31]. Good stuff.

## Virtualization

* The "blurring" between VMs/hypervisors and containers/container daemons continues; for one example, see [this recent article][link-19] by Docker discussing Linux containers on Windows (LCOW).
* Alan Renouf [discusses the ability to manage][link-27] VMware Cloud on AWS (VMC) using the latest release of PowerCLI (version 6.5.4).
* William Lam (with some help from Anthony Burke) outlines [how to move ESXi hosts with LACP/LAG between vCenter Servers][link-28].
* John Slack describes [how to copy files into a Hyper-V VM using Vagrant][link-30] by using a feature of the Hyper-V provider known as the guest service interface.

## Career/Soft Skills

* Pat Bowden [discusses][link-23] the idea of learning styles, and how combining learning styles (or multiple senses) can typically contribute to more successful learning.
* I also found some useful tidbits on learning over at [The Art of Learning project website][link-24].

That's all for now (but I think that should be enough to keep you busy for a little while, at least!). I'll have another Tech Short Take in 2 weeks, though given the holiday season is nigh upon us it might be a bit light on content. Until then!



[link-1]: https://cloudplatform.googleblog.com/2017/11/introducing-Certified-Kubernetes-and-Google-Kubernetes-Engine.html
[link-2]: https://blogs.vmware.com/cloudnative/2017/11/13/vmware-pivotal-container-service-achieves-kubernetes-certification/
[link-3]: https://blog.mobyproject.org/containerd-namespaces-for-docker-kubernetes-and-beyond-d6c43f565084
[link-4]: https://thenewstack.io/kube-node-let-k8s-cluster-auto-manage-nodes/
[link-5]: http://technosophos.com/2017/11/20/securing-helm.html
[link-6]: https://medium.com/devopslinks/security-problems-of-kops-default-deployments-2819c157bc90
[link-7]: https://medium.com/@ApsOps/an-illustrated-guide-to-kubernetes-networking-part-1-d1ede3322727
[link-8]: https://medium.com/@ApsOps/an-illustrated-guide-to-kubernetes-networking-part-2-13fdc6c4e24c
[link-9]: https://anthonyspiteri.net/nsx-bytes-whats-new-in-nsx-t-2-1/
[link-10]: http://kubernetes-on-aws.readthedocs.io/en/latest/admin-guide/kubernetes-in-production.html
[link-11]: https://github.com/containerd/containerd/releases
[link-12]: https://blog.alexellis.io/docker-stacks-attachable-networks/
[link-13]: http://captainkvm.com/2017/12/introduction-ovn-red-hat-virtualization/
[link-14]: http://blog.sflow.com/2017/11/restful-control-of-cumulus-linux-acls.html
[link-15]: http://blog.sflow.com/2017/09/real-time-visibility-and-control-of.html
[link-16]: https://blogs.vmware.com/cloudnative/2017/12/06/updates-project-hatchway-kubecon-2017/
[link-17]: https://webtransparency.cs.princeton.edu/no_boundaries/session_replay_sites.html
[link-18]: https://freedom-to-tinker.com/2017/11/15/no-boundaries-exfiltration-of-personal-data-by-session-replay-scripts/
[link-19]: https://blog.docker.com/2017/11/docker-for-windows-17-11/
[link-20]: https://cloudlearning.pro/2017/11/28/going-all-in-openstack-on-kubernetes-with-rook-part-1/
[link-21]: http://ipengineer.net/2017/11/network-automation-engineer-persona-proof-of-concepts/
[link-22]: http://sysadmin1138.net/mt/blog/2017/11/terraforming-in-prod.shtml
[link-23]: http://onlinelearningsuccess.org/learning-styles/
[link-24]: http://theartoflearningproject.org/
[link-25]: http://frankdenneman.nl/2017/09/26/vsphere-focused-guide-intel-xeon-scalable-family/
[link-26]: http://frankdenneman.nl/2017/10/03/vsphere-focused-guide-intel-xeon-scalable-family-memory-subsystem/
[link-27]: http://www.virtu-al.net/2017/11/29/using-powercli-manage-vmware-cloud-aws/
[link-28]: https://www.virtuallyghetto.com/2017/11/moving-esxi-hosts-with-lacplag-between-vcenter-servers.html
[link-29]: http://bladesmadesimple.com/2017/10/blade-server-options-for-vmware-vsan-readynode/
[link-30]: https://blogs.technet.microsoft.com/virtualization/2017/07/18/copying-files-into-a-hyper-v-vm-with-vagrant/
[link-31]: https://jmetz.com/2017/09/storage-how-does-flash-memory-avoid-data-loss/
