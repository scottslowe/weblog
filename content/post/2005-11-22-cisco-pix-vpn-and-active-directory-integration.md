---
author: slowe
categories: Tutorial
comments: true
date: 2005-11-22T19:01:54Z
slug: cisco-pix-vpn-and-active-directory-integration
tags:
- ActiveDirectory
- Cisco
- CLI
- Encryption
- Interoperability
- IPSec
- Networking
- Security
- VPN
- Windows
title: Cisco PIX VPN and Active Directory Integration
url: /2005/11/22/cisco-pix-vpn-and-active-directory-integration/
wordpress_id: 121
---

Rather than publishing this information in PDF form on my business website, I've decided to try something new and post it here as a blog entry. So, here goes.

This information assumes that you have some experience with the Cisco PIX firewall (i.e., you know how to enter configuration commands and have a basic idea of what the configuration commands actually do) as well as some experience with Windows and Active Directory.

With that information in hand, let's get started.

## Configuring the Cisco PIX

First, we'll need to setup the PIX firewall. Use the commands below to configure the PIX for PPTP-based VPN connections that will authenticate against an Active Directory back-end.

	ip local pool vpn-pool 10.10.10.1-10.10.10.254  
	aaa-server vpn-auth inside host 10.10.10.5 secretkey  
	aaa-server vpn-auth inside host 10.10.10.6 secretkey  
	aaa-server vpn-auth protocol radius  
	vpdn group vpn-pptp-group accept dialin pptp  
	vpdn group vpn-pptp-group ppp authentication mschap  
	vpdn group vpn-pptp-group ppp encryption mppe 128 required  
	vpdn group vpn-pptp-group client configuration \  
	address local vpn-pool  
	vpdn group vpn-pptp-group client configuration \  
	dns 10.10.10.5 10.10.10.6  
	vpdn group vpn-pptp-group client configuration \  
	wins 10.10.10.6 10.10.10.5  
	vpdn group vpn-pptp-group client authentication aaa vpn-auth  
	vpdn enable outside  
	sysopt connect permit-pptp  
	access-list acl-nat0 permit ip 10.10.1.0 255.255.255.0 \  
	10.10.10.0 255.255.255.0  
	nat (inside) 0 access-list acl-nat0

(Note that I have placed a backslash to indicate text that is wrapped onto two lines here but should be entered all on a single line in the PIX configuration.)

In this configuration, replace the IP addresses on lines 2 and 3 (the `aaa-server vpn-auth` commands) with the IP addresses of the servers running Internet Authentication Service (IAS) on Windows. See the next section for more information on configuring IAS.

On those same lines, replace the text `secretkey` with the RADIUS shared secret that will be used when configuring the RADIUS/IAS server in the next section.

Likewise, replace the IP addresses on lines 9 and 10 (the `vdpn group vpn-pptp-group client configuration` lines that pass out the DNS and WINS servers to VPN clients) with the IP addresses of your DNS and WINS servers, respectively.

That should do it. Save the configuration to the PIX and then move on to configuring IAS.

## Configuring Internet Authentication Service

Before doing anything else, create a new global security group in Active Directory. Call it something like "VPN Users" or similar. We'll use this group later as an additional security check in validating VPN connections.

Next, install IAS using the Add/Remove Programs icon in Control Panel. Once it has been installed, launch it from the Administrative Tools folder on the Start Menu and we'll proceed with configuring it for authenticating VPN connections to the PIX firewall.

First, we need to grant IAS permission to read dial-in properties from user accounts in Active Directory. To do this, right-click on the "Internet Authentication Service (Local)" and select "Register Server in Active Directory". Select Yes (or OK) if prompted to confirm.

With that done, we can now configure the PIX firewall as a RADIUS client. Right-click on RADIUS Clients and select New RADIUS Client. In the wizard, specify the IP address (or DNS name) of the PIX firewall's internal IP address and the shared secret. Note that this shared secret is the same secret key specified in the PIX configuration above. RADIUS clients use this to authenticate to RADIUS servers, so make it a reasonably strong password.

Now create a remote access policy. Right-click on Remote Access Policies and select New Remote Access Policy. In the wizard, specify a name, select to create a custom policy, and then add the following conditions to the policy:

* NAS-IP-Address: This will be the IP address of the PIX firewall's internal interface. This helps to ensure that this policy only applies to VPN requests from this firewall and not from any other RADIUS client.

* Windows-Groups: This should be the security group created earlier. Any user that should be allowed to authenticate on a VPN connection will need to be a member of this group.

The rest of the policy should be very straightforward. Make this policy the first policy (using the Move Up/Move Down commands in the IAS console), add a user to the group created earlier, and then test your connection. Remote systems attempting to connect via PPTP should now be able to authenticate the VPN connection using their Active Directory usernames and passwords.

Although this was written from the perspective of authenticating PPTP connections, the process should be very similar for IPSec VPN clients as well.
