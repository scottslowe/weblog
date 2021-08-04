---
author: slowe
categories: Tutorial
comments: true
date: 2021-06-28T08:05:00-06:00
tags:
- CLI
- Encryption
- macOS
- Networking
- VPN
title: Using WireGuard on macOS via the CLI
url: /2021/06/28/using-wireguard-on-mac-via-cli/
---

I've written a few different posts on [WireGuard][link-1], the "simple yet fast and modern VPN" (as described by the WireGuard web site) that aims to supplant tools like IPSec and OpenVPN. My [first post on WireGuard][xref-1] showed how to configure WireGuard on Linux, both on the client side as well as on the server side. After that, I followed it up with posts on [using the GUI WireGuard app to configure WireGuard on macOS][xref-2] and---most recently---[making WireGuard from Homebrew work on an M1-based Mac][xref-3]. In this post, I'm going to take a look at using WireGuard on macOS again, but this time via the CLI.<!--more-->

Some of this information is also found in [this WireGuard quick start][link-2]. Here I'll focus only on using macOS as a WireGuard client, not as a server; refer to the WireGuard docs (or to [my earlier post][xref-1]) for information on setting up a WireGuard server. I'll also assume that you've installed WireGuard via [Homebrew][link-4].

## Generating Keys

The first step is to generate the public/private keys you'll need. If the `/usr/local/etc/wireguard` (or the `/opt/homebrew/etc/wireguard` for users on an M1-based Mac) directory doesn't exist, you'll need to first create that directory. (It didn't exist on my system.) Then, from that directory, run these commands:

```bash
umask 077
wg genkey | tee privatekey | wg pubkey > publickey
```

This generates files named `privatekey` and `publickey`, the contents of which you'll use in the configuration of WireGuard. Some tutorials indicate the need to become root or use `sudo`, but my configuration seems to work fine without either of those.

## Setting up a WireGuard Interface

Once you have public/private keys, you're ready to set up the interface configuration. Note that you'll also need the public key of the server (peer) system to which you're connecting.

Create a file named `wg0.conf` in `/usr/local/etc/wireguard` (that would be in `/opt/homebrew/etc/wireguard` for M1-based Macs). The contents of the file should look like this:

```toml
[Interface]
PrivateKey = <private-key-generated-earlier>
Address = <vpn-ip-address-for-this-interface>

[Peer]
PublicKey = <public-key-for-peer-system>
Endpoint = <public-ip-address>:51280
AllowedIPs = <vpn-ip-address-for-peer-interface>, <additional-cidrs>
PersistentKeepalive = 25
```

Most of this configuration file is pretty straightforward:

* You'll need to specify the private key for the local system plus the public key of the peer system.
* Each system will need "VPN IP addresses" that are routable/reachable from each other (I use a /29 CIDR from which I assign peer VPN IP addresses). The VPN IP addresses do not have to be publicly routable (they can be RFC 1918 ranges), but they should not overlap other IP address ranges---otherwise you'll run into routing table issues.
* For the `AllowedIPs` setting, you need to specify the VPN IP address of the peer _plus_ any additional networks that are reachable via the peer. In my case, I use WireGuard to access private EC2 instances in an AWS VPC, so I specify the VPC CIDR here.

The peer system will have/need a similar configuration. As described [here][xref-1], my primary use case is enabling connectivity to EC2 instances with private IP addresses inside a VPC, so the peer system for me is a Linux instance with WireGuard installed and configured.

## Activating the VPN

Once you have the WireGuard interface configuration finished, you can activate the VPN connection using `wg-quick up wg0` (if you called your configuration file from the previous section something _other_ than `wg0.conf`, then change the command accordingly). If the peer system is already configured and its interface is up, then the VPN connection should establish automatically and you _should_ be able to start routing traffic through the peer (assuming you specified some additional networks to be routed through the peer).

Use `wg-quick down wg0` to take the VPN connection down.

I did see some references to being able to use `launchd` on macOS to automatically start the WireGuard interface. It took a bit of testing and exploration, but I eventually settled on [this configuration][xref-4].

## Troubleshooting the Connection

If you run into problems getting the connection up and working, here are a few things you can check:

* When specifying `AllowedIPs`, make sure to include the VPN interface IP address as well as any additional networks. Failing to include the VPN interface IP address can cause the connection to fail.
* Double-check that you've specified public and private keys correctly. It's easy to get these mixed up. Each system's configuration should reference its own private key and the peer's public key.
* Make sure you've specified the peer's endpoint correctly.
* Make sure that the traffic is being allowed to reach each system. Double-check firewalls, network access control lists, host-based firewalls, and security groups.
* Make sure the WireGuard interface is up and active on both systems. When troubleshooting connections, it can be easy to forget whether the interface is active or not.

Aside from the location of the configuration files, configuring WireGuard via the CLI using configuration files is very, very similar across systems. The interface configuration shown above is---as far as I've been able to tell so far---identical across both macOS and Linux.

I hope this article is helpful. If you have any questions, comments, or corrections, please feel free to contact me. You can reach [me on Twitter][link-3] (DMs are open), or hit me up on one of a variety of Slack communities. I'd be happy to chat with you.

[link-1]: https://www.wireguard.com
[link-2]: https://www.wireguard.com/quickstart/
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://brew.sh
[xref-1]: {{< relref "2021-02-22-setting-up-wireguard-for-aws-vpc-access.md" >}}
[xref-2]: {{< relref "2021-04-01-using-wireguard-on-macos.md" >}}
[xref-3]: {{< relref "2021-06-22-making-wireguard-from-homebrew-work-on-an-m1-mac.md" >}}
[xref-4]: {{< relref "2021-08-04-starting-wireguard-interfaces-automatically-launchd-macos.md" >}}
