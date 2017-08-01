---
author: slowe
categories: Information
comments: true
date: 2015-03-10T13:11:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- Git
- NSX
- Cisco
- VXLAN
- OpenStack
- Linux
- Puppet
- VMware
- vSphere
- Docker
title: 'Technology Short Take #49'
url: /2015/03/10/technology-short-take-49/
---

Welcome to Technology Short Take #49 (also known as Distraction-as-a-Service)! I have here for your reading pleasure an eclectic collection of links and articles from around the web, focusing on data center-related technologies. Here's hoping you find something useful. Bring on the content!

## Networking

* I love this post from Matt Oswalt on [five next-gen skills for networking pros][link-1]. I highly recommend you read the entire post, but in short the five skills Matt recommends are software skills (which includes configuration management and software development tools like [Git][link-2]), Linux, deep protocol knowledge, hypervisor and container networking, and IPv6. What's really interesting to me is the (completely uncoordinated) alignment between Matt's list of skills for networking pros and the list I provided to virtualization pros in [a series of recent VMUG presentations][link-3] (refer to slide 15, but the list is software development basics, Linux, automation/orchestration, and public cloud services). What does this mean? It tells me that some skills---specifically, Linux, automation/configuration management, software development concepts---are going to be _essential_ for all new IT pros in the near future. If you want to stay relevant, regardless of your current "silo," it's time to evolve.
* Carl Niger has a write-up on [the introduction of the Nexus 1000v BGP control plane][link-5], which uses BGP to help VXLAN figure out which MAC addresses live at which VTEPs. His article also provides a quick look at some of the configuration commands involved.
* I don't think I've mentioned this before; if I have, please forgive me. Anthony Burke (disclaimer: Anthony works for VMware in the NSBU) has a post on [how to use Python to automate the bulk creation of firewall rules][link-11] for use with the VMware NSX distributed firewall.
* You may have heard about the "6-pack", Facebook's new open modular hardware switch introduced recently ([here's a post on it][link-14]). With the introduction of 6-pack (as with Wedge, Facebook's top-of-rack switch before it), many people have proclaimed that Facebook is now competing directly with Cisco. Not everyone agrees, though---here's [Steven Noble's take][link-13] on the introduction of 6-pack and why this is pushing the networking industry forward.
* James (Just James), author at The Technical Blog of James, shared [his thoughts][link-18]---both good and bad---on Cumulus Networks and treating the switch as a standard GNU/Linux server.
* There's been a lot of talk about using configuration management tools like Puppet, Chef, Salt, and Ansible to manage networking devices, but (at least as far as I can tell) there haven't been a lot of practical examples. That seems to be changing now---here's an example of [using Ansible to set up OSPF in a test environment][link-25].
* Dmitri Kalintsev (disclaimer: he works for the NSBU at VMware, as I do) has a write-up on the [different control plane modes for VXLAN in VMware NSX][link-26]. It's a pretty detailed article with lots of useful information.

## Servers/Hardware

* This news came out of Mobile World Congress (MWC) 2015: Ericsson has [announced a new "hyperscale" hardware platform][link-29] based on Intel's Rack Scale Architecture (RSA). With all the focus on NFV, it's interesting to see vendors like Ericsson---whose business has been traditionally hardware-based---shift into "new" platforms in a bid to retain control of their customer base. I guess this underscores the difficulty for hardware-based companies to shift to a more software-based business.

## Security

* As we evolve towards more automated/orchestrated environments, it's important we don't leave security behind. Here's a post by Grant Orchard on [using VMware NSX's security groups to protect workloads deployed via vCAC][link-30] (now vRA).

## Cloud Computing/Cloud Management

* If you want to get a rough feel for how some of the OpenStack projects are changing from Juno to Kilo, this post on [12 highlights from the OpenStack roadmap][link-8] might be a good start.
* Heard of Kubernetes, but not really sure what it is or what it does? Afraid to ask that question because...well, doesn't _everyone_ know about Kubernetes? No fear, Google has a post on [everything you wanted to know about Kubernetes but were afraid to ask][link-15]. You're welcome.
* Still trying to wrap your head around OpenStack's use (when using the Open vSwitch plugin) of OVS+Linux bridges+IPtables? You might find [this article][link-22] by Venu Murthy helpful.
* Most people aren't really interested in the details of a given cloud management platform's APIs, but if you _are_ interested, Matt Curry takes a walk through [some of the OpenStack APIs by spinning up a server on Rackspace][link-23]. Readers seeking a bit more detail on the APIs should find this write-up useful.
* Based on [this article][link-24], it looks like filesystem quiescing and the ability to take consistent snapshots might be coming in OpenStack Kilo.

## Operating Systems/Applications

* Per [my list of 2015 projects][xref-1], I've been investigating other configuration management systems (I've primarily focused my efforts on Puppet until now). I found [this post comparing Ansible and Salt][link-4] to be quite helpful, particularly given some of the comparisons with Puppet.
* [NixOS][link-10] seems to be an interesting re-think of a Linux distribution. Sure, it has atomic upgrades and rollbacks, like CoreOS, Ubuntu Snappy Core, and Project Atomic. But it also uses a completely declarative approach to the entire OS build, allowing users to define the exact NixOS build they desire in a NixOS-specific build language (somewhat of a domain-specific language, a DSL). While this offers some power for those who understand it, I wonder if this won't make it a bit too complicated to see broad adoption.
* Puppet Labs recently blogged about [the new VMware vRealize Orchestrator plugin for Puppet][link-12]. The plugin allows users to bootstrap the Puppet agent as VMs are created and provisioned. It sounds pretty handy if you're a combined VMware-Puppet shop.
* You're probably tired of hearing about containers, but for those that aren't...Michael Crosby has [a pretty detailed write-up on container internals][link-19], complete with code examples. Nifty.
* Much is said about how Docker enables applications to be moved about from Linxu distribution to Linux distribution, but I don't think that complete kernel independence has been achieved yet. [This blog post][link-20] highlights just one example.
* In the event you like to keep your eyes on not only where the industry is headed now but also to where the industry might be headed next, [this article][link-21] by Darren Rush talks about unikernels and immutable infrastructure as possible "next steps" in the evolution of the cloud.
* Having recently started exploring Ansible, I liked this article by Jason Edelman on [how to generate Markdown documents for Ansible modules][link-27]. That's pretty handy.
* Brent Salisbury has a very handy write-up of some [Docker command-line one-liners][link-28]. I suspect this is an article I'll be referencing frequently. I'm also looking forward to spending a bit of time working with SocketPlane in the near future.

## Storage

Nothing this time around, but I'll keep my eyes peeled for content to include in future posts!

## Virtualization

* This article's getting a bit old, but I thought it might be valuable to include here nevertheless. Steve Jin, who used to work at VMware and is now an independent consultant, has a write-up on [some powerful hacks combining vim-cmd and shell commands][link-6].
* Kamau Wanguhu published a set of instructions for [replacing the NSX Manager certificate][link-7].
* Here's [a nice dive into the vSphere APIs for IO Filtering][link-9] (VAIO, as the acronym-happy like to call it).
* Luc Dekens [recently announced DRSRule][link-16], a PowerCLI module for more easily managing DRS rules in VMware vSphere environments. While it's very cool that Luc and others wrote this module, I also find it very cool that the module is [hosted on GitHub][link-17].
* [Via Eric Sloof][link-34], here's a link to a white paper discussing deployment considerations around vCenter Server 6.0. Definitely worth a read for all you architects out there that are gearing up for a vSphere 6.0 upgrade in your environment.
* If you're a Hyper-V gal/guy, you should definitely be following [Ben Armstrong's blog][link-35]. He's publishing lots of informative articles.
* If you're thinking of expanding your VMware environment into VMware vCloud Air, Alan Renouf has a great walk-through on [using PowerCLI to retrieve VM metrics from vCloud Air][link-36].

## Career/Soft Skills

* I saw [this post][link-31] by Martin Glassborow (aka "Storagebod"), wherein he talks about three types of folks: the daredevils to come up with crazy new ideas, the implementers/finishers to take those crazy ideas and make them the "new normal," and the folks to run the "new normal" now that it has become boring. This is strikingly similar to [Simon Wardley's pioneers/settlers/town planners model][link-32] and Cringely's commandos/infantry/policy model (described [here][link-33]). (I'm personally a fan of the commandos/infantry/police model, myself.) I mentioned this here because I think it's important for us to understand where we fit in these models---are we commandos, or are we infantry? Are we police? There's no right or wrong answer; some people are better suited to certain roles. The reason it's important for us to know the answer, though, is that it helps us understand if we are in the right role for us. Are you unhappy in your current role? Perhaps you're being asked to be a police officer when what you're really best suited for is infantry (or even a commando). Anyway, it's something to think about.

Well, this post turned out to be quite hefty (over 1600 words!) Sorry for the length; there's just so much good content out there that I want to share with my readers. In any case, thanks for sticking around to read the whole post.


[link-1]: http://keepingitclassless.net/2015/02/five-next-gen-net-skills/
[link-2]: http://www.git-scm.com/
[link-3]: https://speakerdeck.com/slowe/closing-the-cloud-skills-gap
[link-4]: http://club.black.co.at/log/posts/2014-10-13-progress-monday-2/
[link-5]: http://comeroutewithme.com/2014/10/04/1000v-bgp-vxlan-control-plane/
[link-6]: http://www.doublecloud.org/2013/12/powerful-hacks-with-esxi-vim-cmd-command-together-with-shell-commands/
[link-7]: http://www.borgcube.com/blogs/2014/10/nsx-replace-manager-certificate/
[link-8]: http://opensource.com/business/15/1/openstack-overviews-program-technical-leads
[link-9]: http://blogs.vmware.com/vsphere/2015/02/vaio_filters.html
[link-10]: http://nixos.org
[link-11]: http://networkinferno.net/bulk-creation-of-nsx-rules-with-python
[link-12]: http://puppetlabs.com/blog/new-vmware-vrealize-orchestrator-puppet-plugin
[link-13]: http://www.sonn.com/2015/02/12/the-value-of-the-facebook-wedge-and-6-pack-switches/
[link-14]: https://code.facebook.com/posts/717010588413497/introducing-6-pack-the-first-open-hardware-modular-switch/
[link-15]: http://googlecloudplatform.blogspot.com/2015/01/everything-you-wanted-to-know-about-Kubernetes-but-were-afraid-to-ask.html
[link-16]: http://www.lucd.info/2015/01/22/drsrule-drs-rules-and-groups-module/
[link-17]: https://github.com/PowerCLIGoodies/DRSRule
[link-18]: https://ttboj.wordpress.com/2014/11/04/the-switch-as-an-ordinary-gnulinux-server/
[link-19]: http://crosbymichael.com/creating-containers-part-1.html
[link-20]: http://www.fewbytes.com/docker-selinux-and-the-myth-of-kernel-indipendence/
[link-21]: https://medium.com/@darrenrush/after-docker-unikernels-and-immutable-infrastructure-93d5a91c849e
[link-22]: http://thenewstack.io/solving-a-common-beginners-problem-when-pinging-from-an-openstack-instance/
[link-23]: http://mattcurry.com/2015/02/11/spinning-up-a-server-with-the-openstack-api/
[link-24]: http://www.sebastien-han.fr/blog/2015/02/09/openstack-perform-consistent-snapshots-with-qemu-guest-agent/
[link-25]: https://remote-lab.net/ospf-lab-provisioning-on-ios-with-ansible/
[link-26]: https://telecomoccasionally.wordpress.com/2015/01/11/nsx-for-vsphere-vxlan-control-plane-modes-explained/
[link-27]: http://www.jedelman.com/home/generating-web-docs-for-ansible-modules
[link-28]: http://networkstatic.net/docker-one-liners/
[link-29]: http://www.ericsson.com/mwc2015/launches/hyperscale-datacenter-system-ericsson-hds-8000
[link-30]: http://grantorchard.com/vcac/implementation/protecting-vcac-workloads-nsx-security-groups/
[link-31]: http://www.storagebod.com/wordpress/?p=1731
[link-32]: http://blog.gardeviance.org/2012/06/pioneers-settlers-and-town-planners.html
[link-33]: http://blog.codinghorror.com/commandos-infantry-and-police/
[link-34]: http://www.ntpro.nl/blog/archives/2868-VMware-vCenter-Server-6.0-Deployment-Guide.html
[link-35]: http://blogs.msdn.com/b/virtual_pc_guy/
[link-36]: http://www.virtu-al.net/2015/02/20/retrieving-vm-metrics-from-vcloud-air/
[xref-1]: {{< relref "2015-01-16-looking-ahead-2015-projects.md" >}}
