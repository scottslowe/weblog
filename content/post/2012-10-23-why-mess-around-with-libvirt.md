---
author: slowe
categories: Explanation
comments: true
date: 2012-10-23T09:00:00Z
slug: why-mess-around-with-libvirt
tags:
- Interoperability
- Libvirt
- Linux
- OSS
- Virtualization
title: Why Mess Around with libvirt?
url: /2012/10/23/why-mess-around-with-libvirt/
wordpress_id: 2904
---

You might have noticed that I've been writing a fair number of articles around [libvirt](http://libvirt.org), the open source virtualization API project. Here are some of the libvirt-related articles that I've posted over the last couple of months (and there are more in the works):

[Working with KVM Guests][1]  

[Compiling libvirt 0.10.1 on CentOS 6.3][2]  

[Compiling libvirt 0.10.1 on Ubuntu 12.04][3]  

[Wrapping libvirt Virtual Networks Around Open vSwitch Fake Bridges][4]

While libvirt is pretty cool in and of itself (I really like the abstraction layer that libvirt provides between multiple hypervisors and management tools farther up the stack), one of the primary reasons that I'm spending some time working with and attempting to understand libvirt is because libvirt is a key component in the communication that occurs between the OpenStack Compute (Nova) software and the underlying hypervisors that are supported with OpenStack (especially KVM). In attempting to educate myself about OpenStack, I've decided to first start by making myself very familiar with a few key components, including libvirt, [Open vSwitch](http://openvswitch.org), and [KVM](http://www.linux-kvm.org/page/Main_Page). This provides a foundation upon which I can build additional knowledge.

At least, that's the general idea. As always, I'm open to constructive feedback---is it worth the time, or not? I'd love to hear from others who might be on a similar path of discovery and education. Feel free to share your thoughts in the comments below.

[1]: {{< relref "2012-08-21-working-with-kvm-guests.md" >}}
[2]: {{< relref "2012-09-06-compiling-libvirt-0-10-1-on-centos-6-3.md" >}}
[3]: {{< relref "2012-09-07-compiling-libvirt-0-10-1-on-ubuntu-12-04.md" >}}
[4]: {{< relref "2012-10-22-wrapping-libvirt-virtual-networks-around-open-vswitch-fake-bridges.md" >}}
