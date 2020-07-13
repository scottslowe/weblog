---
author: slowe
categories: Education
comments: true
date: 2015-11-21T00:00:00Z
tags:
- Linux
- SSH
- CLI
- Networking
- Security
title: Using an SSH Bastion Host
url: /2015/11/21/using-ssh-bastion-host/
---

Secure Shell, or SSH, is something of a "Swiss Army knife" when it comes to administering and managing Linux (and other UNIX-like) workloads. In this post, I'm going to explore a very specific use of SSH: the _SSH bastion host_. In this sort of arrangement, SSH traffic to servers that are not directly accessible via SSH is instead directed through a bastion host, which proxies the connection between the SSH client and the remote servers.

At first, it may sound like the use of an SSH bastion host is a pretty specialized use case. In reality, though, I believe this is a design pattern that can actually be useful in a variety of situations. I plan to explore the use cases for an SSH bastion host in a future blog post.

This diagram illustrates the concept of using an SSH bastion host to provide access to Linux instances running inside some sort of cloud network (like an OpenStack Neutron tenant network or an AWS VPC):

![SSH bastion host diagram](/public/img/ssh-bastion-host.png)

Let's take a closer look at the nuts and bolts of actually setting up an SSH bastion host.

First, you'll want to ensure you have public key authentication properly configured, both on the bastion host as well as the remote instances. This is a topic that has been discussed extensively in other locations ([here][link-2], for example), so I won't cover it here in any great detail. You can use the same key for both the bastion host and the remote instances, or different keys; you'll just need to ensure that the keys are loaded by `ssh-agent` appropriately so they can be used as needed. (Note that the use of public key authentication isn't strictly _required_, but it's something you really should do.)

Next, you'll want to ensure that name resolution is working---both from the client to the bastion as well as from the bastion to the remote instances. The bastion host is going to use the hostname specified on the `ssh` command line, so if it can't resolve the name the connection will fail.

Finally, you'll want to configure the `ProxyCommand` setting for the remote instances in your SSH configuration file. For example, let's say that you had a remote instance named "private1" and you wanted to run SSH connections through a bastion host called "bastion". The appropriate SSH configuration might look something like this:

    Host private1
      IdentityFile ~/.ssh/rsa_private_key
      ProxyCommand ssh user@bastion -W %h:%p

    Host bastion
      IdentityFile ~/.ssh/bastion_rsa_key

With this configuration in place, when you type `ssh user@private1` SSH will establish a connection to the bastion host and then _through the bastion host_ connect to "private1", using the specified keys. The user won't see any of this; he or she will just see a shell for "private1" appear. If you dig a bit further, though (try running `who` on the remote node), you'll see the connections are coming from the bastion host, not the original SSH client. Handy, eh?

In a future blog post, look for me to explore one or more examples of where using an SSH bastion host might be very useful (or even necessary).

## Additional Resources

I found [this article][link-1] _(update: try [this Wayback machine version][link-3]; it seems the original has gone offline)_ to be extraordinarily helpful while working through the details of an SSH bastion host configuration.

**UPDATE 13 Sep 2016:** I've removed the references to the need for SSH agent forwarding, as it turns out this isn't required (despite indications otherwise from other tutorials). See [this follow-up post][xref-1] for more information.

**UPDATE: 13 Jul 2020:** It seems one of my original links has gone offline (thanks Lucas!), so I've added a link to the Wayback Machine's archived copy.

[link-1]: https://10mi2.wordpress.com/2015/01/14/using-ssh-through-a-bastion-host-transparently/
[link-2]: https://kb.iu.edu/d/aews
[link-3]: http://web.archive.org/web/20170413054605/https://10mi2.wordpress.com/2015/01/14/using-ssh-through-a-bastion-host-transparently/
[xref-1]: {{< relref "2016-09-13-ssh-bastion-host-follow-up.md" >}}
