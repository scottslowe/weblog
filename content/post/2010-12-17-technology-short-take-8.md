---
author: slowe
categories: Information
comments: true
date: 2010-12-17T09:00:00Z
slug: technology-short-take-8
tags:
- Cisco
- EMC
- Microsoft
- Networking
- Nexus
- Storage
- UCS
- Virtualization
- VMware
title: 'Technology Short Take #8'
url: /2010/12/17/technology-short-take-8/
wordpress_id: 2182
---

Welcome to Technology Short Take #8, my latest collection of item from around the Internet pertaining to data center technologies such as virtualization, networking, storage, and servers. I hope you find something useful here!

## Networking

* A couple weeks ago I wrote about [how to connect a Nexus 2148T to a Nexus 5010][1]. What I haven't actually done yet is connect the Nexus 2148T to dual Nexus 5010 upstream switches using virtual port-channels. [This post](https://supportforums.cisco.com/message/3052114) offers a configuration for just such an arrangement (just be sure to read the follow-up post for a key command to include in the configuration).

* And speaking of fabric extenders, Brad Hedlund recently posted a great article [explaining QoS on the Cisco UCS fabric extender](http://bradhedlund.com/2010/12/08/cisco-ucs-fabric-extender-fex-qos/).

* Erik Smith has posted [part 2](http://brasstacksblog.typepad.com/brass-tacks/2010/12/fip-fip-snooping-bridges-and-fcfs-part-2-fip-snooping-bridges-and-fibre-channel-forwarders.html) of his series on FIP, FIP snooping bridges, and FCFs.

* This post by Ethan Banks on [the scaling limitations of EtherChannel](http://packetattack.wordpress.com/2010/11/27/the-scaling-limitations-of-etherchannel-or-why-11-does-not-equal-2)  reinforces what I've said before about the use of EtherChannel and VMware ESX/ESXi (see [this post][2] and [this follow-up post][3]). Apparently this misunderstanding about how link aggregation works isn't just limited to virtualization folks.

* This [article on BPDU Guard and BPDU Filter](http://blog.ipexpert.com/2010/12/06/bpdu-filter-and-bpdu-guard/) is a bit deep for the non-expert networkers (like me), but is good reading nevertheless. Since it is recommended that you disable Spanning Tree Protocol for ports facing VMware ESX/ESXi hosts (and here's [the explanation why](http://blog.ioshints.info/2010/11/vmware-virtual-switch-no-need-for-stp.html) from our good friend Ivan "IOSHints" Pepelnjak), it's good to understand which commands to use and at which scope to use them.

* While link aggregation isn't a panacea, it can be helpful in a number of situations. However, you really need to consider multi-chassis link aggregation in order to eliminate single points of failure. Ivan Pepelnjak posted a series of articles on multi-chassis link aggregation (MLAG): [MLAG basics](http://blog.ioshints.info/2010/10/multi-chassis-link-aggregation-basics.html), [MLAG via stacking](http://blog.ioshints.info/2010/10/multi-chassis-link-aggregation-stacking.html), and [MLAG external brains](http://blog.ioshints.info/2010/11/multi-chassis-link-aggregation-mlag.html). I think there's another post still coming, correct Ivan?

* [This post](http://evilrouters.net/2010/10/12/reason-693-why-i-love-unix/) is written by networking guru Jeremy Gaddis, but it's really more about UNIX and regular expressions. So in which category does it belong? (Does it really matter?) I guess since it deals with how to use UNIX tools and regular expressions to manipulate networking data I'll put it in the networking category.

* Here's a good post on creating [LAN-to-LAN tunnels between a Vyatta router and a Cisco ASA](http://roggyblog.blogspot.com/2010/11/vyatta-to-cisco-tunneling-from-asa-to.html).

* Joe Onisick has a good write-up on [the behavior of inter-fabric traffic in UCS](http://www.definethecloud.net/inter-fabric-traffic-in-ucs).

## Servers

* Still think that Cisco UCS requires 10Gb Ethernet and Cisco Nexus switches? You might want to read [this](http://jeffsaidso.com/2010/12/ucs-only-runs-at-10-ge-and-requires-nexus/). Seriously, go read it.

* Jeff Allen has posted a good discussion of [UCS BIOS policies](http://jeffsaidso.com/2010/11/ucs-bios-policies/).

* In the past I've covered various integrations with RADIUS using Microsoft IAS (see [here][4], [here][5], and [here][6] for some examples). Here's a post on [how to use Microsoft IAS to integrate Cisco UCS into Active Directory](http://jwalther.tumblr.com/post/1204778672/configuring-radius-with-cisco-ucs-and-microsoft-ias).

## Storage

* It's good to see more "virtualization" people getting into storage. Recently, Jon Owings (of "2vcps and a Truck" fame) posted two good articles on storage caching vs. tiering ([part 1](http://www.2vcps.com/2010/11/19/storage-caching-vs-tiering-part-1/) and [part 2](http://www.2vcps.com/2010/11/24/storage-caching-vs-tiering-part-2/)). Jon provides a good vehicle to discuss these technologies, both of which are valuable and both of which have a place in many companies' storage strategy.

* Here's another view on [storage tiering vs. caching](http://techvirtuoso.com/2010/11/12/storage-tiering-vs-caching/).

* And speaking yet again of caching and tiering...here's some [info on FAST (tiering) and FAST Cache (caching)](http://vblog.matt-taylor.org/2010/11/07/fast-and-fast-cache-some-quick-highlights/).

* Finally, rounding out this week's caching and tiering discussions, I have Nigel Poulton's post on [sub-LUN tiering design considerations](http://blog.nigelpoulton.com/sub-lun-tiering-design-considerations/).

## Virtualization

* Noted virtualization performance guru Scott Drummonds posted a collection of thoughts [regarding maximum hosts per cluster](http://vpivot.com/?p=702). Duncan Epping---who along with Frank Denneman recently published their _HA and DRS Technical Deep Dive_ book---followed up with [his own post](http://www.yellow-bricks.com/2010/11/29/re-maximum-hosts-per-cluster-scott-drummonds/) on the matter. Both Scott and Duncan summarized some of the advantages and disadvantages of both large and small clusters. The real key to understanding the "right" cluster size is, as Duncan points out, an understanding of how best to meet the requirements of the environment.

* Scott also posted this useful discussion on [considerations around VMDK consolidation](http://vpivot.com/2010/11/07/storage-consolidation-or-how-many-vmdks-per-volume/) in virtualized environments.

* Need to find the IP address of a VM from an ESX host? Try [this article](http://jlscz.blogspot.com/2010/12/how-to-find-vms-ip-address-from-esx.html), which provides the steps and tools you need.

* I saw a couple of good articles from Frank Denneman recently. One was on [how to disallow multiple VM console sessions](http://frankdenneman.nl/2010/11/disallowing-multiple-vm-console-sessions/); the second was discussing [disabling the balloon driver and why it's generally not a good idea](http://frankdenneman.nl/2010/11/disable-ballooning/). Frank also posted a piece recently on [the interaction between QoS and the adaptive algorithm that DRS uses](http://frankdenneman.nl/2010/11/the-impact-of-qos-network-traffic-on-vm-performance/) to determine concurrent vMotions. While I agree that you don't want to impact DRS' ability to perform concurrent vMotions to restore cluster balance, it's also critically important to ensure proper balance in network traffic. You don't want a single type of network traffic to swamp others. As with many other things in the virtualization arena, just be sure to understand the impact of your decisions.

* If you need to shape network traffic (like vMotion), here's a [how to document](http://v-reality.info/2010/10/shaping-vmotion-in-10-gb-networks/) on how it's done.

* [This article](http://www.softwareadvice.com/articles/accounting/microsoft-is-all-in-for-the-cloud-but-what-about-dynamics-1121310/) underscores the difficulty that many organizations have in adopting "the cloud", at least as it pertains to public cloud-based services. Perhaps this is why VMware is so keen on embracing software development tools that help build cloud-enabled applications...

* I think that `esxtop` remains an underestimated tool in troubleshooting. If you need to deepen your understanding of this very useful tool, a good place to start---among the many, many resources that are available---would be [this presentation](http://www.virtu-al.net/2010/11/10/vmware-session-4-advanced-performance-troubleshooting-using-esxtop/). Thanks for posting this, Alan!

* William Lam has posted a thorough guide to [using vi-fastpass with fpauth and adauth](http://www.virtuallyghetto.com/2010/11/how-to-configure-and-use-vmas-vi.html). As the vMA grows in importance with the transition to ESXi in the next major release of vSphere, tips and tricks such as this are going to be worth their words in gold. Good job William!

* Remember how I've mentioned before that it's funny when vendors taking existing technologies and rename them to include the word "virtualization"? That's the feeling I get from reading [this article on Microsoft's user state virtualization](http://windowsteamblog.com/windows/b/business/archive/2010/11/30/user-state-virtualization-what-is-it-and-how-will-it-help-you-deliver-a-dynamic-and-personal-windows-experience.aspx).

It's time to wrap up now, so I'll just say that I welcome any feedback, corrections, clarifications, or additions in the comments below. I hope you've found something helpful. Thanks!

[1]: {{< relref "2010-11-30-connecting-a-nexus-2148-to-a-nexus-5010.md" >}}
[2]: {{< relref "2008-07-16-understanding-nic-utilization-in-vmware-esx.md" >}}
[3]: {{< relref "2008-10-08-more-on-vmware-esx-nic-utilization.md" >}}
[4]: {{< relref "2007-07-02-authenticating-to-cisco-ios-via-active-directory.md" >}}
[5]: {{< relref "2006-12-07-8021x-integration-with-active-directory.md" >}}
[6]: {{< relref "2005-11-22-cisco-pix-vpn-and-active-directory-integration.md" >}}
