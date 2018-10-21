---
author: slowe
categories: Tutorial
comments: true
date: 2010-03-02T18:20:53Z
slug: pxe-booting-vmware-esx-40
tags:
- Cisco
- ESX
- UCS
- Virtualization
- VMware
- vSphere
title: PXE Booting VMware ESX 4.0
url: /2010/03/02/pxe-booting-vmware-esx-40/
wordpress_id: 1854
---

I recently had the opportunity to work on a proof of concept (PoC) in which we wanted to help a customer streamline the processes needed to deploy new hosts and reduce the amount of time it took overall. One of the tools we used in the PoC for this purpose was PXE booting VMware ESX for an automated installation. Here are the details on how we made this work.

Before I get into the details, I'll provide this disclaimer: there are probably easier ways of making this work. I specifically didn't use UDA or similar because I wanted to gain the experience of how to do this the "old fashioned" way. I also wanted to be able to walk the customer through the "old fashioned" way and explain all the various components.

With that in mind, here are the components you'll need to make this work:

1. You'll need a DHCP server to pass down the PXE boot information. In this particular instance, I used an existing Windows-based DHCP server. Any DHCP server should work; feel free to use the Linux ISC DHCP server if you prefer.

2. You'll need an FTP server to host the kickstart script and VMware ESX 4.0 Update 1 installation files. In this case, I used a third-party FTP server running on the same Windows-based server as DHCP. Again, feel free to use a Linux-based FTP server if you prefer.

3. You will need a TFTP server to provide the boot files. The third-party FTP server used in the previous step also provided TFTP functionality. Use whatever TFTP server you prefer.

Make sure that each of these components is working as expected before proceeding. Otherwise, you'll spend time troubleshooting problems that aren't immediately apparent.

## Preparing for the Automated ESX Installation

First, copy the contents for the VMware ESX 4.0 Update 1 DVD---not the actual ISO, but the contents of the ISO---to a directory on the FTP server. Test it to make sure that the files can be accessed via an anonymous FTP user.

Also go ahead and create a simple kickstart script that automates the installation of VMware ESX. I won't bother to go into detail on this step here; it's been quite adequately documented elsewhere. You'll need to put this kickstart script on the FTP server as well.

At this point, you're ready to proceed with gathering the PXE boot files.

## Gathering the PXE Boot Files

The first task you'll need to complete is gathering the necessary files for a PXE boot environment.

First, copy the `vmlinuz` and `initrd.img` files from the VMware ESX 4.0 Update 1 ISO image. Since I use a Mac, for me this was a simple case of mounting the ISO image and copying out the files I needed. Linux or Windows users, it might be a bit more complicated for you. These files, by the way, are in the ISOLINUX folder on the DVD image.

Next, you'll need the PXE boot files. Specifically, you'll need the `menu.c32` and `pxelinux.0` files. These files are **not** on the DVD ISO image; you'll have to download Syslinux from [this web site](http://syslinux.zytor.com/wiki/index.php/The_Syslinux_Project). Once you download Syslinux, extract the files into a temporary directory. You'll find `menu.c32` in the com32/menu folder; you'll find `pxelinux.0` in the core folder. Copy both of these files, along with `vmlinuz` and `initrd.img`, into the root directory of the TFTP server. (If you don't know the root directory of the TFTP server, double-check its configuration.)

You're now ready to configure the PXE boot process.

## Configuring the PXE Boot Environment

Once the necessary files have been placed into the root directory of the TFTP server, you're ready to configure the PXE boot environment. To do this, you'll need to create a PXE configuration file on the TFTP server.

The file should be placed into a folder named pxelinux.cfg under the root of the TFTP server. The filename of the PXE configuration file should be named something like this:

	01-<MAC address of network interface on host>

If the MAC address of the host was 01:02:03:04:05:06, the name of the text file in the `pxelinux.cfg` folder on the TFTP server would be:

	01-01-02-03-04-05-06

The PoC in which I was engaged involved Cisco UCS, so we knew in advance what the MAC addresses were going to be (the MAC address is assigned in the UCS service profile).

The contents of this file should look something like this (lines have been wrapped here for readability and are marked by backslashes; don't insert any line breaks in the actual file):

```text
default menu.c32  
menu title Custom PXE Boot Menu Title  
timeout 30  

label scripted  
menu label Scripted installation  
kernel vmlinuz  
append initrd=initrd.img mem=512M ksdevice=vmnic0 ks=ftp://A.B.C.D/ks.cfg  
IPAPPEND 1
```

You'll want to replace `ftp://A.B.C.D/ks.cfg` with the correct IP address and path for the kickstart script on the FTP server.

Only one step remains: configuring the DHCP server.

## Configuring the DHCP Server for PXE Boot

As I mentioned earlier, I used the Windows DHCP server as a matter of ease and convenience; feel free to use whatever DHCP server best suits your needs. There are only two options that are necessary for PXE boot:

066 Boot Server Host Name _(specify the IP address of the TFTP server)_  

067 Bootfile Name _(specify `pxelinux.0`)_

In this particular example, I created reservations for each MAC address. Because the values were the same for all reservations, I used server-wide DHCP options, but you could use reservation-specific DHCP options if you wanted different boot options on a per-MAC address (i.e., per-reservation) basis.

## The End Result

Recall that this PoC was using Cisco UCS blades. Thus, in this environment, to prepare for a new host coming online we only had to make sure that we had a PXE configuration file and create a matching DHCP reservation. The MAC address would get assigned via the service profile, and when the blade booted then it would automatically proceed with an unattended installation. Combined with Host Profiles in VMware vCenter, this took the process of bringing new ESX/ESXi hosts online down to mere minutes. A definite win for any customer!
