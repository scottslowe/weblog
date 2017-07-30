---
author: slowe
categories: Information
comments: true
date: 2008-07-11T13:04:54Z
slug: vmware-ha-configuration-notes
tags:
- ESX
- Networking
- Virtualization
- VMware
- VMwareHA
title: VMware HA Configuration Notes
url: /2008/07/11/vmware-ha-configuration-notes/
wordpress_id: 759
---

I recently wrapped up some testing in my lab around VMware HA; specifically, around VMware HA isolation response. My tests involved various network configurations and attempted to clearly document the behavior of VMware HA isolation response under different circumstances. I thought I'd share some of my findings here in the hopes that others would find this information useful as well. (Keep in mind that some of the stuff listed below is just common sense, but I'm including it here anyway just for completeness.)

* Ensure that the vSwitch hosting the Service Console has at least two uplinks. Keep in mind that instead of leaving that second uplink primarily unused, you can place other traffic on the same vSwitch and use the "Override vSwitch failover order" option to direct traffic preferentially onto certain uplinks. (I'll most likely post a separate blog entry about that so that I can explain that in more detail.)

* Ensure that DNS is working correctly on all ESX hosts in the HA-enabled cluster. You should verify host name resolution for both short names as well as fully-qualified domain names (FQDNs). Although I've seen numerous recommendations to hard-code entries into `/etc/hosts`, this approach is difficult to manage and does not scale well. Just fix DNS instead.

* Ensure that the Service Console's default gateway responds to ping. If it does not, you'll need to use the "das.usedefaultgateway" and "das.isolationaddress" parameters to change where VMware HA should check to see if it is isolated. Chad Sakac recently [discussed these items](http://virtualgeek.typepad.com/virtual_geek/2008/06/arghhh-oh-that.html) as well, so check that entry for additional information.

* In a Cisco networking environment, ensure that Portfast is enabled on all physical switch ports. This will help reduce the possibility of an isolation response occurring due to transient network issues. Otherwise, the delay to put the port into a forwarding state is longer than the isolation response timeout, and a brief loss of connectivity could easily result in triggering VMware HA isolation response.

* If you are going to use a second Service Console port, be sure to specify a different IP subnet for the matching vswif interface. Otherwise, the Service Console's routing table gets involved and tries to route everything through vswif0. That kind of defeats the purpose behind the secondary Service Console. My tests showed that isolation response was triggered every single time connectivity to vswif0 was lost when the secondary Service Console shared the same IP subnet as the primary Service Console interface.

* It should go without saying, but be sure that the secondary Service Console port is placed on a different vSwitch than the primary Service Console port. (Common sense, I know, but it's worth pointing out anyway.)

* My tests have not shown that it's not necessary to use a secondary isolation address when using multiple Service Console ports. The same post by Chad I linked to earlier seems to imply (unless I'm reading it incorrectly) that you _should_ have multiple isolation addresses. I'm certainly open to any additional clarification any readers may be able to provide.

If you have any additional information or recommendations to share, please include them in the comments.
