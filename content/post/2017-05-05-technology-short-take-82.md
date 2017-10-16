---
author: slowe
categories: Information
comments: true
date: 2017-05-05T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Ansible
- OVS
- Cisco
- LACP
- Automation
- Linux
- AWS
- OpenStack
- Microsoft
- Azure
- CentOS
- JSON
- PowerShell
- Consul
- HyperV
- VMware
- vSphere
- Fusion
title: 'Technology Short Take 82'
url: /2017/05/05/technology-short-take-82/
---

Welcome to Technology Short Take #82! This issue is a bit behind schedule; I've been pretty heads-down on some projects. That work will come to fruition in a couple weeks, so I should be able to come up for some air soon. In the meantime, here's a few links and articles for your reading pleasure.<!--more-->

## Networking

* Kristian Larsson shows [how to validate data using YANG][link-11]. Practical examples like this have really helped me better understand YANG and its relationship to structured data you might exchange with a device or service.
* There's lots of talk about applying test-driven development (TDD) principles in various automation contexts, but I like the fact that Ajay Chenampara provides some practical examples in his blog post on [applying TDD in network automation using Ansible][link-14].
* Matt Oswalt talks about how the combination of NAPALM and StackStorm enables some interesting results, including the ability to [verify configuration consistency][link-15]. StackStorm isn't something I've had the opportunity to learn/use at all, but it's on my (ever-growing) list of things to check out.
* Aaron Conole provides [an overview][link-17] of using the `ovs-dpctl` command to "program" the Open vSwitch (OVS) kernel module. It's a bit geeky, but does provide some insight into how OVS works.
* Mircea Ulinic shares [some experience around Cisco IOS-XR's buggy XML API][link-18] and the (unfortunate) terrible customer experience that resulted. Mircea's right---bugs will happen in all software (VMware NSX has had its share, for example), but the key is in how it's handled.
* Doug Youd of Cumulus has an excellent 3-part series on the use of LACP in VMware vSphere environments. It's a really good, in-depth review of the topic, the design considerations around this topic, and some of the design ramifications. Highly recommended! Check out [part 1][link-19], [part 2][link-20], and [part 3][link-21].
* Jason Edelman [reminds][link-22] folks that big changes in an industry---like fully embracing network automation, for example---often occurs as a series of smaller steps. If you're just starting your network automation journey, start small. Just be sure to start!
* There's been a fair amount of noise recently over extended BPF (eBPF) as a solution to some server-side networking challenges. [This article][link-24] gives an overview and brief introduction to eBPF. More articles are apparently planned, and I'm looking forward to reading them.

## Servers/Hardware

Nothing this time around. I'll stay alert for items to include next time!

## Security

* Apparently, the NIST (National Institute for Standards and Technology, a US government entity for all the non-US readers out there) is formulating a new set of recommendations for passwords. You can read more about the proposed changes in [this article][link-2].
* It's nice to see some folks attempting [to help tackle potential security concerns with containers][link-10]. Looks like it's early days yet for this effort, so it will be interesting to see what comes out of it.

## Cloud Computing/Cloud Management

* If you're trying to wrap your head around AWS IAM policies, I have yet to find a resource I can recommend more strongly than this article on [AWS IAM policies in a nutshell][link-1]. It's an incredibly well-written and informative article. I _strongly_ recommend using this article to help further your understanding of AWS IAM policies.
* Philipp Garbe describes [a better solution to ECS AutoScaling][link-5] that avoids scaling contention due to "competing" metrics (i.e., memory pressure but not CPU utilization, or vice versa).
* News in the OpenStack space hasn't been so good recently (Intel pulling out of OSIC, various companies laying off folks, other "pure play" companies shifting focus away from OpenStack), but here's [one architect's perspective][link-7] on what you may still want to attend the OpenStack Summit.
* Jon Schulman provides [an overview of the Microsoft Azure endpoint][link-23] included in the vRealize Automation 7.2 release.
* Craig McLuckie shares his [perspective on multi-cloud][link-26]. This is the first in a series of posts, so stay tuned for future installations.

## Operating Systems/Applications

* Ryan Kelly [shows][link-3] readers how to configure Harbor (open source container registry project) to use Amazon S3 for storage.
* YACOOS (Yet Another Container-Optimized Operating System) has gone GA; this time it's Google's (imaginatively-named) "Container-Optimized OS". You can get more details via [this Google blog post][link-4].
* Ajay Chenampara shares [his workaround][link-8] for storing GitHub credentials on Linux. (If you're using GNOME, you may find [this approach][xref-1] easier.)
* There's [a new version of CentOS Atomic Host][link-9] available. I've been playing with this a pretty fair amount recently, and hope to post some thoughts soon.
* Aaron Paxson demonstrates [how to use Ansible to work with JSON-formatted structured data][link-12]. This is pretty cool, and reminds me just how much I still have to learn about using Ansible.
* James Pettigrove shows [how to test your PowerShell code in different versions with just one workstation][link-16].
* Here's an example of [using Ansible to clone Oracle Grid infrastructure][link-25].
* [This post][link-27] illustrates how to use Consul to provide dynamic inventory for Ansible. While this seems like a potentially useful combination, I wonder whether this would be practical. It seems like environments where this may be useful might have more effective ways of getting dynamic inventory (like using one of the dynamic inventory modules for the cloud provider, for example).
* Want to keep your PowerShell Core up to date? Check out [this post][link-29] by Alan Renouf.

## Storage

* Ben Armstrong shows [how you can use Hyper-V storage resource pools][link-31] to help ease migration of workloads between data centers.

## Virtualization

* Melissa Palmer (aka "vMiss") has [a post][link-6] where she's collecting tips and tricks for VMware Fusion.
* William Lam talks about [the new ESXi Learnswitch][link-28], which helps with nested ESXi environments (among other things).
* Gabrie Van Zanten shares a hard-learned lesson on [tracking down][link-30] how a vCenter Server account keeps getting locked out.

## Career/Soft Skills

* I stumbled across this article on [lowering the barrier to entry][link-13] by Annie Hedgpeth. One thing that stuck out in particular was "lending your privilege"; that is, lending someone your expertise, your access to resources, your stamp of approval, your connections (personal network), etc. While this is often used in the context of diversity and inclusion, I think it applies in many more contexts and situations.

OK, that's it for now. Hopefully I've included something you found helpful; if so, please feel free to share a link back to this article using Twitter or your social media platform of choice. Thanks for reading!



[link-1]: http://start.jcolemorrison.com/aws-iam-policies-in-a-nutshell/
[link-2]: https://nakedsecurity.sophos.com/2016/08/18/nists-new-password-rules-what-you-need-to-know/
[link-3]: http://www.vmtocloud.com/how-to-configure-harbor-registry-to-use-amazon-s3-storage/
[link-4]: https://cloudplatform.googleblog.com/2017/04/Container-Optimized-OS-from-Google-is-generally-available.html
[link-5]: http://garbe.io/blog/2017/04/12/a-better-solution-to-ecs-autoscaling/
[link-6]: http://vmiss.net/2017/03/31/the-vmware-fusion-post/
[link-7]: https://snoopj.wordpress.com/2017/04/14/why-im-heading-to-openstack-summit-an-architect-perspective/
[link-8]: https://termlen0.github.io/2017/04/20/observations/
[link-9]: https://www.projectatomic.io/blog/2017/04/new-centos-atomic-host-with-updated-kube/
[link-10]: https://containerhardening.org/
[link-11]: https://plajjan.github.io/validating-data-with-YANG/
[link-12]: http://www.myteneo.net/blog/-/blogs/listing-iterating-and-loading-json-in-ansible-playbooks/
[link-13]: http://www.anniehedgie.com/barriers
[link-14]: https://termlen0.github.io/2017/04/12/observations/
[link-15]: https://stackstorm.com/2017/04/11/ensuring-network-configuration-consistency-stackstorm-napalm/
[link-16]: http://dxpetti.com/blog/?p=1025
[link-17]: https://developers.redhat.com/blog/2017/04/06/direct-kernel-open-vswitch-flow-programming/
[link-18]: https://mirceaulinic.net/2017-04-14-cisco-xr-xml-agent-fun/
[link-19]: https://cumulusnetworks.com/blog/vmware-cloud-design-lacp/
[link-20]: https://cumulusnetworks.com/blog/mlag-and-lacp/
[link-21]: https://cumulusnetworks.com/blog/sharing-state-between-host-and-upstream-network%E2%80%A8-lacp-3/
[link-22]: http://jedelman.com/home/self-driving-cars-and-network-automation/
[link-23]: http://www.vaficionado.com/2016/11/using-new-microsoft-azure-endpoint-vrealize-automation-7-2/
[link-24]: https://ferrisellis.com/posts/ebpf_past_present_future/
[link-25]: https://www.pythian.com/blog/cloning-oracle-grid-infrastructure-using-ansible/
[link-26]: https://blog.heptio.com/perspective-on-multi-cloud-part-1-of-3-6396caf522b5
[link-27]: https://flynnbundy.com/ansible/2016/12/04/dynamic-inventory-with-consul-and-ansible.html
[link-28]: http://www.virtuallyghetto.com/2017/04/esxi-learnswitch-enhancement-to-the-esxi-mac-learn-dvfilter.html
[link-29]: http://www.virtu-al.net/2017/03/27/powershell-core-date/
[link-30]: http://gabesvirtualworld.com/vcenter-account-locked/
[link-31]: https://blogs.msdn.microsoft.com/virtual_pc_guy/2017/05/03/using-hyper-v-resource-pools-to-ease-migration-between-different-configurations/
[xref-1]: {{< relref "2016-11-23-gnome-keyring-git-credential-fedora.md" >}}
