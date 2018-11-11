---
author: slowe
categories: Tutorial
comments: true
date: 2006-10-06T13:10:28Z
slug: openbsd-as-a-simple-nat-router
tags:
- BSD
- ESX
- NAT
- Networking
- UNIX
title: OpenBSD as a Simple NAT Router
url: /2006/10/06/openbsd-as-a-simple-nat-router/
wordpress_id: 344
---

To setup a simple NAT router/firewall using [OpenBSD](http://www.openbsd.org/), use these steps as a general guideline. I'm assuming that you have general knowledge of OpenBSD. This article applies to OpenBSD 3.9.

First, configure the network interfaces appropriately. Typically, this will involve editing the `hostname.<NIC type>` file. In a [VMware ESX Server](http://www.vmware.com/products/vi/esx/) environment, OpenBSD uses pcn0 for the first virtual NIC, pcn1 for the second virtual NIC, etc., so the appropriate configuration files would be `hostname.pcn0`, `hostname.pcn1`, and so forth.

Next, enable IP forwarding by editing `/etc/sysctl.conf` and making the following change (the line is present in a default installation, you just need to uncomment it):

    net.inet.ip.forwarding=1

Next, we'll need to enable the OpenBSD packet filter, pf. This is typically done by creating/editing the file `/etc/rc.conf.local` and making sure the following line is present:

    pf=YES

Next, we'll configure pf for network address translation (NAT) and simple packet filtering. If you've never configured pf before, I highly recommend this [OpenBSD PF guide](http://www.openbsd.org/faq/pf/index.html); it will introduce you to the functionality of this very powerful packet filtering engine. (Sometimes I wish [Mac OS X](http://www.apple.com/macosx/) would switch to using pf.) You configure pf by placing a ruleset into `/etc/pf.conf`.

Here's a quick sample ruleset (keep in mind this is based on OpenBSD running as a virtual machine in a VMware environment):

```text
# Set some variables for use later
ext_if="pcn1"
int_if="pcn0"
icmp_types="echoreq"

# Skip all loopback traffic
set skip on lo

# Scrub all traffic
scrub in

# Perform NAT on external interface
nat on $ext_if from $int_if:network -> ($ext_if:0)

# Define default behavior
block in
pass out keep state

# Allow inbound traffic on internal interface
pass quick on $int_if

# Protect against spoofing
antispoof quick for { lo $int_if }

# Allow other traffic
pass in on $ext_if proto tcp to ($ext_if) port ssh flags S/SA keep state
pass in inet proto icmp from $allowed_hosts icmp-type $icmp_types keep state
```

This is a really, really simple configuration, but it will get the job done. (I did title this "OpenBSD as a _Simple_ NAT Router", after all.)

For more advanced configurations, I highly recommended reviewing the OpenBSD documentation (which, by the way, is very thorough and very extensive; kudos to the OpenBSD team for their documentation efforts.)
