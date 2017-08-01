---
author: slowe
categories: Tutorial
comments: true
date: 2010-12-08T20:25:55Z
excerpt: As a follow up to a few posts I published last year on using the CLI to configure
  and manage Cisco MDS Fibre Channel zones, here is information on using device aliases.
slug: using-device-aliases-on-a-cisco-mds
tags:
- Cisco
- FibreChannel
- SAN
- Storage
title: Using Device Aliases on a Cisco MDS
url: /2010/12/08/using-device-aliases-on-a-cisco-mds/
wordpress_id: 2171
---

Last year, I posted a couple of articles on configuring and managing Cisco MDS Fibre Channel zones via the CLI:

[New User's Guide to Configuring Cisco MDS Zones via CLI][1]  

[New User's Guide to Managing Cisco MDS Zones via CLI][2]

In those posts, I discussed the use of the `fcalias` command to create aliases for World Wide Port Names (WWPNs) on the fabric. A couple of people suggested via Twitter and blog comments that I should use device aliases instead of the the `fcalias` command. As a follow up to those posts, here is some information on using device aliases on a Cisco MDS Fibre Channel switch.

To create a device alias, you'll use the `device-alias database` command in global configuration mode. Once you are in database configuration mode, you can create device aliases using the `device-alias command`, like this:

	mds(config)# device-alias database  
	mds(config-device-alias-db)# device-alias name <Friendly name> pwwn <Fibre Channel WWPN>  
	mds(config-device-alias-db)# exit  
	mds(config)# end

There is an additional step required after defining the device aliases. You must also commit the changes to the device alias database, like this:

	mds(config)# device-alias commit

This commits the changes to the device alias database and makes the device aliases active in the switch.

Once a device alias is created, it applies to that WWPN regardless of VSAN. This means that you only have to define a single device alias for any given WWPN, whereas with the `fcalias` command a different alias needed to be defined for each VSAN. All other things being equal (and they're not equal, as you'll see in a moment), that alone is worth switching to device aliases in my opinion.

Using device aliases also provides a couple other key benefits:

* Device aliases are automatically distributed to other Cisco-attached switches. For example, I defined the device aliases on a Cisco MDS 9134 that was attached to the Fibre Channel expansion port of a Cisco Nexus 5010 switch. The Nexus switch automatically picked up the device aliases. As best I can tell, this is controlled by the `device-alias distribute` global configuration command (or its reverse, the `no device-alias distribute`, which would disable device alias distribution).

* Once a device alias is defined for a WWPN, anytime the WWPN is displayed the device alias is also displayed. So in the output of various commands like `show flogi database`, `show fcns database`, or `show zone` you will see not only the WWPN, but also that WWPN's associated device alias.

* You can use the device alias as the destination with the `fcping` command.

All in all, I see a lot of value in using device aliases over simple Fibre Channel aliases. I'll grant that some of this value is more readily apparent only in homogenous Cisco storage networks, but even in single-switch networks I personally would use device aliases.

To those who suggested I look at device aliases, I thank you! You've made my job easier.

As always, I welcome your feedback! Feel free to speak up in the comments with corrections, clarifications, or suggestions.

[1]: {{< relref "2009-08-24-new-users-guide-to-configuring-cisco-mds-zones-via-cli.md" >}}
[2]: {{< relref "2009-10-20-new-users-guide-to-managing-cisco-mds-zones-via-cli.md" >}}
