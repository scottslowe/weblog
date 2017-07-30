---
author: slowe
categories: Education
comments: false
date: 2006-07-06T16:22:23Z
slug: remotely-setting-the-dns-suffix-search-order
tags:
- ActiveDirectory
- Microsoft
- Windows
title: Remotely Setting the DNS Suffix Search Order
url: /2006/07/06/remotely-setting-the-dns-suffix-search-order/
wordpress_id: 293
---

Invariably, larger organizations end up with a fragmented DNS namespace that has grown over the years due to name changes and acquisitions. As resources move between domains (DNS domains and Active Directory domains), it soon becomes necessary to have clients resolve hostnames even when the fully qualified domain name (FQDN) isn't provided by the user. The DNS suffix search order provides that functionality, and is very often a key part of maintaining connectivity in these separate namespaces.

Because the DNS suffix search order can't be set via DHCP (for Windows clients, at least), it has to be set some other way. [Windows Server 2003](http://www.microsoft.com/windowsserver2003/default.mspx) adds this functionality into Group Policy, so that a GPO can enforce the DNS suffix search order that way. [Windows XP](http://www.microsoft.com/windowsxp/default.mspx) or higher is required to support that GPO functionality on the client side.

If it can't be done via GPO, though, there aren't a lot of options---unless you are using Windows XP and/or Windows Server 2003 and can use WMIC (WMI Command-line). The full power of WMIC is outside the scope of this article (it's quite likely you'll see more from me about WMIC as time progresses), but we can at least look at using WMIC to set the DNS suffix search order on client systems.

First, let's look at the full command, then we'll break it down. Here's the full command:

    wmic /node:@input.txt nicconfig call SetDNSSuffixSearchOrder 
    (legacydomain.com,newdomain.net,some.other.domain.org)

OK, so what does this command do, exactly? Here's the breakdown.

* The `wmic /node:` invokes WMIC, targeted at a remote computer. This allows us to run the command centrally, on one system, yet affect remote computers.

* The `@input.txt` parameter tells WMIC to use the contents of the file input.txt to specify the names of the remote computers that will be configured with this command. This makes it easy for us to export a list of computer names into a text file and then plug that text file into this WMIC command.

* The `nicconfig` is the WMIC _alias_ that allows us to configure NIC properties. A full set of aliases can be viewed by typing `wmic /?`.

* The `call` parameter is the _verb_, followed by the action being called. In this particular case, we're using the `SetDNSSuffixSearchOrder`; this is, in turn, followed by the list of DNS domains to be included in the DNS suffix search list.

By combining WMIC with the handy `for /f` batch command, we can do even more. For example, let's say we wanted to use `dsquery.exe` to produce a list of all the computers in a particular OU. That's pretty straightforward:

    dsquery computer "ou=Workstations,dc=example,dc=net" -o rdn

The "-o rdn" switch is used here to output the _relative distinguished name (RDN)_ instead of the full DN.

Embed that command into a for loop like so:

    for /f "tokens=1" %1 in ('dsquery computer 
    "ou=Workstations,dc=example,dc=net" -o rdn') do 
    @wmic /node:%1 nicconfig call SetDNSSuffixSearchOrder 
    (example.net,subdomain.example.net,otherdomain.org)

And just like that, we've taken the dynamic output of the `dsquery.exe` tool and plugged into WMIC to set the DNS suffix search order for all the systems in a particular OU in Active Directory.

As a side note, it's possible to use this WMIC command across domains (even without a trust relationship) by adding `/user:<username>` and `/password:<password>` switches to the WMIC command. This will allow WMIC to authenticate to the remote system as the specified user with the specified password.

It's also possible to use WMIC to set other parameters, such as DNS server search order and WINS servers. However, setting these parameters via WMIC also requires that we identify the name of the specific network interface card, and that may vary from system to system. I'll try to work on some examples and post those here as soon as I have them working reliably.

**UPDATE:** I failed to mention another important switch for WMIC; namely, the `/FAILFAST:ON` switch. This switch causes WMIC to ping the remote node first as a connectivity test, and proceed with the operation only if the remote node is reachable. This will eliminate delays waiting for timeouts for remote systems that are shut down or otherwise unreachable.

**UPDATE 2:** `dsquery.exe` has a `-limit` switch that can control how many objects will be returned by the query. That switch is not included in the command line above; be sure to add `-limit 0` or `-limit X` to either bypass the limit or limit the query to X objects, respectively.
