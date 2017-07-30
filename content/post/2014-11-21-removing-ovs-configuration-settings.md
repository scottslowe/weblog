---
author: slowe
categories: Explanation
comments: true
date: 2014-11-21T11:57:21Z
slug: removing-ovs-configuration-settings
tags:
- CLI
- Linux
- Networking
- OVS
title: Removing OVS Configuration Settings
url: /2014/11/21/removing-ovs-configuration-settings/
wordpress_id: 3564
---

I've written quite a bit about [Open vSwitch (OVS)](http://openvswitch.org), but I realized recently that despite all the articles I've written I still haven't talked about how to _remove_ a configuration setting to OVS. I'm fixing that now with this article.

As part of my ongoing mission to give back to the open source community, I recently started making contributions and improvements to the OVS web site; specifically, I've been reformatting the configuration cookbooks to make them more readable (and to clean up the HTML source). Along the way, I've been adding small bits of content here and there. Most recently, I just updated the [QoS rate-limiting entry](http://openvswitch.org/support/config-cookbooks/qos-rate-limiting/), and I wanted to add information on how to remove the QoS settings.

Normally, you can remove an OVS configuration setting using the `ovs-vsctl remove` command. For example, if you set a VLAN tag on an port with this command:

	ovs-vsctl set port vnet0 tag=100

Then you could remove that VLAN tag with this command:

	ovs-vsctl remove port vnet0 tag 100

Note the slight syntactical difference in the two commands; the `remove` command expects four parameters.

It turns out, however, that this command won't work for all configuration parameters. In some cases, you can use the `ovs-vsctl remove` command, but in some cases you have to use the `ovs-vsctl set` command to set it back to the original value. (This is how you "remove" QoS rate-limiting, by the way: you set the QoS values back to 0.)

How does one know when to use one command versus the other? The answer lies within the Open vSwitch database schema. ([This document](http://openvswitch.org/ovs-vswitchd.conf.db.5.pdf) is your friend.)

If you look through the OVS database schema, you'll find that some values (the document references them as "columns") will be listed as "integers," while others are listed as "optional integers." For example, the tag column in the Port table (which is where VLAN tags are stored for an OVS port) is listed as an optional integer. The ingress_policing_rate column in the Interface table (where QoS rate-limiting is specified) is listed as an integer.

Why is that important? Here's why:

* If column is classified as an optional integer, then you can use `ovs-vsctl remove` to remove the value.

* If, on the other hand, a column is specified as an integer, then you have to set the value back to its original value (using `ovs-vsctl set`) to remove the value. You _can't_ use `ovs-vsctl remove` as it will generate an error.

I hope this makes sense. Feel free to hit me up if you have any questions, or if anything I've shared here is incorrect or inaccurate.
