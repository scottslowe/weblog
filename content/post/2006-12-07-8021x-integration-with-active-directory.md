---
author: slowe
categories: Tutorial
comments: true
date: 2006-12-07T14:36:07Z
slug: 8021x-integration-with-active-directory
tags:
- Cisco
- IOS
- Macintosh
- Networking
- Security
- Microsoft
- Interoperability
title: 802.1x Integration with Active Directory
url: /2006/12/07/8021x-integration-with-active-directory/
wordpress_id: 382
---

The idea behind [802.1x](http://en.wikipedia.org/wiki/802.1x) is to provide Layer 2 authentication; that is, to authenticate LAN clients at the Ethernet layer. (This is before the client gets a DHCP lease or anything of that nature.) With 802.1x in place, rogue users can't just tap into a physical connection on the network. In order to gain network connectivity, the device must authenticate before network traffic is allowed.

The idea here is to configure 802.1x authentication on a network switch in such a way as to leverage the existing authentication infrastructure provided by Active Directory. Like it or not, Active Directory is a widely deployed directory service and leveraging it where we can will certainly provide an advantage. This process uses RADIUS to provide an interface between a Cisco Catalyst 3560G switch (the 802.1x authenticator in this scenario) and Active Directory. I could only test [Mac OS X](http://www.apple.com/macosx/) as the client (or 802.1x supplicant), but I'm confident that the configuration will work equally well with [Windows XP Professional](http://www.microsoft.com/windowsxp/).

### Configuring the Cisco Catalyst 3560G

The Catalyst switch I used in this configuration was running IOS 12.2(25); please note that the commands listed here may be different in different versions of IOS.

To configure the switch for 802.1x authentication, three steps are involved:

1. Enable 802.1x authentication on the switch (global configuration).

2. Configure the RADIUS server(s) to which the switch will communicate for authentication requests.

3. Enable 802.1x authentication on the individual ports.

([This document](http://www.cisco.com/en/US/products/hw/switches/ps646/products_configuration_guide_chapter09186a00801cdf3a.html) from the [Cisco web site](http://www.cisco.com/) was tremendously helpful in configuring 802.1x.)

First, to enable 802.1x authentication on the switch, use the following commands in global configuration mode:

	aaa new-model  
	aaa authentication dot1x default group radius  
	aaa authorization network default group radius  
	dot1x system-auth-control

This enables 802.1x globally on the switch, but none of the interfaces are enabled for 802.1x authentication. Next, we configure the RADIUS server(s) to which the switch will pass the 802.1x authentication traffic. That's handled with these commands in global configuration mode:

	radius-server host 10.1.1.254 auth-port 1645 acct-port 1646 key Password

(This should all be on one line.) Note that the "auth-port" and "acct-port" parameters are only necessary if you are using nonstandard ports. Since Microsoft's IAS (Internet Authentication Service, which provides the RADIUS interface to Active Directory) uses both sets of standard ports (1645/1812 and 1646/1813) you won't need to specify these parameters. The "key" parameter is a shared secret key between the RADIUS client (the switch) and the RADIUS server. Obviously, you'll want to use something other than "Password".

Finally, to enable 802.1x on the applicable interfaces, you'll use these commands in interface configuration mode (replace `gi0/23` with the interface you want to configure):

	int gi0/23  
	dot1x port-control auto

That enables 802.1x authentication on that specific port. Repeat the process for all ports that should use 802.1x authentication. Note that some ports can't be enabled for 802.1x authentication; most notably, trunk ports can't be used for 802.1x. Refer to the Cisco documentation (or the documentation from your particular vendor) for complete details on the limitations.

Now that the switch is configured, we move on to configuring Active Directory.

### Configuring Active Directory and IAS

I suppose that saying we need to "configure Active Directory" isn't entirely accurate, since no configuration changes and no schema extensions are necessary to make this work. All that really needs to be done is to enable reversible password encryption (which can be done on a per-user basis) and setup Internet Authentication Service (IAS).

First, regarding reversible password encryption: The configuration described here uses MD5 hashes (passwords) to authenticate clients to the network. There are other methods, such as digital certificates, to accomplish the same thing, and I'll probably revisit this configuration again at a later date to look at using those. For now, though, the use of MD5 for authentication means that we have to enable reversible password encryption for every user that will need to authenticate via 802.1x, and those users will need to change their passwords after that change is made. A pain, yes, and a potential security concern, yes, but necessary at this point. (I won't bother going through the details of enabling reversible password encryption here; there are plenty of resources available on the Internet, like [this one](http://download.microsoft.com/download/b/0/e/b0e2a363-0044-4327-8f17-020818f57234/Wired_depl.doc), that provide that information.)

Configuring IAS is really pretty straightforward. I've discussed the use of IAS before (here in discussing [Cisco PIX-AD integration][1] and here regarding [WatchGuard Firebox-AD integration][2]), and I'll refer you back to those articles for some of the basics on setting up and configuring IAS.

To configure IAS in this instance (once it has been installed and registered with Active Directory), we'll do the following:

* Add the Cisco Catalyst switch as a RADIUS client. We'll need to be sure to specify the same shared secret as used in the switch configuration.

* We'll create a new remote access policy. The conditions on the policy should be "NAS-Port-Type" (set to Ethernet) and "Windows-Groups" (set to whatever group should be allowed to authenticate via 802.1x; I used Domain Users).

* The profile associated with this policy should be edited to note only the EAP MD5 authentication type (under "EAP Methods" on the Authentication tab); all other authentication types should be unchecked. In addition, all encryption types on the Encryption tab should be unchecked except for "No encryption".

At this point, the IAS configuration should be complete. Now for the final step: configuring the client to use 802.1x.

### Configuring the Client (Mac OS X)

As mentioned earlier, I didn't have a physical Windows XP Professional-based machine to test with, but I did do some testing with Mac OS X. Although the software used to configure the operating system is different, the overall configuration is similar and should work without any major hitches on Windows XP.

To configure Mac OS X, launch the Internet Connect software in the Applications folder and follow these steps:

1. From the File menu, choose "New 802.1X Connection...".

2. Specify a description and choose the appropriate network port (typically "Built-in Ethernet").

3. Specify a username and password.

4. For authentication types, click to enable MD5 and move it to the top of the list. Uncheck all other authentication types.

5. Click OK to save the connection.

Once the connection has been defined, you can plug your OS X-based system into one of the 802.1x-enabled ports and click "Connect" in the Internet Connect window. If everything is configured correctly, you should be connected and be able to pass network traffic without any issues. If things don't work, go back and check the switch configuration and the logs on the IAS/RADIUS server. In particular, the logs may indicate that an incorrect password was used, or you may be able to determine that the switch isn't even talking to the IAS/RADIUS server (perhaps a typo in the server address?).

By the way, configuring Mac OS X to use 802.1x for wireless connections is equally easy and done the same way (using Internet Connect). I used to regularly use my [MacBook Pro](http://www.apple.com/macbookpro/) in an environment that used 802.1x and EAP-FAST/LEAP for wireless authentication with no problems.

Future enhancements to this configuration include switching from EAP-MD5 to something like EAP-TLS or PEAP; this will avoid the need to enable reversible password encryption on the domain.

[1]: {{< relref "2005-11-22-cisco-pix-vpn-and-active-directory-integration.md" >}}
[2]: {{< relref "2005-12-06-watchguard-firebox-vpn-and-active-directory-integration.md" >}}
