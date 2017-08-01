---
author: slowe
categories: Education
comments: true
date: 2009-10-20T18:02:26Z
slug: new-users-guide-to-managing-cisco-mds-zones-via-cli
tags:
- Cisco
- CLI
- FibreChannel
- Networking
- Storage
title: New User's Guide to Managing Cisco MDS Zones via CLI
url: /2009/10/20/new-users-guide-to-managing-cisco-mds-zones-via-cli/
wordpress_id: 1694
---

Toward the end of August 2009, I posted an article on [how to configure Cisco MDS zones via the command-line interface (CLI)][1]. This article is a follow-up to that article; in this post, I'll review some commands that are helpful in _managing_ those zones.

As with the first post, this post probably won't be very helpful to users who are well-versed with the Cisco MDS family of Fibre Channel switches. Hence, why I've tagged it as a "new user's" post. Similarly, I'm not going into the need for zones, as that is covered amply elsewhere.

First, I find it extremely handy to be able to rename Fibre Channel aliases using the `fcalias rename` command like this (replace "\_XXX\_" with the appropriate VSAN number for your environment):

	switch(config)# fcalias rename <old alias> <new alias> vsan _XXX_

You can also rename zones:

	switch(config)# zone rename <old zone name> <new zone name> vsan _XXX_

And you can rename zonesets:

	switch(config)# zoneset rename <old zoneset name> <new zoneset name> vsan _XXX_

In my earlier article I talked about the `zoneset clone` command, but you can also clone aliases and individual zones. I'm not yet convinced of the value of being able to clone an individual alias, and if you are using single initiator/single target zoning I'm not 100% sure how helpful it will be to clone a specific zone. Still, the functionality is there if you need it.

Adding a new alias, zone, or zoneset is similar to modifying an existing alias, zone, or zoneset. For example, to add a new alias to an existing zone, you would use these commands:

	switch(config)# zone name existing-zone-name-here vsan _XXX_  
	switch(config-zone)# member fcalias new-alias-to-add  
	switch(config-zone)# exit

Likewise, adding a new zone to an existing zoneset is similar to defining a new zoneset:

	switch(config)# zoneset name existing-zoneset-name vsan _XXX_  
	switch(config-zoneset)# member new-zone-to-add  
	switch(config-zoneset)# member another-new-zone  
	switch(config-zoneset)# exit

Managing zones via the CLI can be a bit daunting; as the number of aliases and zones increases, it becomes more difficult to work with all of them and find only the ones in which you are interested at the moment. Here, using the `include` keyword can be rather handy. Consider this command:

	switch# show zone | include server-name  
	zone name **server-name**-storage vsan _XXX_  
	fcalias name **server-name** vsan _XXX_  
	zone name **server-name**-storage2 vsan _XXX_  
	fcalias name **server-name** vsan _XXX_

I've marked the matching text in asterisks, so that you can see that the `include` keywords acts like a bit like `grep`. This makes it much easier to filter out only the zones you want or need to see, instead of having to wade through all the currently defined zones. This is not an MDS-specific trick; it's also applicable in IOS and NX-OS as well. And it works not only with zones, but also with zonesets, FC aliases, etc.

Cisco MDS experts, feel free to post additional suggestions on managing zones via the CLI in the comments below so that all readers can benefit. Thanks for reading!

[1]: {{< relref "2009-08-24-new-users-guide-to-configuring-cisco-mds-zones-via-cli.md" >}}
