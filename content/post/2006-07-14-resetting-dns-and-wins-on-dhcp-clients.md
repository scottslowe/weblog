---
author: slowe
categories: Education
comments: false
date: 2006-07-14T14:36:17Z
slug: resetting-dns-and-wins-on-dhcp-clients
tags:
- Microsoft
- Networking
- Windows
title: Resetting DNS and WINS on DHCP Clients
url: /2006/07/14/resetting-dns-and-wins-on-dhcp-clients/
wordpress_id: 297
---

As you probably know, Windows-based systems can be DHCP clients (and obtain their IP address, subnet mask, and gateway from DHCP) but use hard-coded DNS servers or WINS servers. Right off the top of my head, I can't really think of any reasons _why_ this is useful, or when it may be necessary, but the point remains that it is possible, and invariably this situation crops up during standardization or migration projects. The problem with this situation is that changes made to the DHCP server (such as handing out new DNS server or new WINS server addresses) won't be properly reflected on those workstations that have hard-coded values for these configuration settings.

So, we need to find a way to reset these DHCP clients to revert back to DHCP for these values instead of using the hard-coded entries.

Resetting the DNS server search order on a DHCP client to use the DNS servers passed down via DHCP is pretty easy. In this sitaution, we'll modify the technique described earlier to [remotely set DNS and WINS server addresses][1]. However, instead of setting a specific DNS server, we'll essentially set the value to nothing:

    wmic nicconfig where (IPEnabled=TRUE and DHCPEnabled=TRUE) 
    call SetDNSServerSearchOrder ()

As described in earlier posts, this command uses WMIC to set an empty DNS server search order for all NICs that are both IP-enabled and DHCP-enabled. As with all other WMIC examples that have been presented here, this command is remotely executable (using the `/node:<PC name>` switch), and it can be made to work with multiple machines using either the `/node:@<filename>` syntax or using `for /f` to pipe results into the `/node` switch. All of these examples have been demonstrated already, so we won't delve into them again here. Refer to back to any of these articles for more information:

* [Remotely Enabling Remote Desktop][2]

* [Setting DNS and WINS Server Addresses Remotely][1]

* [Remotely Setting the DNS Suffix Search Order][3]

All of these articles discuss the use of WMIC and provide examples of how to execute the WMIC commands against multiple remote systems.

Resetting the WINS servers remotely, however, is a very different story. In this case, the WMIC command for setting the WINS servers won't allow you to specify an empty value, or any other value that does not appear to be a valid IP address. In addition, as mentioned when discussing how to remotely set the DNS and WINS server addresses, the WMIC command always wants both the primary and secondary WINS server addresses; it does not appear to be possible to set only one or the other.

Netsh (network shell, available in [Windows 2000](http://www.microsoft.com/windows2000/default.mspx), [Windows XP](http://www.microsoft.com/windowsxp/default.mspx), and [Windows Server 2003](http://www.microsoft.com/windowsserver2003/default.mspx)), on the other hand, does provide a way to set both the DNS servers and the WINS servers back to DHCP-supplied via a command line, and netsh does offer a remote option (-r), as well as the ability to supply alternate credentials (via the -u and -p switches to provide username and password, respectively). It would seem like that's exactly the tool to use, and it is---sort of.

The problem with Netsh when being used remotely, however, is that it _won't allow_ changes to DNS or WINS servers. Specifically, the command that would be used to reset WINS servers to DHCP is this one:

    netsh interface ip set wins 
    name="Local Area Connection" source=dhcp

(Be sure to type this all on a single line.) This works just fine when executed locally (tested on both Windows Server 2003 and Windows XP), but refuses to work when executed remotely using the "-r" switch.

The trick here, then, is to find a way to spawn this command on a remote system so that it is executed locally, i.e., to be able to issue a command on a central server that reaches out to a workstation and spawns this command as a local process. That's where WMIC comes back into the picture.

This WMIC command will spawn a process remotely:

    wmic /node:pc01 process call create "cmd.exe"

Using some switches for the Windows command shell (`cmd.exe`), we can build a command to launch `cmd.exe` and then issue the `netsh` command we need. This is the final result:

    wmic /node:<PC name> process call create 
    'cmd.exe /c "netsh interface ip set wins name="Local Area Connection" 
    source=dhcp"'

Pay close attention to the quotes---those are single quotes around the entire command line, then double quotes around the Netsh command and the name of the network adapter. As it turns out, the default behavior of `cmd.exe` is to strip off the first double quote (in this case, the one just before `netsh`) and the last double quote (just after the `source=dhcp` parameter) and leave the remaining. The single quotes are used to bind the entire command line together, so that when it is parsed first by `cmd.exe` on the local system where the command is being typed it will work correctly. I tried several different combinations of single and double quotes and this worked; you may be able to find other combinations (if so, please let me know by posting a comment to this article).

By extension, of course, you could also use the same technique to reset the DNS servers:

    wmic /node:<PC name> process call create 
    'cmd.exe /c "netsh interface ip set dns name="Local Area Connection" 
    source=dhcp"'

It should go without saying, but I'll say it anyway: we can easily modify this command to act upon multiple remote computers by using an input file or by piping command results to this command. Refer to any of the linked articles above for more information and examples.

**UPDATE:** It has come to my attention that you could also use the [PsExec](http://www.sysinternals.com/Utilities/PsExec.html) tool, from [SysInternals](http://www.sysinternals.com/), to replace the `wmic process call create` command. This should achieve the same effect, but I haven't yet had the time to check that myself and fully document the command line.

[1]: {{< relref "2006-07-07-setting-dns-and-wins-server-addresses-remotely.md" >}}
[2]: {{< relref "2006-07-13-remotely-enabling-remote-desktop.md" >}}
[3]: {{< relref "2006-07-06-remotely-setting-the-dns-suffix-search-order.md" >}}
