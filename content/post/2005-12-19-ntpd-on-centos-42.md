---
author: slowe
categories: Rant
comments: false
date: 2005-12-19T16:08:20Z
slug: ntpd-on-centos-42
tags:
- CentOS
- Linux
- Networking
- RedHat
title: NTPd on CentOS 4.2
url: /2005/12/19/ntpd-on-centos-42/
wordpress_id: 142
---

A while back, I started working with [CentOS][1] (version 4.1 at the time, see my [impressions][2] here) as a candidate replacement for the Red Hat Linux 9.0-based servers still in service on the network. Although I was (and still am) very comfortable with Red Hat Linux 9.0, it's getting a bit long in the tooth and I wanted to update the servers to a more modern software base.

The problem is, I kept running into [issues with time synchronization][3] on the CentOS server. This time synchronization issue did _not_ appear on the Red Hat Linux servers. I tried to resolve the issue, but never did find the solution. Instead, I just used ntpdate in a cron job. That seemed to work well enough, but I really wanted it to work PROPERLY.

So I started working with CentOS 4.2 earlier this week, hoping that whatever had plagued me with the previous version wouldn't afflict me this time. Unfortunately, the exact same problem has cropped up again, and I am at a loss trying to figure out why it doesn't work. The output from `ntpq -p` indicates that communication is occurring with the desired time servers, but the NTP daemon still keeps choosing to synchronize with the local clock. SELinux is disabled, so it can't be interfering with NTP (I think that was the problem with the earlier CentOS 4.1 installation), iptables is not running (so it can't be a firewall issue), file permissions and ownership look OK---what could I be missing? If anyone has any ideas, please let me know.

[1]: http://www.centos.org/
[2]: {{< relref "2005-08-08-brief-impressions-of-centos-41.md" >}}
[3]: {{< relref "2005-08-16-strange-ntpd-problem-on-centos-41.md" >}}
