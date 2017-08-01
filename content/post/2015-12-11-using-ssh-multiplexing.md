---
author: slowe
categories: Education
comments: true
date: 2015-12-11T00:00:00Z
tags:
- SSH
- Linux
- CLI
title: Using SSH Multiplexing
url: /2015/12/11/using-ssh-multiplexing/
---

In this post, I'm going to discuss how to configure and use SSH multiplexing. This is yet another aspect of Secure Shell (SSH), the "Swiss Army knife" for administering and managing Linux (and other UNIX-like) workloads.

Generally speaking, multiplexing is the ability to carry multiple signals over a single connection (see [this Wikipedia article][link-2] for a more in-depth discussion). Similarly, SSH multiplexing is the ability to carry multiple SSH sessions over a single TCP connection. [This Wikibook article][link-1] goes into more detail on SSH multiplexing; in particular, I would call your attention to the table under the "Advantages of Multiplexing" to better understand the idea of multiple SSH sessions with a single TCP connection.

One of the primary advantages of using SSH multiplexing is that it speeds up certain operations that rely on or occur over SSH. For example, let's say that you're using SSH to regularly execute a command on a remote host. Without multiplexing, every time that command is executed your SSH client must establish a new TCP connection and a new SSH session with the remote host. With multiplexing, you can configure SSH to establish a single TCP connection that is kept alive for a specific period of time, and SSH sessions are established over that connection. As pointed out in [the Wikibooks article][link-1] (see the measurements just before the "Setting up Multiplexing" section), this can result in speed increases that can add up when repeatedly running commands against remote SSH hosts.

I see a couple of use cases, then, emerging for the use of SSH multiplexing:

* _When running commands remotely on an SSH host:_ I mentioned this in the previous paragraph, but note that this isn't just about instances where you're using `ssh user@host some-command` to execute a command, but it also applies to applications that operate over SSH ([Ansible][link-3] is a great example).
* _When using an SSH bastion host:_ I discussed the use of an SSH bastion host in [an earlier post][xref-1] (as well as [one potential use case][xref-2]). When using an SSH bastion host, multiple SSH sessions are channeled through the bastion, making it a natural use case for multiplexing.

Now that I've established a bit of a foundation for SSH multiplexing and why it might be useful, let's look at how it is configured. The configuration is actually really straightforward. Here's a sample configuration that enables SSH multiplexing for a specific host:

    Host demo-server.domain.com
      ControlPath ~/.ssh/cm-%r@%h:%p
      ControlMaster auto
      ControlPersist 10m

This is a fairly standard SSH configuration block, but let's break it down a bit:

* The `Host` keyword starts the SSH configuration block and specifies the name (or pattern of names, as you can use globbing expressions like `*.domain.com` here) to which the configuration entries will apply. In this case, it's a specific server.
* The `ControlPath` entry specifies where to store the "control socket" for the multiplexed connections. In this case, `%r` refers to the remote login name, `%h` refers to the target host name, and `%p` refers to the destination port. Including this information in the control socket name helps SSH separate control sockets for connections to different hosts.
* The `ControlMaster` setting is what activates multiplexing. With the `auto` setting, SSH will try to use a master connection if one exists, but if one doesn't exist it will create a new one (this is probably the most flexible approach, but you can refer to [ssh-config(5)][link-4] for more details on the other settings).
* Finally, the `ControlPersist` setting keeps the master connection alive for the specified period of time after it has remained idle (no connections). After that time, the master connection will be closed. In this example, we've specified that the master connection should remain open for 10 minutes after becoming idle. Subsequent SSH sessions made while the master connection is open will leverage the master connection and will reset the idle timer.

With this sort of configuration, no extra effort is required on your part, as the user, to take advantage of SSH multiplexing. You'll establish your master connection when you connect to the remote server for the very first time (using a standard `ssh user@host` sort of command). That creates the master connection, and that master connection will remain open for 10 minutes (or whatever you've specified for `ControlPersist`). When you connect again (still using the standard `ssh user@host` command) while the master connection is still open, SSH will automatically leverage the master connection.

I mentioned earlier that one use case for SSH multiplexing was in conjunction with an SSH bastion host. For example, consider this SSH configuration:

    Host bastion
      Hostname server.example.com
      ForwardAgent yes
      ControlPath ~/.ssh/cm-%r@%h:%p
      ControlMaster auto
      ControlPersist 10m

    Host 172.16.*
      ProxyCommand ssh user@bastion -W %h:%p

In this case, the first time you connect to an SSH host in the 172.16.* address space, SSH will establish a master connection to the bastion host (`server.example.com`, in this case). The connection to the private server will be proxied through the bastion host. When you connect to a second SSH host in the 172.16.* address space, the _existing_ master connection to the bastion host is leveraged, and a new SSH session is established with the second private server. The same goes for _all_ subsequent SSH sessions to servers in the private address space that are being proxied through the bastion host---they all share the same master connection to the bastion. Naturally, this can make connecting to the private servers through the bastion faster than it would be otherwise without SSH multiplexing.

Note that some older versions of OpenSSH may have had some issues with combining `ProxyCommand` and `ControlMaster`, so make sure you do some testing. I tested with relatively recent versions of OpenSSH (OpenSSH 6.9p1 on OS X 10.11.2 and OpenSSH 6.6.1p1 on Ubuntu 14.04 LTS) and was not able to reproduce any issues. Your mileage may vary, of course.

I hope you've found this article helpful. If you have any questions, I would love to chat with you [on Twitter][link-5]. Thanks for reading!

**UPDATE:** Jake Robinson pointed out to me on Twitter that OpenSSH 5.1 added a `MaxSessions` parameter to `sshd_config`. The default value is 10, meaning that at most 10 multiplexed connections are allowed. See [the OpenSSH 5.1 release notes][link-6] for full details. Thanks Jake!


[link-1]: https://en.wikibooks.org/wiki/OpenSSH/Cookbook/Multiplexing
[link-2]: https://en.wikipedia.org/wiki/Multiplexing
[link-3]: http://www.ansible.com/
[link-4]: http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man5/ssh_config.5?query=ssh%5fconfig&arch=i386
[link-5]: https://twitter.com/scott_lowe
[link-6]: http://www.openssh.com/txt/release-5.1
[xref-1]: {{< relref "2015-11-21-using-ssh-bastion-host.md" >}}
[xref-2]: {{< relref "2015-12-04-use-case-ssh-bastion-host.md" >}}
