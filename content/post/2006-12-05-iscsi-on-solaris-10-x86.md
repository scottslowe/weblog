---
author: slowe
categories: Tutorial
comments: true
date: 2006-12-05T15:50:35Z
slug: iscsi-on-solaris-10-x86
tags:
- CentOS
- Interoperability
- iSCSI
- Networking
- Solaris
- Storage
- UNIX
title: iSCSI on Solaris 10 x86
url: /2006/12/05/iscsi-on-solaris-10-x86/
wordpress_id: 379
---

Given that I'm neither a Solaris expert (yet) nor an iSCSI expert (yet), I knew that it would be a bit of a challenge to make this work. Fortunately, a found a [very useful blog posting by Frank Berger](http://www.fm-berger.de/blog/index.php/2006/08/) that gave me the framework I needed to get started. From there, Sun's documentation provided the rest of the necessary details. Perhaps this documentation will prove moderately useful as well.

First, I added the following lines to the `/etc/ietd.conf` on the CentOS iSCSI target server:

    Target iqn.2006-08.net.example:server.lun1
            IncomingUser username complicatedpassword
            Lun 1 Path=/dev/vg00/isanvol1,Type=fileio
            Alias iet-lun1
            MaxConnections          8
            InitialR2T              No
            ImmediateData           Yes

A quick restart of the iSCSI target service and I was all set on the target side. If you were going to do this yourself in your own environment, you'd need to modify the "IncomingUser" and "Lun X Path". In this instance I'm using LVM on CentOS, so my path specifies a logical volume in a volume group. Your configuration will differ, obviously. Alternately, if you are using a different iSCSI target implementation, you'd need to configure it appropriately. (I hope to be able to do some testing against a NetApp iSCSI target in the near future.)

On the initiator side, everything is done with the `iscsiadm` command. This command is fairly self-explanatory and has built-in help ("-?") throughout most of the options, but it did take me a little bit of time to get things working.

First, we have to make sure that the iSCSI initiator is online:

    svcs -a | grep iscsi

If disabled, then we can enable it with this command:

    svcadm enable svc:/network/iscsi_initiator

From there, we configure the iSCSI initiator:

    iscsiadm add discovery-address 10.1.1.1:3260
    iscsiadm modify initiator-node -a CHAP
    iscsiadm modify initiator-node -H username
    iscsiadm modify initiator-node -C
    (specify CHAP password)
    iscsiadm modify discovery --sendtargets enable

Because I'd also specified a static config as well (dynamic discovery didn't seem to be working as I expected; more on that in a moment), using `iscsiadm list target` now returned two iSCSI targets. They appeared to be different targets, and since I do have two targets defined on the iSCSI server (one for VMware and one for this), I didn't want to take any chances on affecting the VMware LUN. So, I removed and disabled the dynamic discovery, removed the static config, and then re-added the static config:

    iscsiadm add static-config iqn.2006-08.net.example:server.lun1,
    10.1.1.1:3260

(This should all be on one line; it was wrapped here for readability.) After doing that, `iscsiadm list target` showed only a single target identified as "server.lun1", which assured me that I wasn't seeing the VMware LUN.

&lt;aside&gt;In a more complex environment, how does one ensure that iSCSI LUNs are properly isolated from unwanted hosts? The "IncomingUser" parameter was different between my VMware LUN and the raw LUN being presented to Solaris, so in theory I would have been safe. Better safe than sorry, in my opinion.&lt;/aside&gt;

After I was sure that the iSCSI initiator was properly seeing the LUN, then I created a new device, created a new filesystem on that device, and then mounted it:

    devfsadm -c iscsi
    format <em>(selected new disk identified as iSCSI)</em>
    newfs /dev/dsk/c2t1d0s2
    mount /dev/dsk/c2t1d0s2 /export/iscsi

Of course, you'll need to modify the above commands slightly depending upon your configuration, but the overall process should be pretty close to what I've outlined above.
