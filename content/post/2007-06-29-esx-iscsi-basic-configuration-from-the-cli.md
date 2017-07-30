---
author: slowe
categories: Tutorial
comments: true
date: 2007-06-29T09:50:43Z
slug: esx-iscsi-basic-configuration-from-the-cli
tags:
- ESX
- Interoperability
- iSCSI
- Storage
- Virtualization
- VMware
title: ESX iSCSI Basic Configuration from the CLI
url: /2007/06/29/esx-iscsi-basic-configuration-from-the-cli/
wordpress_id: 481
---

Sometimes, I find it better/faster/easier to perform tasks from the command-line interface (CLI) than going through a GUI. So, the other day, I needed to setup a new [VMware ESX Server](http://www.vmware.com/products/vi/esx/) for iSCSI storage, and thought I'd document the commands I used to set that up.

Unfortunately, the commands weren't able to do all the configuration I needed---I couldn't find the commands to let me set iSCSI security (CHAP username and password), and I needed iSCSI security for the target to which I was connecting. However, these commands _should_ work just fine for a basic iSCSI configuration.

Here are the commands I used. First, to enable the software iSCSI initiator:

	esxcfg-swiscsi -e  

Next, to configure the ESX Service Console firewall (iptables) to allow the traffic from the software iSCSI initiator:

	esxcfg-firewall -e swISCSIClient  

Next, set the target IP address for the vmhba40 adapter (the adapter that represents the software iSCSI initiator):

	vmkiscsi-tool -D -a 192.168.100.50 vmhba40  

Finally, rescan for storage devices on vmhba40:

	esxcfg-rescan vmhba40  

I'm sure there are more commands available; for more information, you can refer to Mike Laverick's excellent [Guide to ESX 3 Service Console](http://www.rtfm-ed.eu/docs/vmwdocs/esx3.x-vc2.x-serviceconsole-guide.pdf). In addition, please note that the ESX Server software iSCSI initiator is simply the open source version of an old Cisco iSCSI initiator for Linux, so you can use [that command reference](http://www.cuddletech.com/articles/iscsi/ar01s07.html) as well (I believe [this information](http://linux-iscsi.sourceforge.net/) is also applicable). Just preface the commands from the CuddleTech article with "vmk" and it should work just as listed.

One oddity: you may find that some of the command-line tools will report a Cisco IQN, but after further configuration (and especially after configuration from VirtualCenter) it will switch to a VMware IQN. This may wreak havoc with iSCSI targets on which LUN presentation is based on IQN names, so plan accordingly.
