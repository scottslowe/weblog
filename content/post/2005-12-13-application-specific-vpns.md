---
author: slowe
categories: Explanation
comments: false
date: 2005-12-13T11:18:37Z
slug: application-specific-vpns
tags:
- Encryption
- IPSec
- Linux
- macOS
- Networking
- OSS
- Security
- SSH
- SSL
title: Application-Specific VPNs
url: /2005/12/13/application-specific-vpns/
wordpress_id: 140
---

I coined the term "application specific VPNs" as I began exploring the many uses of tools such as SSH (the [OpenSSH][1] implementation, in particular) and [Stunnel][2] (the open source [SSL][3] wrapper). An "application specific VPN" is a technique for employing encrypted communication with a particular remote application that does not affect the operation of other applications (local or remote) on the system.

In general, most VPNs employ technology such as [PPTP][4] or [IPSec][5]. These protocols usually affect _all_ traffic originating from that system (unless we use split tunneling). With an application specific VPN, only particular types of traffic are affected, and these particular types of traffic are wrapped in an encrypted SSH or SSL tunnel using either OpenSSH or Stunnel, respectively. Other types of traffic are unaffected.

While this may seem old hat to long-time Linux, Unix, or [Mac OS X][6] users, this is a relatively new concept to other platforms. Being a (fairly) new convert to Mac OS X myself, I found myself enjoying the tremendous flexibility offered by application specific VPNs. However, this functionality is certainly not limited to these platforms, and it is possible for [Windows][7] users to utilize this functionality as well.

### Some Examples

Here are a few examples of how you can use application specific VPNs:

* [Citrix Presentation Server][8]: Users can wrap Citrix ICA traffic in SSL using Stunnel; this avoids the need to pay for commercial solutions (although, quite honestly, the commercial solutions are typically easier to deploy and manage).

* E-Mail: Lots of people talk about securing e-mail, but not usually in the context of securing client-to-server traffic. With Stunnel, you can add SSL support for IMAP, POP, and/or SMTP even if the server doesn't support SSL natively. In fact, you can add Stunnel to [Microsoft Exchange][9] to help bolster that product's built-in support for SSL (such as using SMTPS instead of STARTTLS for encrypting SMTP traffic).

* Remote Desktop: This is a great technique for sysadmins. Need to use Remote Desktop to manage a Windows-based server remotely, but don't want to run into security problems? Use Stunnel or OpenSSH to encrypt the RDP traffic. Stunnel, in my opinion, is particularly good here, since it's incredibly easy to use the Windows port of Stunnel to establish an SSL listener on an arbitrary high port for this very purpose.

### Using Stunnel for SSL-based Application Specific VPNs

On Windows, there is no graphical user interface (GUI) for configuring Stunnel; all configuration must be done with the `stunnel.conf` configuration file. A sample `stunnel.conf` file is found below; this file listens on TCP port 1494 and forwards traffic to TCP port 3389 on the same system.  (Note that this would be a sample `stunnel.conf` file for a server-side configuration.)

    CApath = c:\windows\system32\stunnel
    cert = c:\windows\system32\stunnel\stunnel.pem
    client = no
    service = SSLTunnel
    [rdp]
    accept = 1494
    connect = 3389

Once Stunnel has been configured, running `stunnel --install` will install it as a system service; this service can then be stopped and started through the Services MMC console just like any other background service.

Of course, this is only half the work; you'll also need an equivalent Stunnel configuration on the client side, since the Microsoft RDP client (the [Windows version][10] or the [Mac OS X][11] version) does not support SSL. Also, note that the SSL certificate used by Stunnel needs to be in PEM format (which is a nice lead-in to my article discussing the [conversion of certificates][xref-1]).

On Mac OS X, there is a graphical utility for managing Stunnel called SSL Enabler. (Note that I was not able to find a home page for this utility; downloads can be found at various sites.)

On Linux, the configuration again involves editing the `stunnel.conf` file and launching stunnel (either in the foreground or as a daemon). I'm not aware of any GUI utilities for configuring Stunnel on Linux, but that certainly doesn't mean they don't exist.

### Using SSH for Application Specific VPNs

As with Stunnel, it's also possible to use SSH to create application specific VPNs. Mac OS X comes with OpenSSH, as do most distributions of Linux and various flavors of BSD. Windows users can also find various ports of OpenSSH and utilities such as [PuTTY][13] that make it possible to use SSH for application specific VPNs as well.

On Mac OS X, the following command in Terminal will create an application specific VPN to encrypt IMAP traffic to a remote server:

    ssh -L 1143:imapserver.domain.com:143 -N -f user@sshserver.domain.com

This same technique could be used to encrypt WebDAV traffic to a remote web server, other types of mail traffic (POP3 or SMTP, for example), RDP (for Remote Desktop), etc. Note that it doesn't work well with HTTP traffic that requires the use of host headers.

There are a few graphical utilities on Mac OS X for managing SSH tunnels; these include [SSH Tunnel Manager][14] and [AlmostVPN][15].

Not being a regular day-to-day Linux user (not on the desktop, at least), I don't know of any SSH tunnel management applications, but I would be very surprised if they didn't exist.

### Creating Site-to-Site Application Specific VPNs

Most of the examples so far have been using SSH and/or Stunnel to connect endpoints (i.e., a single laptop or desktop computer) to a remote resource via an application specific VPN. However, it's also easily possible to create application specific site-to-site VPNs, whose purpose is to secure only a particular type of traffic between two locations.

Consider this example. CompanyA wants to send e-mail to CompanyB, but wants that e-mail to be secure. If both mail servers support TLS, then the organizations could use TLS to secure the SMTP traffic. If not, then the companies can use Stunnel to establish an SSL connection between two systems. HostA at CompanyA can use Stunnel as a client side application to listen for unencrypted connections and pass them as encrypted connections to HostB at CompanyB. HostB at CompanyB is using Stunnel to listen for encrypted connections and passes them unencrypted to the mail server at CompanyB. With a simple configuration, the mail servers at both companies can be configured to pass mail connections for other company through the Stunnel connection. With no additional cost and very little configuration, all e-mail traffic between the two organizations has now been secured. All this without the complexity of a typical B-to-B VPN and the associated access controls.


[1]: http://www.openssh.org/
[2]: http://stunnel.mirt.net/index.html
[3]: http://en.wikipedia.org/wiki/Secure_Sockets_Layer
[4]: http://en.wikipedia.org/wiki/PPTP
[5]: http://en.wikipedia.org/wiki/Ipsec
[6]: http://www.apple.com/macosx/
[7]: http://www.microsoft.com/windows/
[8]: http://www.citrix.com/English/ps2/products/product.asp?contentID=186&ntref=PROHOME_Main
[9]: http://www.microsoft.com/exchange/default.mspx
[10]: http://www.microsoft.com/downloads/details.aspx?FamilyID=80111f21-d48d-426e-96c2-08aa2bd23a49&DisplayLang=en
[11]: http://www.microsoft.com/mac/otherproducts/otherproducts.aspx?pid=remotedesktopclient
[13]: http://www.chiark.greenend.org.uk/~sgtatham/putty/
[14]: http://projects.tynsoe.org/en/stm/
[15]: http://www.leapingbytes.com/almostvpn
[xref-1]: {{< relref "2005-12-02-certificate-conversion-using-openssl.md" >}}
