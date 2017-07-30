---
author: slowe
categories: Tutorial
comments: true
date: 2006-09-08T13:59:59Z
slug: ldap-based-access-control
tags:
- ActiveDirectory
- CentOS
- Interoperability
- Kerberos
- LDAP
- Linux
- Security
title: LDAP-Based Access Control
url: /2006/09/08/ldap-based-access-control/
wordpress_id: 331
---

First off, let's set some expectations. First, I performed this testing using [CentOS](http://www.centos.org/) (version 4.3) and [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/default.mspx), both running as VMs on [VMware ESX Server](http://www.vmware.com/products/vi/esx/) (version 3.0.0). Active Directory and the CentOS server were configured for Kerberos authentication as described in my Linux-AD integration instructions. Second, I used [OpenSSH](http://www.openssh.org/) (version 4.2p1 on the client, version 3.9p1 on the server) as the vehicle for testing host access. If you are using telnet, HTTP, or something else, the configuration will look _very_ different. Third, and finally, I can't guarantee you that this procedure will work flawlessly in your environment. It should, however, get you well down the road to completion.

OK, now you're ready to get rolling. Note: If you've configured your SSH server for native Kerberos authentication, the changes we're going to make below are going to break that. Sorry, I couldn't find any way around that.

### Configure OpenSSH

Edit your `/etc/ssh/sshd_config` file and make sure that the following line is present:

    UsePAM yes

(This is the piece that breaks native Kerberos authentication, by the way.) SSHd will need to re-read the configuration after making this change in order for the change to take effect; restarting sshd is an easy way to do that.

### Configure PAM

Because PAM configurations vary from client to client, I can't give you the exact configuration and placement that you need. The CentOS configuration, which would be virtually identical to Red Hat/Fedora, is described below. Please be sure to adjust as needed for your particular Linux distribution (or flavor of Unix).

Add the following line to the top (first line) of the "account" section of the `/etc/pam.d/system-auth` file:

    account   sufficient   /lib/security/$ISA/pam_ldap.so

You should be able to leave the rest of the file intact.

### Configure LDAP

(Just as quick aside, you should be aware by now that modifying `ldap.conf` really only affects the pam\_ldap and nss\_ldap modules, not OpenLDAP itself. OpenLDAP is typically configured elsewhere.)

Make the following changes to the `/etc/ldap.conf` file:

    pam_groupdn cn=GroupName,ou=OUName,dc=example,dc=net
    pam_member_attribute member

These lines may already be present, and you may only have to uncomment them and substitue the correct value for the pam\_groupdn parameter. Do _NOT_ copy and paste the pam\_groupdn line as shown above; they won't work! (The pam\_member\_attribute line is fine to copy and paste.) Just put in the correct DN for the group that you'd like to use to control access to that particular host. Members of that group will be allowed access; others will be denied.

If the group name includes spaces, enclose the entire DN in double quotes (i.e, pam\_groupdn "cn=Group Name,cn=Users,dc=example,dc=net").

### Configure Active Directory

This part should be obvious, but be sure to create the appropriate group (making sure that the DN of the group matches what you specified in `/etc/ldap.conf` on the Linux/Unix server). Add user accounts that should be allowed access to the Linux/Unix host in question to this group (you may find it necessary or desirable to create multiple groups so that you can have fine-grained control over which hosts may be accessed by which users). The testing I performed indicated that the group did not need to be Unix-enabled in AD; your mileage may vary.

### Verify and Test

To verify and test the configuration, add a user account to the appropriate group and then try to login to the Linux/Unix server via SSH. Everything should work just fine. Take the user out of the group and try again; the login should fail.

I'm open to comments, suggestions for improvement, and corrections to this procedure. Also, if any of you out there have had the opportunity to try this on different Linux distributions or other Unix-based operating systems and want to share configs, please do so in the comments.
