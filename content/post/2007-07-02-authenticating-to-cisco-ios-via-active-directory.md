---
author: slowe
categories: Tutorial
comments: true
date: 2007-07-02T16:54:57Z
slug: authenticating-to-cisco-ios-via-active-directory
tags:
- ActiveDirectory
- Cisco
- Interoperability
- Networking
title: Authenticating to Cisco IOS via Active Directory
url: /2007/07/02/authenticating-to-cisco-ios-via-active-directory/
wordpress_id: 482
---

The following configuration will enable you to authenticate login requests to [Cisco](http://www.cisco.com/) equipment running IOS against Active Directory. This would, for example, allow you to centralize the authentication of your Cisco-based network infrastructure against Active Directory.

## Configuring the Cisco Equipment

The equipment I used in this configuration was a [Cisco Catalyst 3560G](http://www.cisco.com/en/US/products/hw/switches/ps5528/index.html) switch running IOS 12.2(25); please note that the commands listed here may be different in different versions of IOS. The commands should be roughly equivalent, however, across hardware platforms.

First, we must enable the external authentication mechanisms, then we'll specify the external authentication servers we're going to use. This is listed below:

1. First, to enable external authentication on the switch, use the `aaa new model` and `aaa authentication login default group radius local` commands in global configuration mode. This enables the authentication of login requests by RADIUS first, then by a local database (just in case network connectivity is down). We specify "local" as well because this configuration applies to both telnet requests as well as physical console requests.

2. Next, we specify the external authentication servers that the switch should use. This is done via the `radius-server host <host IP address> auth-port <host auth-port> acct-port <host acct-point> key <shared secret>` command.  Best practices dictate that you should have at least two RADIUS servers for redundancy. Note that the "auth-port" and "acct-port" parameters are only necessary if you are using nonstandard ports. Since Microsoft's IAS (Internet Authentication Service, which provides the RADIUS interface to Active Directory) uses both sets of standard ports (1645/1812 and 1646/1813) you won't need to specify these parameters. The "key" parameter is a shared secret key between the RADIUS client (the switch) and the RADIUS server.

Now we're ready to move to configuring the Windows servers that we'll use for RADIUS authentication.

## Configuring Internet Authentication Service (IAS)

Configuring IAS is rather simple. I've discussed the use of IAS before (here in discussing [Cisco PIX-AD integration][1] and here regarding [WatchGuard Firebox-AD integration][2]), and I'll refer you back to those articles for some of the basics on setting up and configuring IAS.

Note that these instructions are based on the version of IAS included with [Windows Server 2003 R2](http://www.microsoft.com/windowsserver/default.mspx); different versions may behave slightly differently.

To configure IAS in this instance (once it has been installed and registered with Active Directory), we'll do the following:

* Add the Cisco Catalyst switch as a RADIUS client. We'll need to be sure to specify the same shared secret as used in the switch configuration above. You can specify the Cisco switch either by DNS name (if it is registered in DNS) or by IP address.

* Create a new remote access policy that grants remote access permission. The conditions on the policy should be "NAS-IP-Address" (set to the IP address of the Cisco equipment) and "Windows-Groups" (set to whatever group should be allowed to authenticate to the switch; I created a group called "Cisco Admins" and used it).

* Configure the profile to use only PAP authentication and no encryption.

Repeat this process on the second Windows server running IAS (you did configure two for redundancy, didn't you?).

That's it! At this point, you should be able to telnet to the Cisco switch (or whatever IOS-based equipment you've configured) and log in with your Active Directory username and password. Once logged in, you can use your `enable` or `enable secret` password to enter privileged exec mode.

Now, before you go any farther, add a local account to use in case the network connectivity to the RADIUS server is lost:

	s1(config)#username localaccount password password123

(Obviously, you'll want to use a secure password!) This will ensure that if you lose network connectivity to the equipment, you can still get in through the serial console connection. Be warned: without this local account, you can be locked out of the equipment completely if the RADIUS server(s) are inaccessible!

[This Cisco document](http://www.cisco.com/en/US/tech/tk59/technologies_tech_note09186a0080093c81.shtml) offers some additional information on AAA configurations, so I'll refer you there for more detailed descriptions of the commands involved. Enjoy!

[1]: {{< relref "2005-11-22-cisco-pix-vpn-and-active-directory-integration.md" >}}
[2]: {{< relref "2005-12-06-watchguard-firebox-vpn-and-active-directory-integration.md" >}}
