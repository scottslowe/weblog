---
author: slowe
categories: Information
comments: false
date: 2006-01-22T21:31:09Z
slug: cisco-nat-issue-resolved
tags:
- Cisco
- NAT
- Networking
title: Cisco NAT Issue Resolved
url: /2006/01/22/cisco-nat-issue-resolved/
wordpress_id: 163
---

A short while back I mentioned that I was having a bit of [a problem with network address translation][1] (NAT) on a Cisco router. I've managed to get the issue resolved, so here's the solution in case someone runs across this problem in the future.

In this instance, the original configuration of the router provided a means for both dynamic NAT and static NAT. Specifically, all the workstations on the LAN would be dynamically translated using port address translation (PAT) behind the external interface of the router, while the web server itself would be statically translated. Normally, this would not be a problem.

This feat was accomplished using an access list, like this:

    access-list 1 permit 172.16.1.0 0.0.0.255

This access list was then applied to the interface using a route map. The problem here, though, is that this dynamic NAT setup includes the IP address of the web server (let's just say for the purposes of this example that the web server is 172.16.1.10).

So, to fix the problem, we modified the access list to specifically _exclude_ the web server's IP address:

    access-list 1 deny 172.16.1.10
    access-list 1 permit 172.16.1.0 0.0.0.255

This took care of the apparent conflict between the dynamic NAT setup and the static NAT setup (which was accomplished using an ordinary `ip nat inside source` command), and the setup has worked without any problems since then.

Now, if I could just figure out why my GRE-over-IPSec tunnels aren't working, I'd be in really good shape...


[1]: {{< relref "2006-01-14-cisco-nat-issue.md" >}}