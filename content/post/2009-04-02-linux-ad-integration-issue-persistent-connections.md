---
author: slowe
categories: Information
comments: true
date: 2009-04-02T14:27:03Z
slug: linux-ad-integration-issue-persistent-connections
tags:
- ActiveDirectory
- Interoperability
- Kerberos
- LDAP
- Linux
- Microsoft
- Windows
title: 'Linux-AD Integration Issue: Persistent Connections'
url: /2009/04/02/linux-ad-integration-issue-persistent-connections/
wordpress_id: 1263
---

A reader contacted me a short while ago to inquire about a problem he was having with his Linux-AD integration efforts. It seems he had recently added a new domain controller (DC) that was intended to be a DC for a disaster recovery (DR) site. When he took this new DR DC offline in order to physically move it to the DR site, some of his AD-integrated Linux systems started failing to authenticate. More specifically, Kerberos continued to work, but LDAP lookups failed. When the reader would bring the DR DC back online, those systems started working again.

There was a clear correlation between the DR DC and the AD-integrated Linux systems, even though the `/etc/ldap.conf` file specifically pointed to another DC by IP address. There was no reference whatsoever, by IP address or host name, to the DR DC. Yet, every time the DR DC was taken offline, the behavior returned on a subset of Linux hosts. The only difference we could find between the affected and unaffected hosts was that the affected hosts were not on the same VLAN as the production domain controllers.

I theorized that Windows' netmask ordering feature, which prioritizes the return of DNS lookups to provide clients with addresses that are "closer" to them, was playing a role here. However, the `/etc/ldap.conf` was using IP addresses, not the domain name or even the fully qualified domain name of a DC. It couldn't be DNS, at least not as far as I could tell.

Upon further investigation, the reader discovered that the affected Linux servers---those that were on a different VLAN than both the production DCs as well as the DR DC---were maintaining persistent connections to the DR DC. (He found this via `netstat`.) When the DR DC went offline, the affected Linux hosts tried to continue to communicate to that DC and that DC only. Once the reader was able to get the affected Linux hosts to drop that persistent connection, he was able to take the DR DC offline and the Linux hosts worked as expected.

So, the real question now becomes: _how (or why) did the Linux servers connect to the DR DC instead of the production DC for which they were configured?_ I think that Active Directory issued an LDAP referral to direct the affected Linux servers to the DR DC as a result of site topology. Perhaps due to an incorrect or incomplete site topology configuration, Active Directory believed the DR DC should handle the VLANs where the affected Linux servers resided. If that is indeed the case, the fix would be to make sure that your AD site topology is correct and that subnets are appropriately associated with sites. Of course, this is just a theory.

Has anyone else seen an issue similar to this? What fix were you able to implement in order to correct it?
