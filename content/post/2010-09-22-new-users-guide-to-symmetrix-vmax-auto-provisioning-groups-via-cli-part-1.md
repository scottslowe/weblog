---
author: slowe
categories: Education
comments: true
date: 2010-09-22T13:41:20Z
slug: new-users-guide-to-symmetrix-vmax-auto-provisioning-groups-via-cli-part-1
tags:
- CLI
- EMC
- Storage
- Symmetrix
title: New User's Guide to Symmetrix VMAX Auto Provisioning Groups via CLI, Part 1
url: /2010/09/22/new-users-guide-to-symmetrix-vmax-auto-provisioning-groups-via-cli-part-1/
wordpress_id: 2109
---

Readers who also [follow me on Twitter](http://twitter.com/scott_lowe) have probably noticed that I've been in an EMC Symmetrix VMAX training class this week. As part of that class, we've been working with Auto-Provisioning Groups. For other users who might be new to SYMCLI (the Symmetrix CLI) and Auto-Provisioning Groups (APGs), I thought this "new user's guide" to APGs via the CLI might be handy. Comments from experienced users are welcome! In future posts, I'll provide some additional commands for modifying and viewing APGs via CLI.

Note that this guide doesn't discuss the requirements for SYMCLI (such as Solutions Enabler) or other prerequisites. I'll likely cover those pieces in future blog posts.

An APG, at its most basic level, consists of four items:

1. A storage group containing one or more devices or device groups

2. A port group containing one or more front-end director ports

3. An initiator group containing one or more host initiator ports, denoted by their World Wide Port Name (WWPN)

4. A masking view that combines a storage group, a port group, and an initiator group

OK, with that (very) basic introduction out of the way, here are some commands to create an APG from the command line. In all the sample command shared here, replace the less than/greater than symbols and their contents with appropriate values.

To create a storage group, use this command:

	symaccess -sid <Symmetrix ID> create -name <Storage group name> -type storage -devs <Device IDs>

The device IDs are the IDs of the Symmetrix Logical Volumes you created using SYMCLI or Symmetrix Management Console (SMC).

To create a port group containing one or more ports from one or more directors, use this command:

	symaccess -sid <Symmetrix ID> create -name <Port group name> -type port -dirports <Director slice:Port number,Director slice:port number...>

The _Director slice:Port number_ would look something like `7e:0` for port 0 on director slice 7e (which would be in enclosure 4).

To create an initiator group, I would first recommend that you create a text file containing a list of all the WWPNs for the initiators in the group, like this:

	WWN:1234567890abcdef  
	WWN:abcdef0123456789  
	...

Once you have the text file created, you can then create an initiator group using this command:

	symaccess -sid <Symmetrix ID> create -name <Initiator group name> -type inititator -file <Text file of initiators>

At this point, you can run `symaccess -sid <Symmetrix ID> list` and you'll see the storage, port, and initiator groups you just created.

To combine these groups into a masking view, use this command:

	symaccess -sid <Symmetrix ID> create view -name <Masking view name> -storgrp <Storage group name> -portgrp <Port group name> -initgrp <Initiator group name>

This creates the masking view that contains all three objects---the storage group, the port group, and the initiator group---and automatically performs the mapping (associating devices with directors) and masking (exposing devices to hosts) operations.

To get more details on the masking view once it is created, use this command:

	symaccess -sid <Symmetrix ID> show view <Masking view name>

The output from this command will show you details on the devices in the storage group, the ports in the port group, and the initiators in the initiator group.
