---
author: slowe
categories: Liveblog
comments: true
date: 2009-09-01T13:29:09Z
slug: bc1500-site-recovery-manager-best-practices
tags:
- Storage
- Virtualization
- VMware
- VMworld2009
title: BC1500 - Site Recovery Manager Best Practices
url: /2009/09/01/bc1500-site-recovery-manager-best-practices/
wordpress_id: 1603
---

I arrived at this session a few minutes late because the VMworld 2009 Day 1 keynote was running late. The early part of the session was pretty straightforward with information on the various SRM components: multiple vCenter Server instances, SRM servers, storage replication, SRAs, etc., so I won't bother recreating this here. Most people are already familiar with the basics of SRM.

The technology preview of SRM (the beta of the next version) does support vCenter Server linked mode, which means that your two vCenter Server instances can run in a linked mode group. Very nice!

The next version of SRM will allow you to change the authentication option via a Repair installation. This is a big improvement from the current release, where this has to be selected at installation and can't be changed afterward.

VMware KB article 1008390 provides more information on how to setup the self-signed certificates and which subject names should be used for correct operation.

It's important to remember that SRM doesn't perform replication; the storage performs the replication. The SRA allows SRM to control the movement of that storage and how it is presented to the various hosts involved in the solution.

The SRA is written by the storage vendor and must be installed on both SRM servers. Multiple SRAs can be installed on the same SRM server to support multiple storage platforms. New adapters do not require SRM updates. The SRA then supports tasks such as setup, testing, and failover. So what goes wrong with SRAs?

* Not all SRAs are created equal. They support the same functionality but the implementation might be different.

* Some SRAs have external requirements, such as the Java JRE, or storage-specific components from the storage vendor. The key takeaway is to be sure to read the documentation from the storage vendor about the SRA. The SRM community forum can also be a good source of information.

* SRAs expect a certain level of replicated storage configuration. This might include little quirks like ensuring that certain flags or attributes are set as expected by the SRA.

Some SRAs talk directly to the storage array, but some SRAs require connectivity to a control station or management station. Again, be sure to refer to the SRA documentation to be sure of the configuration requirements.

Recommendation: whatever you do at one site, duplicate that effort at the other site. This will help simplify things.

Some storage arrays require access to "gatekeeper LUNs"; when running SRM as a VM, you can present these LUNs to the SRM VMs as RDMs. Other storage implementations, like HP EVA or IBM SVC, need access to the management server itself and not to the array directly. HP recently released a guide to using SRM with HP storage that might be a good idea to read.

Other pitfalls during SRA setup:

* Attempts to configure SRA at protected site will fail if SRA components are not installed at recovery site.

* For active/passive setup, just perform configuration at the protected site, not the recovery site. Configuration at the recovery site is only required for failback or active/active setups.

* Double-check path definitions; older SRAs used Perl or similar and caused problems with failing to update the path definitions. Get updated SRAs; this has all been fixed in newer versions of the SRAs.

* Be sure the array configuration matches the pre-requisites.

* SRM might need additional licensed features in order to perform a Test failover. (Key example: FlexClone required on a NetApp array in order to perform test failovers.)

* Empty datastores won't be detected by SRM even if it is replicated. This has been fixed in the next release.

Next the presenter shifts to a discussion of virtual machine networking.

SRM will work with stretched VLANs. To architect SRM to work with stretched VLANs and failover tests, you can use the bubble network, but that isn't really practical. Instead, the presenter recommends the creation of test VLANs on the physical network to support failover testing.

Instead of stretched VLANs, you can also use disparate networks. In this case, of course, the VLANs change from the protected site to the recovery site. This impacts IP addressing, DNS, Active Directory, default gateways, DHCP, etc. You can use existing tools to fix these problems; you **don't** have to use the SRM-supplied tools. SRM supplied tools include VM customization or the dr-ip-customizer.exe batch utility. Dynamic DNS updates are another option as well.

So what about virtual machine disks? What are the storage factors that come into play here?

* Current version of SRM supports iSCSI and Fibre Channel, and supports both physical and virtual RDMs

* VMs can span multiple datastores (but need to consider replication consistency)

* SRM does not support VM snapshots

* VMs cannot span arrays, even arrays of the same type

The presenter next moves into a section he called "Running," but it was really more about how to walk through the site pairing, storage array configuration, and inventory mapping (more setup and configuration than operation, in my opinion).

A few notes about protection groups, placeholder VMs, and recovery plans:

* SRM supports up to 150 protection groups. Each protection group may have multiple LUNs.

* SRM supports up to 500 VMs. This should increase to 1000 VMs per site in the next release.

* Need a datastore at the recovery site to hold VM placeholders. This datastore does not need to be replicated. Each placeholder is typically <1KB each, so there is not a lot of storage required.

* Don't delete the placeholders; this breaks the SRM configuration.

* SRM supports multiple recovery plans, so you can use mutliple plans to support multiple recovery targets.

* Recovery plans support permissions, so not everyone needs access to all plans.

In general, also be sure to keep in mind the "upstream" and "downstream" dependencies. SRM isn't going to fix dependencies for Microsoft Exchange, for example.

I'm ending coverage here, because I have to leave to go to a special, super-secret press announcement with Paul Maritz. I'm not allowed to blog, tweet, or any such thing during the press announcement, so you'll have to wait until after the event to hear anything from me.
