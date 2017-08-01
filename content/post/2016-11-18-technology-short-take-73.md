---
author: slowe
categories: Information
comments: true
date: 2016-11-18T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Ansible
- NSX
- VMware
- Cumulus
- OVN
- OpenFlow
- OpenStack
- Photon
- JSON
- AWS
- PowerCLI
- Libvirt
- Vagrant
- vSphere
title: 'Technology Short Take #73'
url: /2016/11/18/technology-short-take-73/
---

Welcome to Technology Short Take #73. Sorry for the long delay since the last Technology Short Take; personal matters have been taking quite the toll (if you follow me on Twitter, you'll know to what personal matters I'm referring). In any case, enough of that---here's some data center-related content that I hope you find useful!

## Networking

* Ansible has made some good progress in supporting network automation in the latest release (2.2), according to [this blog post][link-10]. This is an area where I hope to spend more time in the coming weeks before years' end.
* Tomas Fojta shows how to use a PowerShell script to [monitor the health of NSX Edge gateways][link-13].
* Jeremy Stretch mulls over the (perceived) problem of [getting traffic into and out of overlay networks][link-20]. I recommend reading this article, as well as reading the comments. Many commenters suggest just using L3 and having the hosts participate in a routing protocol like BGP, but as Jeremy points out many switches don't have the capacity to handle that many routes. (Or, if they do, they're quite expensive.) Seems like there's this company in Palo Alto making a product that handles this issue pretty decently...([hint][link-21]).
* Cumulus Networks recently [introduced the Cumulus Linux Network Command Line Utility (NCLU)][link-23], which provides a more unified front-end to configuring switches running Cumulus Linux. I could see where some might say that the introduction of the NCLU proves that mainstream Linux on network switches isn't a viable model; personally, I don't see that to be the case. (I'm admittedly a Cumulus Networks fan.) I'm curious to know what others think.
* Russell Bryant has a _really_ great post on [OVN logical flows and the new `ovn-trace` command][link-27]. Russell's post not only provides a great explanation of OVN logical flows, but also does a good job of giving someone the basics of OpenFlow as well.
* If you're not familiar with OVN, Dustin Spinhirne has a good [primer on OVN][link-28].

## Servers/Hardware

* Kevin Houston [discusses][link-29] workload acceleration options---specifically, storage acceleration via NVMe or PCIe SSDs---for blade servers. He also has a good article outlining some [storage options for various blade server platforms][link-30] that might be helpful as well.

## Security

* Marcos Hernandez has a pair of great articles on integrating next-generation security services into OpenStack by leveraging NSX (see [part 1][link-8] and [part 2][link-9]). Marcos also has a post on [integrating Palo Alto into OpenStack][link-11] using NSX and the NetX APIs.

## Cloud Computing/Cloud Management

* Viktor van den Berg has a good article on [using vRA and vRO to automate DNS registrations][link-12].

## Operating Systems/Applications

* Jeff Geerling walks you through [how to use Ansible via Windows 10's Subsystem for Linux][link-1]. Based on Jeff's description, it sounds like the Subsystem for Linux is essentially nothing more than an Ubuntu VM running on Windows 10.
* There's the beginnings of a Photon OS Administration Guide [available on GitHub][link-4]. While it looks reasonably comprehensive and complete, I do have to question how current it is given that there are still references to AppCatalyst (which was discontinued by VMware quite a while ago). Still, there's some good information in there if you're using/experimenting with Photon OS.
* I've mentioned both [Paw][link-6] (OS X API tool I discussed [here][xref-1]) and [`jq`][link-7] (command-line tool for manipulating JSON, found in [my post here][xref-2]) before, but now you can have [both of them together][link-5]. Nifty.
* On the topic of Ansible again, here's an article by Scott Harney on [using Ansible to bootstrap an EC2 instance][link-15] configured just the way he wants it. You may also find some of the other articles in that series useful as well; I encourage you to have a look at them.
* Matt Oswalt has a _great_ article on [the principles of automation][link-16]. Read it, study it, breath it, live it. Seriously.
* PowerCLI Core, which enables cross-platform support for PowerCLI, is  [available as a fling][link-17]. Cool.
* Chris Evans [speculates][link-19] that the release of Amazon Linux as a Docker container for use w/ on-premises workloads signals a shift for AWS.

## Storage

* Robin Harris [tackles some concerns over 3D XPoint][link-31]; specifically, concerns over original performance claims (which focuses on raw chip-level performance) and tested performance numbers (which were based on complete systems).
* Chris Evans [talks a bit about Swordfish][link-32], the extension to the Redfish server hardware management API that focuses specifically on block storage.

## Virtualization

* If you [follow me on Twitter][link-3], you may have seen me mention that I was experimenting with [the Libvirt provider for Vagrant][link-2]. While this provider looks promising, my experience is that it's still "bleeding edge"---expect some minor cuts if you decide to use it.
* William Lam describes some anticipated [enhancements for nested ESXi in vSphere 6.5][link-14]. William also discusses [a new USB 3.0 Ethernet adapter driver][link-33] for vSphere 6.5.
* Frank Denneman---now back at VMware (welcome back, Frank!)---has [a closer look at VMware Cloud on AWS][link-18].

## Career/Soft Skills

* Tom Hollingsworth [article on designer vs. architect][link-22] is one that has interesting implications for IT professionals. It sounds like what Tom is saying is that we need to cultivate a broader skill set with the ability to find a solution to a problem (instead of finding a problem for the solution---think about that, you'll get it after a while).
* Want to "level up" on your distributed systems skills? Then I suggest reading first [this introductory post on Flexible Paxos][link-24], then going for a deeper dive into [the details][link-25] and [more details][link-26].

I'll cut it off there before it gets any longer. Still so much content, but so little room to share it all! I suppose I'll save it for the next Technology Short Take, and give you a reason to return here. Until then, best of luck!



[link-1]: http://www.jeffgeerling.com/blog/2016/using-ansible-through-windows-10s-subsystem-linux
[link-2]: https://github.com/vagrant-libvirt/vagrant-libvirt
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://github.com/vmware/photon/blob/master/docs/photon-admin-guide.md
[link-5]: https://paw.cloud/docs/dynamic-values/jq-processor
[link-6]: https://paw.cloud
[link-7]: https://stedolan.github.io/jq/
[link-8]: http://blogs.vmware.com/openstack/next-generation-security-services-openstack/
[link-9]: http://blogs.vmware.com/openstack/next-generation-security-services-openstack-part-2/
[link-10]: https://www.ansible.com/blog/ansible-network-updates
[link-11]: http://blogs.vmware.com/openstack/advanced-security-services-neutron-nsx-palo-alto-next-generation-firewall/
[link-12]: https://www.viktorious.nl/2016/10/12/automated-dns-registration-with-vra/
[link-13]: https://fojta.wordpress.com/2015/03/02/how-to-monitor-health-of-nsx-edge-gateways/
[link-14]: http://www.virtuallyghetto.com/2016/10/nested-esxi-enhancements-in-vsphere-6-5.html
[link-15]: https://www.scottharney.com/using-ansible-to-bootstap-my-work-environment-part-4/
[link-16]: https://keepingitclassless.net/2016/10/principles-of-automation/
[link-17]: http://blogs.vmware.com/PowerCLI/2016/10/powercli-core-fling-available.html
[link-18]: http://frankdenneman.nl/2016/10/13/vmware-cloud-aws-closer-look/
[link-19]: https://blog.architecting.it/2016/11/02/aws-getting-serious-about-enterprise-on-prem-with-dockerised-linux-ami/
[link-20]: http://packetlife.net/blog/2016/sep/30/overlay-problem-getting-in-out/
[link-21]: http://www.vmware.com/products/nsx.html
[link-22]: https://networkingnerd.net/2016/10/27/designer-or-architect-its-a-matter-of-choice/
[link-23]: https://cumulusnetworks.com/blog/cumulus-linux-network-command-line-utlility/
[link-24]: https://dahliamalkhi.wordpress.com/2016/08/26/flexible-paxos-a-new-breed-of-scalable-resilient-and-performant-consensus-algorithms-is-made-possible/
[link-25]: http://hh360.user.srcf.net/blog/2016/08/majority-agreement-is-not-necessary/
[link-26]: http://ssougou.blogspot.com/2016/08/a-more-flexible-paxos.html
[link-27]: https://blog.russellbryant.net/2016/11/11/ovn-logical-flows-and-ovn-trace/
[link-28]: http://blog.spinhirne.com/2016/09/a-primer-on-ovn.html
[link-29]: http://bladesmadesimple.com/2016/10/workload-acceleration-options-for-blade-servers/
[link-30]: http://bladesmadesimple.com/2016/11/storage-options-for-blade-servers/
[link-31]: http://storagemojo.com/2016/10/06/is-3d-xpoint-in-trouble/
[link-32]: https://blog.architecting.it/2016/10/17/snia-has-high-hopes-for-swordfish/
[link-33]: http://www.virtuallyghetto.com/2016/11/usb-3-0-ethernet-adapter-nic-driver-for-esxi-6-5.html
[xref-1]: {{< relref "2015-11-14-handy-gui-tool-working-apis.md" >}}
[xref-2]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
