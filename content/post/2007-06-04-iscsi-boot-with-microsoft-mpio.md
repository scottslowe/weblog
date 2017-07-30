---
author: slowe
categories: Explanation
comments: true
date: 2007-06-04T22:30:44Z
slug: iscsi-boot-with-microsoft-mpio
tags:
- iSCSI
- Interoperability
- NetApp
- Networking
- Storage
- Windows
title: iSCSI Boot with Microsoft MPIO
url: /2007/06/04/iscsi-boot-with-microsoft-mpio/
wordpress_id: 465
---

There's a small gotcha when using [Microsoft's iSCSI initiator](http://www.microsoft.com/downloads/details.aspx?familyid=12cb3c1a-15d6-4585-b385-befd1319f825&displaylang=en) and MPIO driver to do iSCSI multipathing: the Microsoft initiator and MPIO driver will overwrite the IQN of the iSCSI HBA. Obviously, this could cause problems where access control to iSCSI LUNs is based on initiator IQN.

As pointed out in this [Qlogic support document](http://kb.qlogic.com:8080/KanisaPlatform/Publishing/272/14322_f.html) (check the "Additional Notes" section at the bottom of the page), installation of the Microsoft iSCSI initiator will overwrite the IQN of the HBA with a Microsoft-generated IQN, like "1991-05.com.microsoft:servername.domain.com" or similar.

In environments where access to LUNs is controlled in part or in whole by initiator IQN, this is a problem. One such environment is [NetApp](http://www.netapp.com/) iSCSI SANs, where initiator groups (or "igroups") control access to LUNs based on the IQNs of the initiators. To work around this, you'll want to add the original IQNs of the HBAs (before the installation of the Microsoft iSCSI initiator) as well as the Microsoft IQN in the igroups for the LUNs that should be visible to that server. Otherwise, you could lose access to the LUN after installation of the Microsoft initiator.

(By the way, in case you're wondering why one would install the Microsoft iSCSI initiator when you've already got HBAs, there's a good reason---to get multipath support.)
