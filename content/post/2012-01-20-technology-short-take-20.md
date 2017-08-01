---
author: slowe
categories: Information
comments: true
date: 2012-01-20T09:47:47Z
slug: technology-short-take-20
tags:
- Networking
- Storage
- Virtualization
- VXLAN
- Cisco
- Nexus
- OpenStack
- CLI
- EMC
- FibreChannel
- FCoE
- Linux
- HyperV
title: 'Technology Short Take #20'
url: /2012/01/20/technology-short-take-20/
wordpress_id: 2525
---

Welcome to Technology Short Take #20, the latest collection of various data center-related links, articles, and thoughts. I hope you find something useful here.

## Networking

* For all the writing and thinking I've done on VXLAN ([here][1], [here][2], and [here][3]), someone (thanks Ed!) mentioned something to me recently that I hadn't even considered. (It's so simple I'm embarrassed that I overlooked it.) VXLAN uses UDP for its encapsulation. What about dropped packets, lack of sequencing, etc., that is possible with UDP? What impact is that going to have on the "inner protocol" that's wrapped inside the VXLAN UDP packets? Or is this not an issue in modern networks any longer?

* Since Ivan thinks we _do_ need to [worry about spaghetti and horseshoes](http://blog.ioshints.info/2012/01/can-we-really-ignore-spaghetti-and.html), I'm curious to know if he thinks we need to worry about UDP as the transport for VXLAN.

* Jake Howering of Cisco (with whom I've worked on a few occasions) posted a couple of good articles on the Cisco Data Center and Cloud blog. Jake focuses on DCI (data center interconnect), so his articles naturally reflect that focus. The first article [discusses the use of LISP for ingress path optimization](http://blogs.cisco.com/datacenter/lisp-finding-the-optimized-path-for-your-workload/) in VM mobility scenarios. Ingress path optimization is only half of the solution, though; Jake discusses the other half in the second article, which covers [egress path optimization with FHRP filtering](http://blogs.cisco.com/datacenter/fhrp-egress-path-optimization-from-the-server-to-the-client/), something I discussed in [my recent VXLAN/OTV article][4].

* It looks like you [can't create a vPC](http://www.vnephos.com/index.php/2011/04/nexus-55xx-50x0-vpc-incompatibility/) (virtual port channel) between a Nexus 55xx and a Nexus 50x0 switch.

* Kyle Mestery speculates a bit here about [integrating VXLAN in OpenStack Quantum](http://blogs.cisco.com/openatcisco/integrating-vxlan-in-openstack-quantum/).

## Servers/Applications/Operating Systems

* I'll put this under the "Servers" category even though it's storage-related: In [this post](http://angryjesters.wordpress.com/2012/01/05/cisco-vic-boot-from-san-troubleshooting/), Ryan Hughes explains how to use the UCSM CLI to do some cool---and useful---boot from SAN troubleshooting.

* Purely by accident, I stumbled across this humorously-titled series regarding Oracle on Fibre Channel. It turns out the author works for EMC, although I didn't know that ahead of time. In any case, if you're interested in finding out how manly men deploy Oracle, check out the series titled "Manly Men Only Deploy Oracle with Fibre Channel" (parts [1](http://kevinclosson.wordpress.com/2007/06/14/manly-men-deploy-oracle-with-fibre-channel-only-oracle-over-nfs-is-weird/), [2](http://kevinclosson.wordpress.com/2007/06/28/manly-men-only-deploy-oracle-with-fibre-channel-part-ii-what-so-simple-and-inexpensive-about-nfs-for-oracle/), [3](http://kevinclosson.wordpress.com/2007/06/29/manly-men-only-deploy-oracle-with-fibre-channel-part-iii-did-i-hear-emc-say-nas/), [4](http://kevinclosson.wordpress.com/2007/07/09/manly-men-deploy-oracle-with-fibre-channel-only-part-iv-sans-are-simple-rac-is-difficult/), [5](http://kevinclosson.wordpress.com/2007/07/10/manly-men-deploy-oracle-with-fibre-channel-only-part-v-what-about-oracle9i-on-rhas-21-yippie/), [6](http://kevinclosson.wordpress.com/2007/07/11/manly-men-only-deploy-oracle-with-fibre-channel-part-vi-introducing-oracle11g-direct-nfs/), [7](http://kevinclosson.wordpress.com/2007/07/12/manly-men-only-deploy-oracle-with-fibre-channel-part-vii-a-very-helpful-step-by-step-rac-install-guide-for-nfs/), and [8](http://kevinclosson.wordpress.com/2007/07/17/manly-men-only-deploy-oracle-with-fibre-channel-part-viii-after-all-oracle-doesnt-support-async-io-on-nfs/)).

## Storage

* Clint Kitson recently published a guide on [setting up the FC zoning for WAN communications on a VPLEX Metro cluster](http://velemental.com/2012/01/06/an-fc-dive-preparing-fc-switches-for-a-vplex-metro-install/). This article is a good complement to my own MDS zoning articles ([creating MDS zones][5], [managing MDS zones][6], [using device aliases][7]) and shows some practical examples of those commands in action. 

* Simon Seagrave [points out](http://www.techhead.co.uk/iomega-ix2-ix4-os-x-lion-time-machine-update-fix) a firmware update for Iomega IX2 and IX4 storage devices that fixes problems with Time Machine under Mac OS X 10.7 "Lion". Thanks Simon!

* [This post](http://hansdeleenheer.blogspot.com/2011/07/vendor-acquisitions-partnerships-v2.html) by Hans De Leenheer was written in July of last year, but it only came to my attention in late November after I met Hans in Vienna during my "round the world" trip. (Great guy, by the way.) He asked me to respond to his criticism of EMC's "mega-launch" in early 2011. I wish I could say Hans is way off, but I personally agree that the hyperbole was a bit much. I do have to disagree a bit with his comment about no new products; while EMC didn't introduce all new hardware for some lines (like the VMAX), they did introduce new array software (like Enginuity 5875 for the VMAX). Does that count as a "new product"? I guess the answer to that depends on whether you work in marketing. At least it's good to hear that Hans feels EMC is, in his words, "a solid company making solid storage solutions". Perhaps EMC should focus more on that

* Erik Smith has written a very detailed post on [FC/FCoE hard zoning versus soft zoning](http://brasstacksblog.typepad.com/brass-tacks/2012/01/hard-zoning-versus-soft-zoning-in-a-fcfcoe-san.html). It's an excellent post, and well worth the time if you aren't already an FC/FCoE expert. By the way, if you haven't yet subscribed to Erik's RSS feed, I _highly_ recommend it. He puts out some great stuff.

* David Robertson wrote up the steps for [creating a Linux-based SMC/SPA server](http://storageboy.com/2012/01/09/creating-a-linux-based-smc-spa-server/), in the event you use EMC Symmetrix in your environment.

## Virtualization

* If you haven't read [this vSphere 5 APD/PDL post](http://blogs.vmware.com/vsphere/2011/08/all-path-down-apd-handling-in-50.html) by Cormac Hogan, go read it now. Seriously.

* Several bloggers weighed in on the release of PowerCLI 5.0.1, which adds support for automating VMware vCloud Director. You can get more information from [Julian Wood](http://www.wooditwork.com/2012/01/10/vmware-powercli-5-01-released-adding-vcloud-director-automation/) or [Luc Dekens](http://www.lucd.info/2012/01/10/powercli-5-0-1-goes-cloud/).

* Forbes Guthrie, lead author on _VMware vSphere Design_ (for which I was a co-author), has released [a vSphere 5 version of his vReference card](http://www.vreference.com/2012/01/09/vsphere-5-vreference-card-released/).

* Chris Wahl posted an article on [managing user assignments in VMware View dedicated pools](http://wahlnetwork.com/2012/01/07/managing-user-assignments-in-vmware-view-dedicated-pools/). Watch out for Mr. Fancy Pants, though

* If you are experiencing time-out errors with vSphere HA waiting for cluster election, see [this post](http://www.yellow-bricks.com/2012/01/04/vsphere-ha-waiting-for-cluster-election-to-complete-operation-timed-out/) by Duncan Epping for a fix.

* Need to convert from vCloud linked clones? Chris Colotti has [a solution](http://www.chriscolotti.us/vmware/how-to-convert-from-vcloud-linked-clones/) that might work for you.

* Rick Vanover indicates that [the next release of Hyper-V will support NPIV](http://www.techrepublic.com/blog/networking/windows-server-8-virtual-fibre-channel-with-hyper-v-overview/5168). I wonder if Microsoft's implementation of NPIV will be asum, goodas vSphere's implementation. Let's hope that Microsoft does a better job with NPIV. Personally I think NPIV has potential in a virtualized environment, but the current VMware implementation is crippled at best.

* Scott Sauer gives us a [good write-up on Elastic Memory for Java (EM4J)](http://www.virtualinsanity.com/index.php/2012/01/10/infrastructure-deep-dive-on-em4j-with-vmware-vsphere/). I think EM4J is an important part of expanding virtualization's footprint by allowing it to more efficiently handle Java workloads.

* Using vCenter Operations? You might be interested in this tool created by Matt Cowger called [the vCenter Operations Data Extraction Tool](http://blog.cowger.us/2012/01/19/vcenter-ops-vcops-data-extraction-tool-1-0/).

* Gabrie van Zanten (aka [@gabvirtualworld](http://twitter.com/gabvirtualworld)) points out an [interesting interaction between licensing and DRS rules](http://www.gabesvirtualworld.com/vcenter-drs-rules-bug-when-downgrading-license/).

* Here are some potential [vCloud Director design guidelines](http://www.vmguru.nl/wordpress/2012/01/vmware-vcloud-director-design-guidelines/).

* Frank Denneman posted a discussion on the use of [multi-extent datastores with Storage DRS (SDRS)](http://frankdenneman.nl/2012/01/sdrs-and-multi-extents-datastores/). His conclusion is that the introduction of SDRS in vSphere 5 greatly alleviates the need for multi-extent datastores, a conclusion with which I (generally) agree.

* [This VMware KB article](http://kb.vmware.com/kb/2005284) has a collection of the firewall-related commands in ESXi 5.0; this is a useful reference in my opinion.

Here are a few other virtualization-related links that I also thought you might find interesting or useful:

[Are operational considerations overlooked in virtualization?](http://www.techrepublic.com/blog/networking/are-operational-considerations-overlooked-in-virtualization/5237)  

[VMware KB - Detaching a datastore or storage device from multiple ESXi 5.0 hosts](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2011506)  

[What happened to vShield in vSphere 5?](http://vsphere-land.com/news/what-happened-to-vshield-in-vsphere-5.html)  

[Linux VMware Tools Install Changes to Upstart](http://www.chriscolotti.us/vmware/info-linux-vmware-tools-install-changes-to-upstart/)  

[VMware KB - Disabling VAAI Thin Provisioning Block Space Reclamation (UNMAP) in ESXi 5.0](http://kb.vmware.com/kb/2007427)

## Cloud Computing

* You'll note that I didn't lump cloud computing under virtualization, since I think that there's more to "cloud" than virtualization (although I would grant that virtualization is a big part of it). So much more, in fact, that people are referring to cloud computing as complex. (Gasp!) But [this post](http://blog.theloosecouple.com/2012/01/10/cloud-complexity-its-a-wrench/) puts cloud complexity in perspective. If you are merely a consumer of services, then cloud is simple; if, however, you are a provider of services, then cloud is more complex. Good article.

That's it for this time around. If you have any comments or thoughts on anything shared here, feel free to speak up in the comments.

[1]: {{< relref "2011-12-02-examining-vxlan.md" >}}
[2]: {{< relref "2011-12-07-revisiting-vxlan-and-layer-3-connectivity.md" >}}
[3]: {{< relref "2011-12-22-otv-and-vxlan-layer-3-connectivity-compared.md" >}}
[4]: {{< relref "2011-12-22-otv-and-vxlan-layer-3-connectivity-compared.md" >}}
[5]: {{< relref "2009-08-24-new-users-guide-to-configuring-cisco-mds-zones-via-cli.md" >}}
[6]: {{< relref "2009-10-20-new-users-guide-to-managing-cisco-mds-zones-via-cli.md" >}}
[7]: {{< relref "2010-12-08-using-device-aliases-on-a-cisco-mds.md" >}}
