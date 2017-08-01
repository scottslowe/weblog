---
author: slowe
categories: Education
comments: true
date: 2009-08-24T14:11:07Z
slug: new-users-guide-to-configuring-cisco-mds-zones-via-cli
tags:
- Cisco
- FibreChannel
- Networking
- Storage
title: New User's Guide to Configuring Cisco MDS Zones via CLI
url: /2009/08/24/new-users-guide-to-configuring-cisco-mds-zones-via-cli/
wordpress_id: 1556
---

I'm a bit new to the Cisco MDS family of Fibre Channel switches, so I'm sure that this information is "old hat" to the storage pros out there who've done it a million times. Hence, I'm labeling this one as a "new user" article. The topic of this post is how to use the command-line interface (CLI) to configure zones on a Cisco MDS 9000 series Fibre Channel switch.

I won't go into great detail on the purpose of zones and that sort of thing; I'm sure it's been covered in excruciating detail elsewhere. (Knowledgeable readers with any links to that sort of information are encouraged to share those links in the comments.) Instead, I'll just focus on the mechanics of how it's done.

First, create some aliases for your own use instead of having to remember the Fibre Channel World Wide Port Names (WWPNs). This will make life a _lot_ easier, in my opinion. You create aliases using the `fcalias` command, like this (where applicable in this command and all other commands in this post, replace `_XXX_` with the appropriate VSAN number):

	switch(config)# fcalias name stor-array-processor-a vsan _XXX_  
	switch(config-fcalias)# member pwwn _AA:BB:CC:DD:EE:FF:00:11_  
	switch(config-fclias)# exit  
	switch(config)#

Obviously, you'll replace the fake WWPN I used in the command above with the correct WWPN for that device. Repeat this process for all the storage processor ports, server HBAs, etc. From this point forward, you can use the alias in place of the WWPN when creating zones. See, isn't that easier?

Next, create zones. Each zone should have a single initiator and (ideally) a single target, although multiple targets is usually acceptable. To create a zone, use the `zone` and `member` commands like this:

	switch(config)# zone name first-new-zone vsan _XXX_  
	switch(config-zone)# member fcalias stor-array-processor-a  
	switch(config-zone)# member fcalias server-hba  
	switch(config-zone)# exit  
	switch(config)#

Since each zone contains only a single initiator, you'll need to repeat this process for each initiator.

Once you have all the zones created, next create a zoneset. You can create a new zoneset just using the `zoneset` command, or you can clone an existing zoneset with the `zoneset clone` command. In this case, I'll clone an existing zoneset:

	switch(config)# zoneset clone existing-zoneset new-zoneset vsan _XXX_

From here, you have a copy of the existing zoneset, which already had all the previously defined zones as members. Add the new zones you've defined to the zoneset like this:

	switch(config)# zoneset new-zoneset vsan _XXX_  
	switch(config-zoneset)# member first-new-zone  
	switch(config-zoneset)# member second-new-zone  
	switch(config-zoneset)# exit

Finally, activate the zoneset:

	switch(config)# zoneset activate name new-zoneset vsan _XXX_

Then save the configuration with `copy runn start` and you should be good to go! All you need to do now is configure and present storage from the storage array to the initiators. But that's another topic for another post...

**UPDATE:** I've posted a follow-up to this article on [managing zones via the CLI][1].

[1]: {{< relref "2009-10-20-new-users-guide-to-managing-cisco-mds-zones-via-cli.md" >}}