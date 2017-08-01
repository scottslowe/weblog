---
author: slowe
categories: Information
comments: true
date: 2008-02-04T13:22:39Z
slug: netsh-shortcuts
tags:
- Microsoft
- Networking
- Windows
title: Netsh Shortcuts
url: /2008/02/04/netsh-shortcuts/
wordpress_id: 621
---

Reader Tom Spis read my article on [resetting DNS and WINS on DHCP clients][1] and shared some insight on shortening some of the Netsh commands used in that article.

The key command referenced in that article was this one:

	netsh interface ip set wins name="Local Area Connection" source=dhcp

Tom points out that this command could be shortened:

>You can use netsh in a more concise way by:  
>
>1. Using the shortest possible unambiguos commands, and  
>
>2. Skipping parameter labels altogether  
>
>So, for example, I would normally write the above command as:  
>
>`netsh int ip set wins local dhcp`
>
>If you want to be really extreme (although I think you'd end up wasting more time just thinking about it), you can also write it as:  
>
>`netsh i ip se w l d`

Tom goes on to point out, though, that this extremely shortened version makes the assumption that there is only a single network interface card (NIC) in the system; otherwise, you'd have to spell out the full "Local Area Connection 2" and not use the abbreviated syntax.

In addition, Tom has some good information to share regarding Windows Server 2008 and Vista:

>On a related note, be aware that on Vista and Windows Server 2008, the ip subcontext has been renamed to ipv4, so any scripts that use ip would break on those platform versions. For that reason, in scripts, I would recommend preceding the netsh command with the following line:  
>
>`wmic os get version | find "6." && set ipver=ipv4 || set ipver=ip`
>
>and respectively changing the netsh command to  
>
>`netsh int %ipver% set wins local dhcp`

Good information, Tom! I appreciate you sharing the information with us. Readers are always welcome---encouraged, in fact---to share this kind of information, either in the comments or via e-mail to me directly.

[1]: {{< relref "2006-07-14-resetting-dns-and-wins-on-dhcp-clients.md" >}}
