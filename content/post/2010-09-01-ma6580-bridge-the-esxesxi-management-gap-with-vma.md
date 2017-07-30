---
author: slowe
categories: Liveblog
comments: true
date: 2010-09-01T11:37:47Z
slug: ma6580-bridge-the-esxesxi-management-gap-with-vma
tags:
- ESXi
- Virtualization
- VMware
- vSphere
title: 'MA6580: Bridge the ESX/ESXi Management Gap with vMA'
url: /2010/09/01/ma6580-bridge-the-esxesxi-management-gap-with-vma/
wordpress_id: 2083
---

This is MA6580, titled "Bridge the ESX/ESXi Management Gap with the vSphere Management Assistant (vMA), Tips and Tricks Included". The presenters are Chris Monfet and Tim Murnane. This is my first session of Day 3 of VMworld 2010 in San Francisco. Following this is a whirlwind of vendor meetings, video interviews, a book signing, and more sessions this afternoon.

The focus on the vMA is due to the shift in focus from VMware from VMware ESX (vSphere 4.1 will be the last version with VMware ESX) to VMware ESXi. vMA is based on CentOS (will they switch to SuSE like they are for all other virtual appliances?) and supports VMware ESX/ESXi 3.5 Update 2 or later. The vMA uses 512MB of RAM and has a 5GB VMDK. It does use hardware version 4 in order to provide support for VI3 environments. You can deploy the vMA directly from the vSphere Client or by downloading the OVF and then deploying it.

The `/opt/vmware/vima/bin/vmware-vma-netconf.pl` script allows you to reconfigure vMA network settings if necessary.

The `vma-update` command (with the parameters info, update, or scan) allows you to patch or update the vMA. If you have a proxy server, you'll want to update `/etc/vmware/esxupdate/vmaupdate.conf` file accordingly.

By default, vMA does not run the NTP daemon, although it is preconfigured to use the pool.ntp.org servers. You can use `chkconfig` to enable the NTP daemon. You'll also want to update the time zone configuration.

The preferred target for vMA is vCenter Server, and you can also use it as a remote log host for VMware ESX/ESXi. You can also run vMA outside of the actual vSphere environment; for example, you can run it under VMware Workstation.

With regard to authentication, vMA uses interactive logon (prompted for username and password for every command), FastPass (stores credentials locally in a file), or Active Directory (using Likewise Open integration).

When using FastPass, you'll use the `vifp addserver`, `vifp removeserver`, `vifp listservers` commands. There's also a `vifp rotatepassword` option to automatically rotate passwords between the vMA and the VMware ESX/ESXi hosts.

With Active Directory integration, you only need to use the `domainjoin-cli` command to join the Active Directory domain. From there, authentication will happen automatically.

As I mentioned earlier, you can also use the vMA as a remote loghost. The `vi-logger` command is what you use to set this up. This is particularly important for VMware ESXi. Note that vxpa logs are not sent to syslog (see VMware KB 1017658). All log files go to `/var/log/vmware/<hostname>`.

The presenters now move into some use case/operational discussions. There are lots of examples provided; a bit more detail is provided for using the vMA to configure storage with the `esxcli` command. Examples are also provided for setting the MTU size on a vSwitch (using `vicfg-vswitch`), setting up log collection with `vi-logger`, and customizing management services. New to vMA 4.1 is the `vicfg-hostops` command, which you can use to put hosts into (and out of?) maintenance mode.

Now the session moves into a few best practices for vMA:

* One vMA per 100 VMware ESX/ESXi hosts when using `vi-logger`.

* Place vMA on your management LAN/VLAN.

* Use a static IP address, a fully qualified domain name, and correct DNS settings. This is especially important for AD integration.

* Configure the vMA as a remote log host.

* Enable NTP and configure it for UTC (VMware ESXi uses UTC).

* The recommended target for vMA/vCLI is vCenter Server (much in the same way vCenter Server is the recommended target for the vSphere Client).

* You might need to leave a VMware ESX host for tools like `mbralign`; this functionality still hasn't been migrated over to VMware ESXi or the vMA.

* Cleanup local accounts on your VMware ESX/ESXi when using a new VMA or destroying one.

* Try to limit the use of `resxtop`, and use it for real-time troubleshooting not monitoring.

The session wraps up with a few pre-recorded demos of bulk adding servers, bulk adding users, and running `resxtop`.
