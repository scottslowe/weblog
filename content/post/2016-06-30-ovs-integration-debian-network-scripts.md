---
author: slowe
categories: Explanation
comments: true
date: 2016-06-30T00:00:00Z
tags:
- OVS
- Debian
- Linux
title: OVS Integration with Debian Network Scripts
url: /2016/06/30/ovs-integration-debian-network-scripts/
---

I had a reader contact me recently with some questions regarding the use of [Open vSwitch (OVS)][link-1] on [Debian "Jessie" 8.5][link-2] and using the OVS integration with the Debian network scripts. For those of you that might be unfamiliar with this functionality, it's the ability to configure OVS via instructions and directives found in the `/etc/network/interfaces` file. As I was helping this reader, I came across a couple potential "gotchas" that I wanted to point out here.

First, I'll point you to the documentation for the Debian network scripts integration, which is found in [this file][link-3] in the "Debian network scripts integration" section. This documentation provides the complete breakdown of the various commands that can be used in `/etc/network/interfaces` to configure OVS.

Based on that documentation, you could create an OVS bridge and add a physical port to that bridge by including the following stanzas in `/etc/network/interfaces`:

``` text
allow-ovs ovsbr0
iface ovsbr0 inet manual
  ovs_type OVSBridge
  ovs_ports eth1

allow-ovsbr0 eth1
iface eth1 inet manual
  ovs_bridge ovsbr0
  ovs_type OVSPort
```

Now for the gotchas...

The Debian "Jessie" repos include version 2.3.0 of OVS; the latest release in the 2.3.x train is 2.3.3. As it turns out, 2.3.3 includes a couple of fixes that did not make it into 2.3.0, and these fixes address a couple potential issues you might run into when using OVS with the Debian network scripts integration. Specifically:

1. You should _not_ include the `auto` keyword in any of your OVS-related stanzas. If you do that, the OVS integration scripts will not function as expected. 2.3.3 includes a fix that forces the integration scripts to work as expected even if the user includes the `auto` keyword.
2. The 2.3.3 release adds `_SYSTEMCTL_SKIP_REDIRECT=yes` to the `/etc/init.d/openvswitch-switch` file, which prevents some systemd-related issues that might crop up (see [here][link-4] for the change). This line is not found in the 2.3.0 version of OVS when installed on Debian 8.x. (That being said, I tested both with and without this line, and it worked as expected in both cases.)

The first issue appears to be the more "serious" of the two; in my testing with version 2.3.0 of OVS (straight from the Debian repos), the integration simply didn't work when the `auto` keyword was included. As soon as I removed the `auto` keyword, it started working as expected. The addition of the `_SYSTEMCTL_SKIP_REDIRECT` line did not seem to be as significant; once the `auto` keyword issue was sorted, the presence (or absence) of that line didn't seem to have any effect on my testing.

So, if you're having issues making OVS integration with the Debian network scripts work as expected, the first thing I'd check is to ensure that the `auto` keyword is _not_ included in any configuration stanzas related to OVS. From there, you _may_ find it helpful to edit `/etc/init.d/openvswitch-switch` to include `_SYSTEMCTL_SKIP_REDIRECT`, but that seems (at this point) less likely to be the culprit.

For other OVS-related content, have a look at any of [my OVS-tagged articles][link-5].

**UPDATE:** Another reader contacted me to say that the `_SYSTEMCTL_SKIP_REDIRECT` setting may help in situations where you need to restart OVS (perhaps due to an update or similar). Without this setting, VM interfaces may be disconnected from the bridge. I haven't tested or verified this, so keep that in mind.


[link-1]: http://openvswitch.org/
[link-2]: https://www.debian.org/
[link-3]: https://github.com/openvswitch/ovs/blob/master/debian/openvswitch-switch.README.Debian
[link-4]: https://github.com/openvswitch/ovs/blob/master/debian/openvswitch-switch.init#L30
[link-5]: /tags/ovs/
