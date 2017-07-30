---
author: slowe
categories: Tutorial
comments: true
date: 2006-12-27T14:06:33Z
slug: ssh-from-esx-server-to-data-ontap
tags:
- ESX
- Interoperability
- NetApp
- ONTAP
- SSH
- Virtualization
- VMware
title: SSH from ESX Server to Data ONTAP
url: /2006/12/27/ssh-from-esx-server-to-data-ontap/
wordpress_id: 392
---

By default, the SSH configuration on [VMware ESX Server](http://www.vmware.com/products/vi/esx/) only supports AES encryption types (specifically, AES-256 and AES-128). If you need SSH connectivity from ESX Server to a [Network Appliance](http://www.netapp.com/) storage system running Data ONTAP, you'll need to modify this to support 3DES.

This kind of connectivity would be necessary if you were interested in running scripts on ESX Server that connected to the NetApp storage system via SSH to run commands (for example, to initiate a snapshot via the command line). This arrangement is described in [this document from NetApp](http://www.netapp.com/tech_library/ftp/3393.pdf).

To modify the ciphers supported by ESX Server, edit the `/etc/ssh/ssh_config` file and change this line:

	Ciphers aes256-cbc,aes128-cbc

Instead, it should look like this:

	Ciphers aes256-cbc,aes128-cbc,3des-cbc

This will enable SSH connections from ESX Server to find a compatible cipher with the SSH daemon running in Data ONTAP. Note that we change the SSH configuration on ESX Server because, as far as I know, the ciphers supported by the SSH daemon in Data ONTAP are not configurable by the user.

Note that you'll also need to enable SSH traffic through the ESX firewall:

	esxcfg-firewall -e sshClient

And, of course, you'll need to configure and enable SSH access on the Network Appliance storage system itself using the `secureadmin` command in Data ONTAP:

	secureadmin setup ssh  
	secureadmin enable ssh2

Once SSH is reconfigured on ESX Server and configured/enabled in Data ONTAP, then using SSH to run commands remotely from ESX Server to the NetApp storage system should work without any problems. For complete automation, you'll also want to setup SSH shared keys as well, but I'll save those details for a future article.
