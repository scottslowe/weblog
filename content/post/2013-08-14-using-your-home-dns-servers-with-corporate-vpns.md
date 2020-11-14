---
author: slowe
categories: Tutorial
comments: true
date: 2013-08-14T09:00:00Z
slug: using-your-home-dns-servers-with-corporate-vpns
tags:
- macOS
- Networking
- VPN
title: Using Your Home DNS Servers with Corporate VPNs
url: /2013/08/14/using-your-home-dns-servers-with-corporate-vpns/
wordpress_id: 3236
---

Among IT pros, it's quite common these days to have a home lab/home network complete with DNS, DHCP, hypervisors, storage, etc. As IT pros, it's also quite common to have to use a virtual private network (VPN) solution to connect back to our employers' corporate networks in order to get work done. In this post, I'll show you how you can reconcile these two (sometimes conflicting) situations with a relatively little-known feature in OS X. (This post builds upon an article on this same feature that I [published in 2006][1].)

Like many other IT pros, I have my own home network, and I use a custom domain name for that home network. Since this custom domain name doesn't exist outside my home network, I also have DNS servers on the home network, and I configure the systems on my home network to use these internal DNS servers via DHCP (or static assignment). No big deal, right?

It's not a big deal until it comes time to connect to the corporate network via VPN. Like many other corporate VPN solutions, my employer's VPN solution changes the DNS configuration on systems that connect so that these remote systems perform DNS queries via the corporate DNS servers. This is what allows access to the usual `server.internal.dept.company.com` systems, whether those are wikis, web portals, expense reporting systems, or whatever else. Clearly this is a necessary piece, as without being able to resolve internal corporate DNS queries we'd be unable to access the systems we need to access. The unfortunate drawback is that it breaks the home DNS configuration---which means now you (typically) lose access to internal home systems.

Fortunately for OS X users, there is a workaround. (This workaround might also work for Linux and other BSD-derived systems; I haven't tested it there yet. I don't know of any similar functionality for Windows.)

The workaround involves taking advantage of what is known as multi-client support in the OS X resolver. (See [this online copy](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man5/resolver.5.html) of the `resolver (5)` man page for more details.) Basically, what this means is that you have the ability to create domain-specific DNS configurations. So, you could have a "default" DNS configuration---this would be stored in `/etc/resolv.conf` and maintained automatically by OS X via changes in the Network panel of the System Preferences tool---as well as a series of domain-specific DNS configurations. When the DNS resolver client needs to resolve a name to an IP address, it will consult the hierarchy of domain-specific configurations, falling back to the default configuration if it doesn't find any matches.

Let's take a look at a specific example, and I'll walk you through how to configure your OS X system. For the purposes of this example, I'll assume that your internal home domain name is homedomain.net. As you work through these configuration steps, substitute the correct domain name as needed.

1. Open a terminal (I use [iTerm 2](http://www.iterm2.com/#/section/home)).

2. If your everyday user account isn't an administrative user (mine isn't), then use the `su` command to switch to an administrative user. For example, if your administrative user was named george, then you'd use `su - george` and supply the correct password for that account.

3. Create the right directory structure with the command `sudo mkdir /etc/resolver`. You'll be prompted for the password of the administrative user again. This directory structure is hard-coded into the DNS resolver client, so be sure to name the directory `resolver` and ensure that it is in the `/etc` directory.

4. Create the resolver configuration for your domain name using the command `sudo touch /etc/resolver/homedomain.net`. The name of the file has to match your internal domain name, so modify it accordingly. If you ran the previous command within a couple of minutes of running this one, you won't be prompted for the password again.

5. Edit the resolver configuration with `sudo vi /etc/resolver/homedomain.net`. (No `vi` versus `nano` arguments here, please. Use the tool you prefer.) The contents of this document leverage standard `resolv.conf` statements, like `domain homedomain.net` and `nameserver 192.168.1.10` (substituting the appropriate domain names and DNS server addresses, naturally).

6. Save the domain-specific file and exit the editor, log out of the administrative user account, and (optionally) close your terminal window. (Personally, I almost always have a terminal window open anyway.) That's it---you're done!

You can test this by logging into your corporate VPN. You'll note that once you've logged into your corporate VPN, the values inside `/etc/resolv.conf`--which is the "default" DNS resolver configuration used when no domain-specific configuration file is found---will have been modified to use corporate values. However, the domain-specific file in `/etc/resolver` (whose filename matches your internal domain name---you did make sure of that, right?) will remain unchanged, and **that's** the configuration OS X will use when attempting to resolve fully-qualified domain names in your home domain. Go ahead, try it. Pretty cool, eh?

I use this configuration myself, and I haven't run into any problems with it. The one drawback I see is that the domain-specific configuration file is hard-coded, not managed by DHCP, so that could present some additional configuration overhead should the IP addresses of your internal DNS servers change. However, I think that is a reasonable trade-off for being able to easily access home network resources while connected to my corporate VPN.

Questions? Comments? Clarifications? Feel free to post them in the comments below, where courteous comments are not only welcome, but encouraged.

**UPDATE:** Seven years later, someone has uncovered a Windows equivalent for this workaround. See [this post][0] for details.

[0]: http://www.patrickkremer.com/per-zone-dns-resolution-for-homelabs/
[1]: {{< relref "2006-01-04-mac-os-x-and-local-domains.md" >}}
