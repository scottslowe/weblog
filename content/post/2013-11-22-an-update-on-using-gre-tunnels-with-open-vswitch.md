---
author: slowe
categories: Explanation
comments: true
date: 2013-11-22T09:00:00Z
slug: an-update-on-using-gre-tunnels-with-open-vswitch
tags:
- GRE
- Linux
- Networking
- OVS
- Virtualization
title: An Update on Using GRE Tunnels with Open vSwitch
url: /2013/11/22/an-update-on-using-gre-tunnels-with-open-vswitch/
wordpress_id: 3333
---

In this post, I'm going to provide an update on using GRE tunnels with Open vSwitch (OVS) to include more than 2 hosts. I previously showed you [how to use GRE tunnels with OVS][1] to connect VMs on different hypervisor hosts, but in my testing I didn't use this technique with more than two hypervisors. A few readers posted comments to that article asking how to extend the solution to more than 2 hypervisors, but I hadn't had the time to test anything more.

Now, as a result of some related work I've been doing, I have an update on using this technique for more than two hosts. If you didn't read the post on [using GRE tunnels with OVS][1], go back and read that now. Also, be sure to read my post on [examining OVS traffic patterns][2], as this is also useful information. Finally, note that this information applies to any use of GRE tunnels with OVS, not just GRE tunnels with OVS on hypervisors.

Let's say you have three hosts:

1. HostA, with an IP address of 10.1.1.1

2. HostB, with an IP address of 10.1.1.2

3. HostC, with an IP address of 10.1.1.3

To connect entities (VMs, containers, etc.) on these hosts using GRE tunnels, you'd need to manually configure OVS on each of hosts:

* On HostA, you'd need a GRE tunnel to HostB (10.1.1.2) and a GRE tunnel to HostC (10.1.1.3)

* On HostB, you'd need a GRE tunnel to HostA (10.1.1.1) and one to HostC (10.1.1.3)

* On HostC, you'd need two GRE tunnels, one to HostA (10.1.1.1) and one to HostB (10.1.1.2)

I won't repeat the specific commands to create those tunnels here, as it is well explained in my earlier article. What this creates is a virtual topology like this:

![GRE tunnel full mesh](/public/img/gre-full-mesh.png)

What you'll find when you try this yourself is that everything works fine when there are just two hosts; this is what I also found when I first wrote the article. When you add the third host, though, you'll find---assuming you created a full mesh of GRE tunnels---is that everything stops working.

Here's how to fix that. Run this command on each of the hosts running OVS:

    ovs-vsctl set bridge <bridge name> stp_enable=true

Yes, that's right: looping is the culprit here. Look back at the topology figure above. In the physical world, a topology like that using switches (without STP) would take down your network because of a bridging loop. The same applies here as well. In both cases (physical or virtual) you have two choices: you can either not create a full mesh topology (you could use a star topology or something if you wanted) or you run STP. It's up to you.

Assuming you turn on STP, then you'll find after a few minutes that you'll be able to happily ping between VMs on these hypervisors.

I do want to share one final note before I wrap up. STP is needed in this instance because we are relying on OVS in MAC learning mode (just like a physical switch). If we were to add an OpenFlow controller to this mix and push flow rules down to OVS, OVS would stop using MAC learning, and we would no longer need STP in order to build full-mesh topologies of tunnels.

Feel free to post any questions or comments below. All courteous comments are welcome!

[1]: {{< relref "2013-05-07-using-gre-tunnels-with-open-vswitch.md" >}}
[2]: {{< relref "2013-05-15-examining-open-vswitch-traffic-patterns.md" >}}
