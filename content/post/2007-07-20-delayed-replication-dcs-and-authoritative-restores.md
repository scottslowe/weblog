---
author: slowe
categories: Tutorial
comments: true
date: 2007-07-20T14:31:17Z
slug: delayed-replication-dcs-and-authoritative-restores
tags:
- ActiveDirectory
- Microsoft
- Networking
title: Delayed Replication DCs and Authoritative Restores
url: /2007/07/20/delayed-replication-dcs-and-authoritative-restores/
wordpress_id: 492
---

The idea behind an Active Directory "delayed replication DC" (also referred to as a "slow DC" or a "lag DC") is that organizations can more quickly recover portions of their Active Directory structure by performing an authoritative restore from this delayed replication DC instead of having to go back to tape. Keep in mind that many organizations use off-site storage of backup tapes, and recalling backup tapes from the off-site storage facility can take some time, as can the tape operations themselves. Let's face it: when it comes to speed, tape isn't exactly king of the hill.

With a delayed replication DC (this would be a DC that only replicates on specific days of the week or after an extended period of time, like 72 hours), however, that process can be shortened drastically because recovering an OU, a user object, a group, or even AD-integrated DNS data can be done directly from the delayed replication DC.

First, let's look at setting up a delayed replication DC, then have a look at the process of performing an authoritative restore from that DC.

## Setting Up a Delayed Replication DC

Setting up a delayed replication DC (hereafter referred to as a delayed-rep DC) is not terribly complicated:

1. Create a subnet object with a 32-bit mask representing the IP address of the new DC that will become the delayed-rep DC. For example, if the new DC will have the IP address 192.168.213.150, create a subnet object (in Active Directory Sites and Services) for 192.168.213.150/32. Doing it this way means that we don't have to create a new subnet or VLAN and potentially waste IP addresses, and simplifies the networking configuration (no need to worry about routing tables or anything like that).

2. Create a new Active Directory site and link the new subnet you just created to that site.

3. Create a new site link between the new AD site and an existing AD site with production domain controllers. Don't worry about setting the replication interval just yet; we'll do that after the initial replication takes place.

4. Create a new GPO, linked to this new site, that will prevent registration of specific DNS SRV records for the delayed-rep DC. This is the only (relatively) complicated part. [This article by Gary Olsen](http://searchwinit.techtarget.com/tip/0,289483,sid1_gci1172883,00.html) (a well-respected AD expert) provides more information on the specific settings that should be included in this GPO. (More information available [here](http://searchwinit.techtarget.com/tip/0,289483,sid1_gci1086805,00.html) and in Microsoft [KB306602](http://support.microsoft.com/kb/306602/en-us) as well---refer to the "To Configure Domain Controllers or global catalogs to Not Register Generic Records" section for [Windows Server 2003](http://www.microsoft.com/windowsserver/default.mspx).) When specifying the mnemonics to prevent the registration of DNS SRV records, be sure to allow the A record (for the hostname-to-address lookup of the server) and the GUID CNAME record (used for establishing replication connections).

5. Promote the new DC (making sure its IP address matches the address specified during the creation of the subnet above!) and it will automatically place itself into the delayed-rep site.

6. Once you are satisfied that initial replication is complete, adjust the replication schedule on the site link to the delayed replication DC.

See, that wasn't so bad! The cool thing is that you can use a virtual machine as your delayed-rep DC to further save some costs/rack space/equipment.

Once the delayed-rep DC is up and running, then you are ready to use it to perform authoritative restores of AD objects.

## Performing Authoritative Restores from the Delayed-Rep DC

The process for performing an authoritative restore using this delayed-rep DC is also pretty straightforward. Again, there is one minor complication:

1. Reboot the delayed-rep DC into Directory Services Restore Mode (DSRM). You'll need the DSRM password to log in; you _do_ know it, right? (If not, you can reboot the DC normally, then use Ntdsutil to set the DSRM password; see [here](http://support.microsoft.com/kb/322672).)

2. Once you log on, use `ntdsutil` to mark the desired objects as authoritatively restored (more on that in a moment).

3. Reboot the server into normal operation.

4. Force outbound replication from the delayed-rep DC to the rest of the organization.

It's item #2 that's the complicated part this time. First, because it's `ntdsutil`, and not many people know/use `ntdsutil`. Second, because you **_must_** know the full DN of the object or objects you want to mark as authoritatively restored.

Fortunately, you can use ADSI Edit to help you find the full DN. (Those of you that did not install the Support Tools on your delayed-rep DCs may take a moment to go and install them. Done now? Good, let's continue.) There are a couple of gotchas, though, even with using ADSI Edit:

* You'll need to put double quotes around the DN if there are any spaces anywhere along the way.  ADSI Edit doesn't do this, so be sure to add them yourself.

* Any commas in the individual components of the DN must be escaped with a backslash (\) character. For example, in situations where the user object is named "Smith, Bob" you'll need to reference that as "Smith\, Bob" in the DN.

* Don't forget to use the right components in the DN---CN=, OU=, or DC=. More on that in a moment.

Let's look at some examples. First, let's say we need to restore a user object named "John Doe" in the Raleigh OU under the Locations OU in the domain example.com. The DN would look like this:

	"cn=John Doe,ou=Raleigh,ou=Locations,dc=example,dc=com"

What if the user accounts are listed with lastname then firstname, separated by a comma?

	"cn=Doe\, John,ou=Raleigh,ou=Locations,dc=example,dc=com"

What about a group?

	cn=DnsAdmins,cn=Users,dc=example,dc=com

Note that we omit the double quotes here because there are no spaces in the DN.

What if what we want to restore isn't a user object at all, but instead is an Active Directory-integrated DNS zone? Good question!

In this case, the answer is "It depends". Upon what does it depend? The zone's replication scope:

* For zones with a replication scope of "All domain controllers in the Active Directory domain" (or if you are still running Windows 2000), then the correct location for these zones is "cn=MicrosoftDNS,cn=System,dc=example,dc=com".

* For zones with a replication scope of "All DNS servers in the Active Directory domain", then the correct location for these zones is "cn=MicrosoftDNS,dc=DomainDnsZones,dc=example,dc=com".

* For zones with a replication scope of "All DNS servers in the Active Directory forest", then the correct location for these zones is "cn=MicrosoftDNS,dc=ForestDnsZones,dc=example,dc=com".

The tricky part about restoring AD-integrated DNS zones is the naming in the DN. Here's an example:

	"dc=testlab.example.com,cn=MicrosoftDNS,dc=DomainDnsZones,dc=example,dc=com"

This would be for the zone testlab.example.com, configured as a AD-integrated zone with a replication scope of all DNS servers in the domain, in the AD domain example.com. Key thing to note here: pay attention to the use of "dc=" at the beginning of the DN. Zones use "dc=" and _not_ "cn="! (Fortunately this is correctly represented with ADSI Edit.)

OK, enough about DNs. Once you have the DN and understand how it should be represented when using `ntdsutil`, you can proceed with the actual authoritative restore itself. Refer to [this Microsoft web page](http://technet2.microsoft.com/windowsserver/en/library/1b707085-98cb-4282-a67d-6406b9aacf191033.mspx?mfr=true) for more details on the specific Ntdsutil commands to perform an authoritative restore. Once the authoritative restore process is complete, reboot the delayed-rep DC normally and force replication with the rest of the organization. Your objects should magically re-appear in Active Directory!

There's more to this process than I've described here, but there are a ton of resources available to provide more in-depth information and explanations of the various scenarios. This information should at least get you started.
