---
author: slowe
categories: Information
comments: false
date: 2005-12-23T14:50:45Z
slug: centos-ntpd-problem-mostly-resolved
tags:
- BSD
- CentOS
- Linux
- Networking
- OSS
title: CentOS NTPd Problem (Mostly) Resolved
url: /2005/12/23/centos-ntpd-problem-mostly-resolved/
wordpress_id: 144
---

The NTPd problem that I [wrestled with in CentOS 4.1][1] and [again in CentOS 4.2][2] has finally been resolved. Mostly. I think. The specific steps I took to resolve the issue came from a number of sources, so read on for all the details.

Since these servers are virtual servers running under [VMware][3], I first consulted the VMware Knowledge Base and turned up this article on [slow and fast clocks for Linux guest VMs][4]. Based on that information, I added a few extra commands to the grub configuration:

    noapic nosmp nolapic clock=pit

In addition, I found a number of forum postings in various sites (too many to list or link here) that referenced problems with NTP and ACPI. So, based on that information, I further edited the grub configuration to look like this:

    noapic nosmp nolapic clock=pit acpi=no

Finally, based on information regarding NTP itself and the NTP configuration parameters, I added the "burst iburst" parameters to the server lines in my `ntp.conf` file, like this:

    server W.X.Y.Z burst iburst

This helped, as at least now NTP would synchronize against something other than the local clock (which was more than it had done previously). For some reason, though, the `/var/log/messages` log file was filling up with messages about synchronizing against the local clock, then synchronizing against the server, then against the local clock, etc. (You get the picture.)

Given that I was synchronizing against a [Windows Server 2003][5]-based computer, I thought perhaps that Microsoft's NTP implementation was simply broken. (This certainly wouldn't be the first time.) So I configured [OpenNTPd][6] on an [OpenBSD][7] server (running OpenBSD 3.8) and re-configured the CentOS server to synchronize against the OpenBSD NTP server.

&lt;aside&gt;Let me just say: OpenNTPd is ridiculously simple to configure and operate, and _just works._&lt;/aside&gt;

The repetitive synchronization messages still appeared, but were appearing with far less frequency. And the time still isn't synchronized to the level I would like, but it does stay within a minute or so of the rest of the network (well within the 5 minute gap required in order for Kerberos authentication to work).

So, it's still not working as cleanly as the older Red Hat Linux 9.0-based servers, but it _is_ working. Given that I'm still running these servers under an old version of VMware (which, technically, doesn't support Linux 2.6 kernels) I may try upgrading to a new version of VMware to see if that helps at all.

[1]: {{< relref "2005-08-16-strange-ntpd-problem-on-centos-41.md" >}}
[2]: {{< relref "2005-12-19-ntpd-on-centos-42.md" >}}
[3]: http://www.vmware.com/
[4]: http://www.vmware.com/support/kb/enduser/std_adp.php?p_sid=Nuh3rAXh&p_lva=&p_faqid=1420&p_created=1093994398&p_sp=cF9zcmNoPTEmcF9ncmlkc29ydD0mcF9yb3dfY250PTc0MyZwX3NlYXJjaF90ZXh0PUxpbnV4IGd1ZXN0IGNsb2NrJnBfc2VhcmNoX3R5cGU9NyZwX3Byb2RfbHZsMT1_YW55fiZwX3Byb2RfbHZsMj1_YW55fiZwX3NvcnRfYnk9ZGZsdCZwX3BhZ2U9MQ**&p_li=
[5]: http://www.microsoft.com/windowsserver2003/default.mspx
[6]: http://www.openntpd.org/
[7]: http://www.openbsd.org/
