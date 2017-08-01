---
author: slowe
categories: Information
comments: true
date: 2016-09-13T00:00:00Z
tags:
- Linux
- SSH
- CLI
- Networking
- Security
title: A Follow Up on SSH Bastion Hosts
url: /2016/09/13/ssh-bastion-host-follow-up/
---

This post is a follow-up on my earlier post on [using an SSH bastion host][xref-1]. Since that article was published, I've gotten some additional information that I wanted to be sure to share with my readers. It's possible that this additional information may not affect you, but I'll allow you to make that determination based on your use case and your specific environment.

## Agent Forwarding

You may recall that my original article said that you needed to enable agent forwarding, either via the `-A` command-line switch or via a `ForwardAgent` line in your SSH configuration file. **This is unnecessary.** (Thank you to several readers who contacted me about this issue.) I tested this several times using AWS instances, and was able to transparently connect to private instances (instances without a public IP address) via a bastion host without enabling agent forwarding. This is odd because almost _every_ other tutorial I've seen or read instructs readers to enable agent forwarding. I've not yet determined why this is the case, but I'm going to do some additional testing and I'll keep readers posted as I learn more.

Note that I've updated the original article accordingly.

## The "-W" Parameter vs. Netcat

The original `ProxyCommand` I shared with you was this:

    ProxyCommand ssh bastion -W %h:%p

In this command, the `-W` command is a command-line option for SSH that enables "netcat mode." This option was added in OpenSSH 5.4, released in March 2010 (see [the OpenSSH 5.4 release notes][link-1]). If you think you may need to work with earlier versions of SSH, then this could be a problem.

The solution to the problem is to use "real" netcat on the `ProxyCommand`, like this:

    ProxyCommand ssh bastion nc %h %p 2> /dev/null

As with the original command, the `%h` and `%p` parameters expand to host and port, respectively. The `2> /dev/null` at the end of the command suppresses an error message from Netcat when disconnecting from the remote system. This command will work with both newer and older versions of OpenSSH, and therefore may prove to be a better option than the use of the `-W` parameter. (To be fair, though, most recent distributions of Linux will come with a version of OpenSSH that is new enough to support this parameter.)

I hope this information helps!



[link-1]: http://www.openssh.com/txt/release-5.4
[xref-1]: {{< relref "2015-11-21-using-ssh-bastion-host.md" >}}
