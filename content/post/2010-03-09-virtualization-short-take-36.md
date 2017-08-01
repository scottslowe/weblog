---
author: slowe
categories: Information
comments: true
date: 2010-03-09T10:04:55Z
slug: virtualization-short-take-36
tags:
- NFS
- Virtualization
- VMware
- vSphere
title: 'Virtualization Short Take #36'
url: /2010/03/09/virtualization-short-take-36/
wordpress_id: 1858
---

It's been a busy couple of weeks! I was in Vienna, Austria, all last week, and I'm on the US West Coast this week. Even though I've been on the go, I've still been collecting various virtualization-related posts and tidbits. Here they are for you in Virtualization Short Take #36! I hope you find something useful.

* You might recall that in early 2008 I wrote about [how thin provisioned VMDKs on NFS storage tend to inflate][1]. In a [recent post](http://virtualgeek.typepad.com/virtual_geek/2010/02/fixing-eagerzeroedthick-use---in-vi35-vsphere-and-using-zero-reclaim.html), Chad Sakac pointed out that VMware has addressed this problem, which is caused by the use of the eagerzeroedthick VMDK format instead of the zeroedthick format. The fix requires ESX 3.5 Update 5 and VirtualCenter 2.5 Update 6, plus a configuration change that is outlined in [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1017666). Kudos to VMware for fixing the underlying issue instead of just forcing customers to upgrade to vSphere.

* VMware's Scott Drummonds provides [a bit more information](http://vpivot.com/2010/03/01/memory-compression/) on the memory compression technology previewed by Steve Herrod at Partner Exchange 2010 a few weeks ago. In my opinion, anyone who says that the hypervisor is a commodity isn't paying attention to the fact that VMware is still innovating in this space.

* If you perform virtualization assessments using VMware's Capacity Planner tool, you'll find Gabe's [Capacity Planner troubleshooting tips](http://www.gabesvirtualworld.com/?p=1032) helpful.

* Gabe also published [a "wish list" for VMware datastores](http://www.gabesvirtualworld.com/?p=1086). If you take a deeper look at what Gabe is really trying to address, though, a great deal of the functionality he's looking for could be achieved through a combination of policy-based storage tiering and greater integration between VMware and the storage array. Would you really need category labels on VMware datastores if the underlying storage was tiering data effectively based on utilization? Probably not. It might still make sense in some cases, but I think the vast majority of cases would be addressed. I think that you are going to see some very cool innovation in this space over the course of this year.

* Simon Gallagher recently asked this question: [with the move to ESXi, is NFS more useful than VMFS?](http://vinf.net/2010/03/02/with-the-move-to-esxi-is-nfs-becoming-more-useful-than-vmfs/) It appears that a large part of Simon's argument centers around the speed at which files can be transferred into VMFS using ESXi, and it seems to me that VMware needs to do some optimization there. I'm not knocking NFS---I've used it extensively in the past and I have and continue to recommend it to customers where it is appropriate---but I'm not sure that you can build an argument for NFS based on ESXi's file transfer performance. My friend and former colleague Aaron Delp (whose [blog](http://blog.aarondelp.com/) was recently added to Planet V12n; congrats!) points out that fixing VMDK alignment using ESXi could be an issue; now that's a great point. Even third-party utilities like vOptimizer don't work with ESXi. In my opinion, these points underscore the need for VMware to concentrate very heavily on ESXi if that is indeed going to be the "platform moving forward".

* I came across an interesting VMware KB article while browsing the weekly VMware KB digest for [the week ending 2/28/10](http://blogs.vmware.com/kbdigest/2010/02/new-articles-published-for-week-ending-02282010.html?utm_source=feedburner&utm_medium=twitter&utm_campaign=Feed%3A+VmwareKnowledgebaseWeeklyDigest+(VMware+Knowledge+Base+Digest)). The article, which discusses a situation in which [VMware HA would fail to configure at 90% completion](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1018217), describes how some network switches---HP ProCurve 1810G switches with automatic denial-of-service protection enabled and Cisco Catalyst 4948 switches with ICMP rate limiting enabled---can drop packets that are necessary for VMware HA to configure and start correctly.

* Unfortunately, the latest VMware KB weekly digest (for [the week ending 3/6/2010](http://blogs.vmware.com/kbdigest/2010/03/new-articles-published-for-week-ending-362010.html?utm_source=feedburner&utm_medium=twitter&utm_campaign=Feed%3A+VmwareKnowledgebaseWeeklyDigest+(VMware+Knowledge+Base+Digest))) didn't include links to the actual articles that were published. Bummer! Still, it's easy enough to simply look up the articles directly.

* EMC today released a couple of plug-ins for vCenter Server. The Celerra plug-in for VMware Environments brings Celerra NFS provisioning into the vSphere Client. The Celerra Failback Plug-in for SRM automates failback in VMware SRM environments. The official press release is [here](http://www.emc.com/about/news/press/2010/20100309-01.htm), which contains links to more information on the individual plug-ins. _(Disclaimer: I work for EMC.)_

* Newly-minted VCDX #029 Frank Denneman posted a good article on [using reservations on resource pools to bypass slot sizing](http://frankdenneman.nl/2010/02/resource-pools-and-avoiding-ha-slot-sizing/). As Frank points out, it's not a recommended practice necessarily, but it might be warranted depending on customer requirements.

* Duncan's recent article on the [behavior of CPU and memory reservations](http://www.yellow-bricks.com/2010/03/03/cpumem-reservation-behaviour/) is also helpful, especially for those who might not be familiar with the differences between the two types of reservations.

* Similarly, [this guest post](http://www.yellow-bricks.com/2010/02/22/the-resource-pool-priority-pie-paradox/) on Duncan's site by VCDX Craig Risinger also helps explain how shares on a resource pool work. This is good information to have if you are unfamiliar with the topic.

* I'm not a security geek, but I did think that the RSA-Intel-VMware announcement at RSAC 2010 (third-party coverage [here](http://www.darkreading.com/securityservices/security/encryption/showArticle.jhtml?articleID=223101210&cid=RSSfeed_DR_News)) was pretty cool. Security experts, I'd love to hear your thoughts on the matter. What was good about the announcement? What was missing?

* If you will be working with distributed vSwitches, [this post](http://thesaffageek.wordpress.com/2010/03/08/vms-cant-ping-while-on-distributed-virtual-switches-vlans/) by EMC's Gregg Robertson might help; it underscores the need to ensure that your environment is being consistently and thoroughly patched and maintained. vCenter Update Manager, anyone?

I do have a few other articles in my "things to read list" that I haven't yet gotten around to reading:

[The Official Quest Software Desktop Virtualization Group Blog  Blog Archive  How to Integrate ThinApp with Quest vWorkspace 7.0](http://blogs.inside.quest.com/provision/2010/03/02/how-to-integrate-thinapp-with-quest-vworkspace-70/)  

[DRS Resource Distribution Chart](http://frankdenneman.nl/2010/03/drs-resource-distribution-chart/)  

[HP Flex-10 versus Nexus 5000 & Nexus 1000V with 10GE passthrough](http://www.internetworkexpert.org/2010/02/09/hp-flex-10-versus-nexus-5000-nexus-1000v-with-10ge-passthrough/)

That's it for now. I hope that you've found something useful here, and---as always---I'd love to hear your thoughts in the comments below.

[1]: {{< relref "2008-03-31-only-thin-provisioned-in-the-beginning.md" >}}
