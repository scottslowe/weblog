---
author: slowe
categories: News
comments: true
date: 2010-09-09T10:00:00Z
slug: keep-your-eyes-on-hytrust
tags:
- Cisco
- Networking
- Security
- Virtualization
- VMware
title: Keep Your Eyes on HyTrust
url: /2010/09/09/keep-your-eyes-on-hytrust/
wordpress_id: 2097
---

I'm not a security expert (I'll leave that to [Ed](http://www.astroarch.com/blog/) or [the Hoff](http://www.rationalsurvivability.com/blog/)), but if there's a security company out there to keep your eyes on it is, in my opinion, HyTrust. Since [releasing their security appliance][1] in April of 2009, HyTrust has continued to expand their reach. Last week at VMworld 2010 in San Francisco, HyTrust made a few announcements to note:

* On August 30, HyTrust announced HyTrust Cloud Control and out-of-the-box integration between the HyTrust Appliance and VMware vCloud Director. This combination brings HyTrust's strong authentication, role-based access control, and visibility to vCloud Director environments. Other specific capabilities enabled by HyTrust Cloud Control include persistent zoning for multi-tenancy; detailed audit logging for compliance; and hardening and monitoring of the cloud services platform.

* On August 31, HyTrust announced integration with RSA enVision _(disclaimer: I work for EMC, RSA's parent company)_. This means that HyTrust's detailed logging and auditing information is passed to enVision for security information and event management purposes. The HyTrust Appliance offers granular role-based access controls, strong authentication, directory services integration, and command authorization, and with this integration passes all of its detailed logging information over to enVision to be rolled up into a broader set of logs that also include information from the VMware ESX/ESXi hosts, VMware vCenter Server, and VMware View connection servers for a holistic view of the entire virtualized environment. You can read the full press release [here](http://www.businesswire.com/news/home/20100831006388/en/HyTrust-Teams-RSA-Security-Division-EMC-Enable).

* On September 1, HyTrust announced an update to the HyTrust Appliance that added new functionality. Significant new features in the update include support for smart card two-factor authentication; support for complex, multi-domain directories; single sign-on via Windows passthrough authentication; improvements to audit logs and new vCenter event archiving; application-level high availability for the HyTrust Appliance; support for VMware vSphere 4.1; and support for command-line management of the Cisco Nexus 1000V. This last item is particularly important; it enables the HyTrust Appliance to perform authorization of Nexus 1000V command line statements on a very granular basis. This functionality actually extends to the entire Nexus family, although the focus at this point is on the Nexus 1000V.

All in all, it looks to me like a pretty impressive set of updates. Based on a conversation between Eric Chiu (CEO of HyTrust), well-known analyst Chris Wolf, and me, I'd say that HyTrust has other impressive updates on the roadmap. Based on what they've delivered so far, I'm of the opinion that this is a company to watch. Keep up the great work, Eric and team!

Disclosure: I have no financial interest in HyTrust nor have I received any compensation from HyTrust. These views and opinions of HyTrust are mine and mine alone.

[1]: {{< relref "2009-04-06-hytrust-launches-security-appliance.md" >}}
