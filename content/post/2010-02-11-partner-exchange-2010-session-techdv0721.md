---
author: slowe
categories: Liveblog
comments: true
date: 2010-02-11T18:10:56Z
slug: partner-exchange-2010-session-techdv0721
tags:
- Security
- VDI
- Virtualization
- VMware
- vSphere
title: Partner Exchange 2010 Session TECHDV0721
url: /2010/02/11/partner-exchange-2010-session-techdv0721/
wordpress_id: 1836
---

This is a two-hour session on VMware View security architecture and security benefits titled "VMware View Security Benefits, Architecture, and Best Practices".

So what is VMware's security strategy? First, start with core platform security. This encompasses all the various features and functions of the hypervisor like memory protection and isolation, kernel module protections, hypervisor attack surface, etc. Next, continue with operational security. This is about integrating VMware's products into your organization's existing operational security policies and includes things like the vSphere Security Hardening Guide that was recently released. Using security virtual appliances is another step that enables broad-based security for all VMs in the environment. Finally, VMware is striving for a "better than physical" model where virtual security is better than physical security. Consider VMsafe as an effort in this area.

The presenter next reviewed the VMware View infrastructure and all the various components that are included in this infrastructure. To ensure security, all of these various components need to be reviewed with an eye on security. For example, _componentizing_ the different parts of a View infrastructure---for example, separating access points, user data, applications, data, and operating system helps to secure each of these different pieces.

A further benefit of this separation is that it allows for the creation of a true "gold master" for VMs. Products like ThinApp and VMware View Composer helps to simplify this process and help maintain a true "gold master" image. This means that all the various security guidelines can be more easily incorporated into this master image, the master image can be patched more easily, configuration drift is reduced, and you can recover more easily and more quickly after an attack.

Using virtual desktops also allows organizations to more easily create "desktop security zones" that help isolate higher-risk PCs from lower-risk PCs, thus containing potential security risk to a limited subset of all desktops. This might also help with meeting compliance requirements (the presenter specifically mentioned PCI).

Thin clients are helpful in reducing complexity at the edge, which can (in some cases) help reduce the attack surface and limit the amount of work that IT organizations need to do to help secure the endpoints.

What about backing up data? Using View to centralize desktops allows organizations to more easily implementation full data backups for the various types of data that are being created within the virtual desktop environment.

The presenter next moves on to vSphere security. Because VMware View depends upon vCenter and ESX/ESXi, the security of View is dependent upon the security of vCenter and ESX/ESXi. This led into a discussion of the benefits of virtualization vs. the security impact of virtualization. The topics covered here include all the usual suspects: greater impact of misconfiguration or attack; loss of visibility in the network access layer; loss of separation between network admins and server admins; potential VM sprawl without consistent configurations and properly defined procedures; possible security problems resulting from VM mobility; and unauthorized access to VMs because of VM encapsulation (users copying a VM by copying the VM's files).

So how does one protect the virtual infrastructure? You use existing techniques such as hardening and lockdown; defense in depth; and authorization, authentication, and accounting.

The same goes for protecting virtual machines. Use anti-virus, IDP/IDS systems, firewalls, etc. VMsafe and the functionality enabled by VMsafe will be very helpful here.

Be sure to isolate the management interfaces using physically separate management networks or by using VLANs. You should also control access to the management network using ACLs, jump boxes, VPNs, or other access controls. Only authorized individuals should have access to the management network and "ordinary end-users" should absolutely not have access.

The separation of duties is also important. Use vCenter Server's built-in roles to enable the principle of least privilege to help enforce separation of duties. Third-party products like HyTrust might also be helpful.

The presenter argues that moving to a vNetwork Distributed Switch is a security benefit. One big plus is the mitigation of the risk associated with misconfiguration. In addition, there is support for private VLANs (PVLANs), inbound traffic shaping, Network VMotion, and (with the Nexus 1000V) ACLs and a natural separation of duties.

At this point the presenter moves on to a discussion of secure access to virtual desktops.

Authentication is one key area; View supports AD authentication as well as RSA SecurID. View Manager does not store any of the authentication information; this is all offloaded to Active Directory or the RSA Authentication Manager. Smart Card authentication is an alternative to standard username and password authentication. The certificate on the Smart Card contains a Subject Alternative Name (SAN); the SAN is matched against the User Principal Name (UPN) in Active Directory. Smart Card authentication is not supported with PCoIP.

View does support a form of single-sign on so that users log on to the View Client and is authenticated all the way down to the virtual desktop.

Future support with regard to authentication will include Kerberos realm authentication; UPN authentication; RADIUS support in the View Connection Server; and improved SSO to virtual desktops.

Moving on to access options, PCoIP requires direct access to the virtual desktop; it won't work with SSL tunneling. Fortunately, PCoIP is already encrypted (wirespeed encryption using AES 128-bit encryption). For non-PCoIP connections, HTTPS tunneling of RDP is supported by VMware View. This can greatly simplify firewall configuration (only TCP port 443 is required). Secure tunneling also has the benefit of helping to maintain sessions in the event of a dropped connection.

Some advantages of PCoIP is the built-in encryption and support for blocking USB Plug events (to control USB device usage).

The View Security Server enables you to create a DMZ infrastructure that prevents end points from having direct access to virtual desktops or the Connection Server. The use of load balancers is supported with both Security Servers and Connection Servers.

VMware does recommend replacing the self-signed certificates that are supplied with VMware View with valid SSL certificates. Note that the specific SSLv3/TLSv1 ciphers that are used with secure connections can be configured to enable or disable specific ciphers.

The use of a VPN can also help provide a single point of entry and simply the firewall configuration.

The next topic is VMware View's entitlements model. View uses Microsoft ADAM on Windows Server 2003 or Microsoft AD LDS on Windows Server 2008. Back-end Active Directory is still leveraged for authentication. View uses the idea of foreign security principals (FSPs), which means that Active Directory doesn't have to be synchronized with the local LDAP instance. In addition, user authorizations and entitlements don't have to be stored in Active Directory (which would require schema extensions).

At this point the presenter moves into a discussion of View security best practices:

* Harden the base OS within the virtual desktops and enforce refresh intervals and OS patching.

* Choose the proper authentication model and use a Security Server or VPN for secure remote access.

* Be sure to understand the firewall requirements and configure the firewall accordingly.

* Be sure to harden the Connection Server and the underlying Windows Server OS upon which it is installed.

* Replace the default self-signed certificates.

* Set appropriate entitlements within the Connection Server. Zone users according to use case and risk.

* Avoid direct remote access to virtual desktops where possible. Don't allow users to connect without going through the Connection Server.

* Control USB access, redirection of clipboard, printers, and drives.

* Leverage Active Directory Group Policy to help with virtual desktop OS lockdown and some View-specific settings. (You might need to use Loopback Policy Processing in this instance.)

* Know the different ports and the directions that are required when configuring firewalls. Refer to the View Architecture Planning Guide for full details.

* Install anti-virus, but use a minimal installation to reduce bloat.

* Use a staggered or randomized scanning policy to avoid overwhelming the infrastructure. Use policies or corporate configuration tools to enforce staggered scanning and signature updates and to configure exclusion lists (only need to scan the user data disk; the base OS is locked down through the use of linked clones).

* Consider a VMsafe Ready AV product.

* Include Network Access Control (NAC) management agent in the parent VM prior to cloning.

* Use ThinApp to gain some security benefits (prevents the OS from getting infected through the actions of a ThinApped application). Consider using ThinApp for browsers.

* Specific to ThinApp and anti-virus, don't install AV on the Capture/Build system if at all possible. If AV is installed, no on-demand scanning of the ThinApp project directory.

The next topic of the session was a discussion of using VMware vShield Zones. vShield Zones provide virtual firewalls that operate as transparent Layer 2 bridges and allow you to create different security zones. This can provide some technological enforcement of zones for different user environments (different pools for web browsing vs. internal CRM access and these pools cannot communicate with each other because of vShield Zones).

The presenter wrapped up the session with an overview of VMsafe and how VMsafe can help contribute to the security of a VMware View environment. VMsafe enables greater protection of VMs through APIs that allow deepened inspection of CPU/memory, networking, and storage. For example, VMsafe allows knowledge of specific CPU state or inspection of specific memory pages. VMsafe allows networking traffic to be inspected, intercepted, modified, or even replicated (consider vShield Zones integrated with the VMsafe APIs). With regard to storage, VMsafe allows the ability to mount VMDKs, inspect storage I/Os, and do so transparently and inline to the storage stack.

The session wrapped up with a list VMsafe-integrated solutions from companies like Altor Networks, TrendMicro, McAfee, and Checkpoint.
