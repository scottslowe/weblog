---
author: slowe
categories: News
comments: true
date: 2008-10-02T09:35:14Z
slug: now-ciscos-participation-in-svvp-becomes-clearer
tags:
- Cisco
- Microsoft
- Networking
- Virtualization
- Windows
title: Now Cisco's Participation in SVVP Becomes Clearer
url: /2008/10/02/now-ciscos-participation-in-svvp-becomes-clearer/
wordpress_id: 962
---

Some while ago, it was noted that [Cisco was signed up as a participant](http://www.virtualization.info/2008/08/cisco-vmware-signs-microsoft.html) in Microsoft's Server Virtualization Validation Program (SVVP). Many wondered why---what did Cisco have up its sleeve?

[This article](http://www.infoworld.com/article/08/10/02/Cisco_Microsoft_roll_out_server_networking_appliance_1.html) today from InfoWorld seems to make the story much clearer:

>With the new product, called Windows Server on WAAS, branch offices can host services locally including Active Directory, Microsoft Print Services, Microsoft Domain Name System Server and Microsoft Dynamic Host Configuration Protocol Server. That can improve performance for branch workers and reduce costs related to wide area network connectivity and branch systems management. An IT administrator can remotely manage the Windows Server functions using Microsoft System Center.
>
>Cisco used embedded virtualization technology in its appliance to enable Windows Server 2008 to run on it.

Now, the _real_ question is this: what "embedded virtualization technology" did Cisco use?

**UPDATE:** Based on the comments below, it looks like KVM is the technology Cisco chose to virtualize Windows Server on WAAS. Very interesting!
