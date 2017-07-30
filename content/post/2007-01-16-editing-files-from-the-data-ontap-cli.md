---
author: slowe
categories: Tutorial
comments: true
date: 2007-01-16T22:41:37Z
slug: editing-files-from-the-data-ontap-cli
tags:
- NetApp
- ONTAP
- Storage
title: Editing Files from the Data ONTAP CLI
url: /2007/01/16/editing-files-from-the-data-ontap-cli/
wordpress_id: 402
---

While setting up a [Network Appliance](http://www.netapp.com/) storage system today for a customer, I ran into a situation that was a bit puzzling for a moment. I needed to change the IP address on the storage system's clustered controllers, but in order to do that I needed to edit some files in the /etc directory on the root volume. Normally, that wouldn't be a big deal; I'd just mount the root volume (vol0) via CIFS or NFS from my [MacBook Pro](http://www.apple.com/macbookpro/) and go from there.

However, in this particular instance, the customer hadn't licensed the CIFS and NFS protocols because this storage system would be used strictly as an iSCSI target for a [VMware ESX Server](http://www.vmware.com/products/vi/esx/) deployment. That meant there would be no mounting the root volume this time. So how does one go about editing files in the Data ONTAP CLI?

Excellent question. The answer is the `priv set advanced` command. Rightly so, NetApp warns you that running `priv set advanced` exposes functionality that can be dangerous; don't muck around in there or you're likely to find yourself with a non-functional NetApp storage system. However, with a bit of caution, the advanced commands are just what we need in this situation.

The advanced command set includes useful commands like `ls` (to list the files in the `/etc` directory, for example), `rdfile` (think of `cat` or `more` from a Linux/UNIX system), and `wrfile` (for redirecting standard input to a text file). While there's no text editor _per se_, these three tools can get us most of the way there.

So here's how I used the advanced command set to change the IP addresses of the storage system:

1. Enabled the advanced command set with `priv set advanced	.

2. Displayed the current contents of the `/etc/hosts` file with `rdfile /etc/hosts`.

3. Created `/etc/hosts.new` (containing the contents of `/etc/hosts` with the changes I needed) using `wrfile /etc/hosts.new`.

4. Verified the contents of `/etc/hosts.new` using rdfile.

5. Renamed `/etc/hosts` to `/etc/hosts.setup` using `mv`.

6. Renamed `/etc/hosts.new` to `/etc/hosts` using `mv`.

7. Rebooted the storage system for the change to take effect.

Upon the reboot, the storage system was now using the new IP addresses requested by the customer. Problem solved!
