---
author: slowe
categories: Tutorial
comments: true
date: 2005-12-06T17:38:24Z
slug: watchguard-firebox-vpn-and-active-directory-integration
tags:
- ActiveDirectory
- Encryption
- Security
- Windows
- Interoperability
title: WatchGuard Firebox VPN and Active Directory Integration
url: /2005/12/06/watchguard-firebox-vpn-and-active-directory-integration/
wordpress_id: 136
---

A short while back, I posted an article on [Cisco PIX VPN and Active Directory integration][1]. Now, I'd like to follow that article up with a version looking at integration between Active Directory and [WatchGuard][2] Firebox VPNs.

As with the PIX-AD integration document, this article assumes that you have some basic knowledge of how to work with the WatchGuard Firebox series of firewalls. This article was written using version 6.2 of the WatchGuard Firebox System software and Windows Server 2003; other versions of either the firewall software or Windows should be similar.

### Configuring the Firebox

First, we'll need to setup the Firebox. Use the Firebox software (Policy Manager, specifically) to perform the following configuration tasks:

* Add the server that is running IAS (or will be running IAS; see below) as a RADIUS server in the Authentication Servers dialog box (found on the Setup menu). Here, you'll need to specify the server's IP address, port number (the default of 1645 will be fine), and the shared secret.

* Instruct the firewall to use RADIUS by going to Setup > Firewall Authentication and selecting "RADIUS" as the authentication type.

* Configure the firewall's Remote User VPN (on the Network menu) to use RADIUS by checking the "Use RADIUS to authenticate remote users" check box.

Once this is done, proceed with configuring PPTP-based remote user VPNs as usual. Be sure to add a rule allowing traffic to/from the pptp_users group; otherwise, VPN users will be subject to the same traffic restrictions as Internet users.

### Configuring Internet Authentication Service

Before doing anything else, create a new global security group in Active Directory. Call it "pptp_users", just like the name of the group on the Firebox. This is an important part of the glue that will bind the Firebox together with Active Directory.

If IAS is not already installed, install IAS using the Add/Remove Programs icon in Control Panel.

Once it has been installed, launch it from the Administrative Tools folder on the Start Menu and we'll proceed with configuring it for authenticating VPN connections to the Firebox.

First, we need to grant IAS permission to read dial-in properties from user accounts in Active Directory. To do this, right-click on the "Internet Authentication Service (Local)" and select "Register Server in Active Directory". Select Yes (or OK) if prompted to confirm. Note that if IAS was already installed, it may have already been registered with Active Directory as well.

With that done, we can now configure the Firebox as a RADIUS client. Right-click on RADIUS Clients and select New RADIUS Client. In the wizard, specify the IP address (or DNS name) of the Firebox's trusted interface and the shared secret. Note that this shared secret is the same secret key specified when configuring the RADIUS server in the Firebox previously. RADIUS clients use this to authenticate to RADIUS servers, so make it a reasonably strong password.

Now create a new remote access policy. Right-click on Remote Access Policies and select New Remote Access Policy. In the wizard, specify a name, select to create a custom policy, and then add the following conditions to the policy:

* NAS-IP-Address: This will be the IP address of the Firebox's trusted interface. This helps to ensure that this policy only applies to VPN requests from this firewall and not from any other RADIUS client.

* Windows-Groups: This should be the "pptp_users" security group created earlier. Any user that should be allowed to authenticate on a VPN connection will need to be a member of this group.

Of course, this policy should grant access. On the next screen, select "Edit Profile" to edit the remote access profile. This is important because we'll need to verify that the RADIUS server is passing the correct information to the Firebox.

On the Advanced tab, remove all the attributes listed there (Service-Type and Framed-Protocol are there by default) and then add the Filter-Id attribute. To this attribute, add the string value "pptp_users". Click OK to save these changes to the profile and then finish creating the policy.

Make this policy the first policy (using the Move Up/Move Down commands in the IAS console), add a user to the group created earlier, and then test your connection. Remote systems attempting to connect via PPTP should now be able to authenticate the VPN connection using their Active Directory usernames and passwords.

**UPDATE:** I've updated this entry to correct some errors pointed out in the comments. Thanks for the feedback!

[1]: {{< relref "2005-11-22-cisco-pix-vpn-and-active-directory-integration.md" >}}
[2]: http://www.watchguard.com/
