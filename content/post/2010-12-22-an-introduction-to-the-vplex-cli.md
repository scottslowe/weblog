---
author: slowe
categories: Education
comments: true
date: 2010-12-22T09:00:00Z
slug: an-introduction-to-the-vplex-cli
tags:
- CLI
- EMC
- Storage
- VPLEX
title: An Introduction to the VPLEX CLI
url: /2010/12/22/an-introduction-to-the-vplex-cli/
wordpress_id: 2194
---

The VPLEX command line interface (CLI) is a bit different than a lot of other CLIs with which I've worked. In some respects, it's similar to the scope-based CLI that Cisco uses with Unified Computing System (UCS). In this post, I'd like to explore the VPLEX CLI a bit and provide a brief introduction to the VPLEX CLI. Keep in mind that this is just an introduction---for a complete reference to the VPLEX CLI, I'll refer you to the VPLEX product area on EMC PowerLink where a complete VPLEX CLI User's Guide is available in PDF.

In my opinion, there are two things to know about the VPLEX CLI that will help you understand how to use it:

1. First, the CLI is a lot like navigating a directory tree, except that you're navigating through _contexts_ instead of directories. In the VPLEX CLI, you move between contexts using the `cd` command and list the contents of a context using the `ls` and `ll` commands. Just as with a typical filesystem, if you're in the right context, you can just run a command (say, like the `unclaim` command). If you're not in the right context, you simply specify the full context before the command (I'll show you some examples later in this post).

2. Second, the VPLEX CLI supports wildcards and _globbing_, much like a typical filesystem. Use the asterisk (\*) character to represent anything in a context, and use the double asterisk (\*\*) to represent multiple matches across multiple contexts. You can use regular expressions as well.

With these concepts in mind, the real key to using the VPLEX CLI is simply learning where all the various objects reside. For example, in an earlier post on [VPLEX storage objects][1], I discussed the building blocks of storage in a VPLEX environment: storage volumes, extents, devices, and virtual volumes. These reside in different places in the VPLEX contextual tree:

Storage volumes: `/clusters/<cluster name>/storage-elements/storage-volumes`  

Extents: `/clusters/<cluster name>/storage-elements/extents`  

Devices: `/clusters/<cluster name>/devices`  

Distributed devices: `/distributed-storage/distributed-devices`  

Virtual volumes: `/clusters/<cluster name>/virtual-volumes`

Within each of these contexts, you can use the `ls` and `ll` commands to view the contents of that context. The `help` command will show you what other commands are available in each context.

Here's my first example. When you first log into the VPLEX CLI, you'll get dropped into a "root context". Running `ls` here will produce output something like this:

	VPlexcli:/> ls  
	clusters data-migrations distributed-storage engines management-server  
	monitoring notifications system-defaults

Let's take a look at a few more examples. For example, the following two sets of commands will produce the same output:

	cd /clusters  
	ll

and

	ll /clusters

Remember that specifying the context in the command is the same as changing into that context and then running the command. That is why these two commands produce the same output.

To view the list of directors in a cluster:

	ll /engines/*/directors

This is an example of using the asterisk to represent a wildcard that matches all entries in that context.

Let's look at another example of two different commands that produce the same output. To view the status of the ports on a particular director in a cluster, you could use either of these two commands:

	ll /engines/<engine name>/directors/<director name>/hardware/ports

or

	cd /engines/<engine name>/directors/<director name>/hardware/ports  
	ll

By the way, if you haven't figured it out yet, you can easily get the names of the engines (or the directors) by simply running the `ll` command against the `engines` context or the `directors` context within a specific engine.

But what if you wanted to see the status of the ports across multiple directors? Now you won't want to use the `cd` command. You'll want to use globbing with wildcards instead, like this:

	ll /engines/**/ports

While the single asterisk matches anything within a context, the double asterisk matches across multiple contexts. So, in this case, it ends up matching multiple engines and directors within those engines.

And if you wanted to see only some of the front-end ports on all directors:

	ls /engines/**/hardware/ports/*1-FC0[0-3]

In some cases, you might need to set an attribute on an object within a context. For example, you might need to enable a port by setting the enabled attribute on that port to True. Here's how you would do that:

	set /engines/<engine name>/directors/<director name>/hardware/ports/A0-FC00::enabled true

If you think about it, you could easily combine some globbing to enable multiple ports at the same time:

	set /engines/**/hardware/ports/A0-FC0[0-3]::enabled true

You can then verify the operation by using the `-t` parameter to the `ls` command, which instructs it to include attributes:

	ls -t /engines/**/hardware/ports/A0-FC0[0-3]::enabled

There are many, many more examples I could share with you, but this should get you started for now. I hope to have a post up soon with a CLI guide to setting up storage volumes, extents, devices, and virtual volumes.

As usual, feel free to speak up in the comments if you have any questions or clarifications. Thank you!

[1]: {{< relref "2010-12-13-a-quick-review-of-vplex-storage-objects.md" >}}
