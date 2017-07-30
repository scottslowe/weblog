---
author: slowe
categories: Tutorial
comments: true
date: 2011-01-18T11:43:33Z
slug: changing-the-ip-address-on-a-vplex-management-server
tags:
- EMC
- Storage
- VPLEX
title: Changing the IP Address on a VPLEX Management Server
url: /2011/01/18/changing-the-ip-address-on-a-vplex-management-server/
wordpress_id: 2218
---

EMC VPLEX leverages a Linux-based management server as an integral part of the overall architecture. The management server, as the name implies, provides the management interfaces---both HTTPS (web-based) and CLI---for managing the VPLEX cluster(s). In a VPLEX Local configuration, changing the IP address is a single-step process; in a VPLEX Metro configuration, there is an additional step required. In this post I'll walk you through both situations.

All in all, it's a pretty simple process, but the one thing that might trip you up is the different CLI environments involved. Some commands are run from the management server's native Linux-based CLI, while other commands are run from the Vplexcli. Remember that you'll access the management server's Linux-based CLI by simply opening an SSH session to the management server. You'll access the Vplexcli by running either the `vplexcli` or `telnet localhost 49500` commands once you've logged into the management server.

## VPLEX Local

When you are running in a VPLEX Local configuration, changing the IP address of the management server is a single-step process. From the Vplexcli, a single command is all you need:

	management-server set-ip -i <IP address:Netmask> -g <Gateway IP address> -p eth3

This command changes the IP address on the management server and you're done.

## VPLEX Metro

In addition to providing the management interfaces for the VPLEX cluster(s), in a VPLEX Metro configuration each management server is also responsible for creating and maintaining an IPSec-based VPN between itself and its peer management server in a VPLEX Metro configuration. The management servers use this VPN as a private connection between them in order to communicate with the directors in the remote cluster. Therefore, changing the IP address of a management server in a VPLEX Metro configuration could impact more than just the address used to access the VPLEX cluster(s).

So, in a VPLEX Metro configuration there's a second step you must also follow in order to update the configuration for the VPN between the two management servers.

First, you'll change the management server's IP address using the `management-server set-ip` command I described above.

After changing the IP address using the `management-server set-ip` command, you then need to edit the `/etc/ipsec.conf` file on the **other** management server to reflect the new IP address you just assigned. For example, if you changed the IP address on management server 1 to 172.16.100.100, then you would edit the `/etc/ipsec.conf` file on management server 2 to show that management server 1 is now reachable at a new address. This ensures that the VPN configuration on each management server properly reflects the correct IP address of its peer.

To do this, open an SSH session to the peer management server---whose VPN configuration needs to be updated---and use `vi` to edit the `/etc/ipsec.conf` file. The specific line in the file that needs to be changed is the "right=" line, where the IP address indicates the remote server ("right" means "remote" in this case). Save your changes and return to the management server's Linux-based CLI.

You'll then need to re-enter the Vplexcli (again, using the `vplexcli` or `telnet localhost 49500` commands). Once in the Vplexcli, restart the VPN using the `vpn restart` command. Restart the VPN on both management servers using this process.

To verify that the VPN is working properly, from the Vplexcli you can use the `vpn status` command like this:

	vpn status -l <Local cluster ID> -n <Number of engines in local cluster> -c <Remote cluster ID> -e <Number of engines in remote cluster> -r <IP address of remote management server>

The output of that command should show that each director in each engine, both local and remote, is reachable.

If you need to change the IP addresses of both management servers in a VPLEX Metro configuration, then change one at a time following the process I described above.
