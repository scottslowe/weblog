---
author: slowe
categories: Tutorial
comments: true
date: 2012-11-27T14:40:01Z
slug: connecting-ovs-bridges-with-patch-ports
tags:
- Linux
- Networking
- OSS
- OVS
title: Connecting OVS Bridges with Patch Ports
url: /2012/11/27/connecting-ovs-bridges-with-patch-ports/
wordpress_id: 2967
---

Spurred to action by a brief mention at the bottom of [a blog post](http://blog.aaronorosen.com/open-vswitch-and-libvirt/), I began digging into the use of _patch ports_ to connect multiple [Open vSwitch (OVS)](http://openvswitch.org/) bridges. In this post, I'll show you how to connect two separate OVS bridges using a patch port, and then we'll explore some possible use cases for this functionality.

You've seen me use the `ovs-vsctl set interface <interface name> type=...` syntax several times; I provided some insight on what it's doing in [this post][1]. Basically, what we're doing with this command is manipulating the OVS configuration database. This is how we configure internal interfaces that can be plumbed with an IP address (for use in [Layer 3 routing with OVS][2]. In this article, we're going to use this command again, but this time we're going to set `type=patch`.

I'd not seen any references to patch ports before before running into [this blog post](http://blog.aaronorosen.com/open-vswitch-and-libvirt/), where patch ports receive a brief mention at the end of the article. So, I did a little digging, and finally found the manpage for the OVS configuration database (visible by running `man 5 ovs-vswitchd.conf.db` from a system with OVS installed). In that manpage, it describes patch ports as "a pair of virtual devices that act as a patch cable." A patch port only has a single option, and that single option is the name of the patch port at the opposite end.

Graphically, you can think of patch ports like this:

![OVS patch ports](/public/img/ovs-bridges-patch-ports.png)

To create a patch port, it's quite easy. This group of commands will create a patch port:

``` bash
ovs-vsctl add-port <bridge name> <port name>
ovs-vsctl set interface <port name> type=patch
ovs-vsctl set interface <port name> options:peer=<peer name>
```

You would repeat these commands for each bridge you want connected to another bridge. To connect two bridges, you would repeat them twice---once for the first bridge (specifying the patch port on the 2nd bridge as the peer), and again for the second bridge (specifying the patch port on the 1st bridge as the peer). Or, as the manpage puts it:

>That is, the two patch interfaces must have reversed name and peer values.

Once you've run through the commands, running `ovs-vsctl show` will reveal a configuration that would look something like this (obviously your bridge names, port names, and interface names will vary):

``` text
    Bridge "ovsbr2"
        Port "ovsbr2"
            Interface "ovsbr2"
                type: internal
        Port "patch2-0"
            Interface "patch2-0"
                type: patch
                options: {peer="patch0-2"}
    Bridge "ovsbr0"
        Port "bond0"
            Interface "eth0"
            Interface "eth2"
        Port "patch0-2"
            Interface "patch0-2"
                type: patch
                options: {peer="patch2-0"}
        Port "ovsbr0"
            Interface "ovsbr0"
                type: internal
```

Let's review this a bit:

* The bridge `ovsbr2` contains a port and interface named `patch2-0`, with a peer set to `patch0-2`. Note that `ovsbr2` has **no physical uplinks.**

* Likewise, the bridge `ovsbr0` has a port and interface named `patch0-2`, whose type is patch and whose peer is set to `patch2-0`. Note that, just as the manpage stated, the two patch ports have reversed name and peer values---this creates the connection between the two bridges. This bridge has a bond with two interfaces that take it to the outside world.

With this configuration, I can start up a guest domain attached to `ovsbr2`---which has no physical uplinks---and gain connectivity to the outside world via the patch ports that connect it to `ovsbr0` and its uplinks.

The next natural question (at least, it was the next natural question for me): how many levels of connected OVS bridges can you build? It turns out the answer is 5 (as described [here](http://openvswitch.org/pipermail/ovs-discuss/2012-July/007689.html)).

OK, this is interesting (sort of), but what sort of uses does this have? Well, I'm still exploring that myself, and I'd love to hear from readers as to how they might see this functionality utilized. Here are two examples of which I know:

* In [the original post](http://blog.aaronorosen.com/open-vswitch-and-libvirt/) that sparked this work, the author used a separate OVS bridge (or datapath) for OpenFlow testing. So, one bridge that had no uplinks was connected to an OpenFlow controller and had a patch port to an externally-connected bridge. Thus, he could perform OpenFlow testing with some guests, while leaving other guests (on the other bridge) unaffected.

* One other use case I saw was around XenServer integration. (I apologize but I can't find the link where I saw it now---if any readers have it, please share it in the comments.) Basically, patch ports were used to connect an integration bridge that is manipulated/managed by XenServer to an external bridge that managed all the connectivity to the outside world. This is similar to the first example, but in this case it's not OpenFlow that's involved but XenServer instead.

I've racked my brain for other potential use cases, but they seem to be escaping me at the moment. If you have other use cases, feel free to share them in the comments. Likewise, if there are technical errors or corrections, please share those in the comments as well. Courteous comments are always welcome and encouraged.

[1]: {{< relref "2012-10-04-some-insight-into-open-vswitch-configuration.md" >}}
[2]: {{< relref "2012-10-31-layer-3-routing-with-open-vswitch.md" >}})
