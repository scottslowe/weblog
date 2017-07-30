---
author: slowe
categories: News
comments: true
date: 2008-08-11T23:41:51Z
slug: apparent-datetime-issue-with-update-2
tags:
- ESX
- ESXi
- Virtualization
- VMware
title: Apparent Date/Time Issue With Update 2
url: /2008/08/11/apparent-datetime-issue-with-update-2/
wordpress_id: 813
---

Reader Rince pointed out [this VMware Communities thread](http://communities.vmware.com/thread/162377?tstart=0) that has highlighted a pretty serious problem with VMware Infrastructure 3 version 3.5 Update 2. This problem seems to affect both ESX and ESXi.

The problem is that as soon as the date hits August 12, 2008, all VMs refuse to power on and an error is logged indicating that the product's license has expired. If VMs are already running, they should be fine; the problem, as I understand, only prevents users from powering on new VMs or performing VMotion operations.

If you are being affected by this bug, then your only real option at this point is to disable NTP (if it is being used) and set your date back a couple of days.

There's no firm word yet on when a fix will be available from VMware.

Rince, thanks for the heads-up!

## Update as of 9AM ET August 12, 2008

Updated ISOs and TARs for Update 2 (let's call them Update 2 v2) will be available by noon PT tomorrow, August 13. In the meantime, the following workaround should help:

1. Disable NTPd on the ESX hosts.

2. Set the date on the ESX hosts back to some point prior to August 12, 2008. Some people have suggested using the same day and month but a completely different year in the past; this will make it easier to repair ESX log files using search/replace.

3. Set the VM to boot into the BIOS.

4. Boot the VM. In the BIOS, set the date and time properly. This will be incorrect because it gets inherited from the host. This is independent of the time sync functionality in VMware Tools. If you don't fix this, VMs will boot up with the wrong date and/or time and that will cause additional problems (like Kerberos authentication within Active Directory).

5. After the VM has booted, ensure that time sync within VMware Tools has been disabled. Configure time sync to a reliable source. Active Directory usually handles this for Windows-based systems in a domain.

This workaround is only necessary to power on a VM, resume a suspended VM, or perform a VMotion operation. If all your VMs are currently up and running, just disable DRS.

Some have also suggested disabling HA. If you disable HA, then you will lose the ability for the VMs to be registered on another host if a host in the cluster fails. However, if you leave HA enabled, then without this workaround the VMs won't boot anyway (but they will be registered). If you choose to leave HA enabled, just be aware that you will need to use the workaround in order for the VMs to actually power up after a host failure.

Hopefully this information will help. I'll post more information as it becomes available.

## Update as of 3PM ET August 12, 2008

VMware will apparently release an express patch to specifically address this issue by 6PM PT (9PM ET) today, August 12, 2008. As soon as I get word that the patch has been released, I will post an update here.

## Update as of 11:15PM ET August 12, 2008

An express patch specifically to correct this issue has been released. It is available for [download from VMware's site](http://www.vmware.com/download/). Also available there are links to new KB articles for ESX and ESXi that describe deployment and installation considerations.

## Update as of 9AM ET August 13, 2008

I received word from VMware that the express patch released last night is fully compatible with VMware Update Manager. This should help simplify the deployment of the patch to affected servers.
