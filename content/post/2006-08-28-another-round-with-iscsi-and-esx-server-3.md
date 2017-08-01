---
author: slowe
categories: Explanation
comments: true
date: 2006-08-28T23:37:11Z
slug: another-round-with-iscsi-and-esx-server-3
tags:
- Interoperability
- CentOS
- ESX
- iSCSI
- Linux
- Storage
- VMware
title: Another Round with iSCSI and ESX Server 3
url: /2006/08/28/another-round-with-iscsi-and-esx-server-3/
wordpress_id: 327
---

There were two driving factors that led me to rebuild the iSCSI-based storage that was currently serving the test lab, instead of continuing to use the Data ONTAP Simulator. First, we had acquired a Gigabit Ethernet backbone for the test lab, and I wasn't convinced that the Data ONTAP Simulator was taking full advantage of the Gigabit Ethernet NICs I installed in the server. I also wanted to test bonding some NICs together for more throughput, or possibly to try some multipathing. I couldn't do either of those with the Data ONTAP Simulator. Note that this is not a knock against the Data ONTAP Simulator; it's not designed or intended for those kinds of things. (It would be great if I could get a real [NetApp](http://www.netapp.com/) device in the lab, but I don't know if that will ever happen.)

After doing some brief research, I settled on using [CentOS](http://www.centos.org/) 4.3 and the [iSCSI Enterprise Target (IET)](http://iscsitarget.sourceforge.net/). The installation of CentOS was straightforward and simple, and the installation of IET was equally simple, due perhaps to [these fairly detailed instructions](http://mail.digicola.com/wiki/index.php?title=User:Martin:iSCSI) for installation of RHEL 4 (which are equally applicable to CentOS 4). I heartily recommend, based on my experience so far, using the source RPMs instead of building from source. It made the process easy and (almost) painless.

I setup a 10GB logical volume using LVM2 and configured IET to present it via iSCSI by editing `/etc/ietd.conf` to show this:

    Target iqn.2006-08.com.example:vmware.lun1
        IncomingUser isanuser secretpw
        Lun 0 Path=/dev/VolGroup00/lvol0,Type=fileio

(Obviously, you'd need to adjust this as appropriate for your own installation.)

Having already learned my lesson regarding the ESX firewall, I ensured that the software iSCSI initiator traffic was allowed outbound before continuing (refer [here][1] for more details). Using the Virtual Infrastructure client, I reconfigured the ESX 3 server to see the new iSCSI server, and the new LUN popped up immediately upon a rescan. From there, it was a simple operation to establish a new VMFS datastore on the iSCSI LUN and move a VM to the LUN. That was easy!

The next steps will be to do some performance tuning, test bonding the NICs and/or multipathing, and perform some NFS interoperability tests. (Remember that NFS is also supported by ESX 3 for datastores.)

[1]: {{< relref "2006-07-10-iscsi-and-esx-server-3.md" >}}
