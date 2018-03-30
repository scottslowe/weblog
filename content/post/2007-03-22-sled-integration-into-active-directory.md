---
author: slowe
categories: Tutorial
comments: true
date: 2007-03-22T18:53:28Z
slug: sled-integration-into-active-directory
tags:
- ActiveDirectory
- Interoperability
- Kerberos
- LDAP
- Linux
- Microsoft
- Samba
- Windows
title: SLED Integration into Active Directory
url: /2007/03/22/sled-integration-into-active-directory/
wordpress_id: 432
---

The information below comes from site reader Shannon VanWagner, who had the time to research this and come back with the information necessary to integrate SuSE Linux Enterprise Desktop (SLED) 10 into Active Directory (on [Windows Server 2003 R2](http://www.microsoft.com/windowsserver/default.mspx)) using Kerberos, LDAP, and Samba. I haven't tested these instructions (no copy of SLED with which to test), but I imagine that they should be fine. It's really just the SLED-specific tweaks to the existing AD integration instructions, anyway.

## Preparing Active Directory (One-Time)

### Enabling Editing/Display of UNIX Attributes

On your Windows Server 2003 R2 Domain Controller, enable "Identity Management for UNIX" via Add/Remove Programs > Add Windows Components > Active Directory Services > Identity Management for UNIX. Note that this will require a reboot and does require Schema Admin privileges. This will add a UNIX Properties tab to each user account in AD Users and Computers that will allow you to control the user UID, primary group GID, NIS Server setting, and user shell setting (e.g. `/bin/bash`).

(As an aside, I'll refer readers back to [my earlier article][1] on various ways of making the "UNIX Attributes" tab appear.)

### Create an LDAP Bind Account

You'll also need to create an account in Active Directory that will be used to bind to Active Directory for LDAP queries. This account does _not_ need any special privileges; in fact, making the account a member of Domain Guests and _not_ a member of Domain Users is perfectly fine. This helps minimize any potential security risks as a result of this account.

## Prepare Active Directory (Each User)

Each Active Directory account that will authenticate via Linux must be configured with a UID and other UNIX attributes. This is accomplished via the new "UNIX Attributes" tab on the properties dialog box of a user account. (Installing the "Server for NIS" component enables this, as mentioned previously.) Be sure to set login shell, home directory, UID, and primary UNIX group ID.

After all the user accounts have been configured, then we are ready to configure the Linux server(s) for authentication against Active Directory.

## Configure the SLED Workstation

On the SLED 10 client setup your config files as illustrated in the following examples. See the file comment headers for the file names and locations (replace items such as "domain.com" with settings specific to your environment).

We'll start first with the `/etc/hosts` file:

``` text
127.0.0.1 localhost
10.10.10.1 WINDOWS-DC-HOSTNAME.DOMAIN.COM WINDOWS-DC-HOSTNAME
# special IPv6 addresses
::1 localhost ipv6-localhost ipv6-loopback

fe00::0 ipv6-localnet

ff00::0 ipv6-mcastprefix
ff02::1 ipv6-allnodes
ff02::2 ipv6-allrouters
ff02::3 ipv6-allhosts
127.0.0.2 client-hostname.DOMAIN.COM client-hostname
```

Next, we configure the `krb5.conf` file. In the interest of brevity, I won't paste it into this article; rather, just follow [this link](https://gist.github.com/scottslowe/3eb79dec00dc8e754b6c) to view it on GitHub (and have the option to download it).

Once Kerberos is configured, we configure LDAP via `ldap.conf`. Again, just follow [this link](https://gist.github.com/scottslowe/29aed4672a012946f1cb) to view the full file.

And then configure the Name Switch Service to use LDAP. [This link](https://gist.github.com/scottslowe/55f2ddd14214499a629c) will show you a properly-formatted NSS configuration.

Almost there---next we need to make sure that time synchronization is working, since this is a prerequisite for Kerberos authentication. To make sure time synchronization is working, we'll configure NTP using a configuration similar to [this configuration](https://gist.github.com/scottslowe/1375d23864c8ad41498a).

At this point we have Kerberos authentication configured, LDAP configured, NSS configured to use LDAP, and time synchronization configured and running. Now we need to get Samba configured to help automate the process of integrating into Active Directory. A sample configuration such as [this one](https://gist.github.com/scottslowe/790ffff17a56e2d21189) would get the job done.

SLED uses PAM (Pluggable Authentication Mechanism) to control authentication and authorization, so we next need to configure PAM to use Kerberos and LDAP, where necessary. There are a number of files that need to be configured to make this happen; [this GitHub gist](https://gist.github.com/scottslowe/2f3bb6cd609cc2926178) shows the various files and how each would need to be configured.

All these files are in turn referenced by a "master" PAM configuration file, like this:

``` text
#%PAM-1.0
###########line above is part of this file#################
#/etc/pam.d/su config file
###########################################################
#auth sufficient pam_rootok.so
auth include common-auth
account include common-account
password include common-password
session include common-session
session optional pam_xauth.so
```

Once all these configuration files are in place, we can proceed with the following steps:

1. Run `getent passwd` (you should only see SLED 10 local users in this listing).

2. Run `kdestroy` to destroy any cached Kerberos tickets you may currently have.

3. Run `kinit domain-admin-user@DOMAIN.COM` to create a new Kerberos ticket for the machine with Domain Admin credentials; you can then run `klist` to verify the Kerberos ticket.

4. Run `net ads join -U domain-admin-user@DOMAIN.COM` to join the machine to the domain using the Kerberos ticket of the domain administrative user.

5. Restart the smb and winbind services/daemons using `/etc/init.d/<service name> stop` followed by `/etc/init.d/<service name> start`.

6. Run `getent passwd`; the output should now list domain users and their associated UIDs.  Likewise, `getent group` should output domain groups and GIDs.

7. The `wbinfo -u` and `wbinfo -g` commands should list domain users and domain groups, respectively.

8. Finally, run `su <domain-username>`. It should prompt you for the users password, create a home directory for that user if necessary, and then switch you to the user.

We're not really sure if it's necessary, but you can add the LDAP bind account (used to bind to LDAP for queries) to the list of SMB users with the `smbpasswd -w` command. It may prove over time that this command isn't necessary. (Anyone want to double-check this for us?)

Finally, using YaST (System > RunLevel Editor), enable the NTP, SMB, and Winbind daemons (I'm fairly sure that Winbind isn't necessary). Also, disable the nscd daemon to avoid caching problems and unwanted interaction with Winbind.

At this point, if you were successful in using `su` to switch to a Windows user, you should be able to reboot the machine and login to the machine as a Windows user (be sure to use an account that has UNIX attributes assigned in Active Directory).

NOTE: If you happen to get yourself locked out of the system, it will be likely an `/etc/nsswitch.conf` file problem. Simply boot to the SLED 10 installation disc using the "Recover System" option, then issue these commands to change the `/etc/nsswitch.conf` file:

    mount -w /dev/hda1 /mnt  
    vi /mnt/etc/nsswitch.conf

Replace "/dev/hda1" with the correct notation for your system partition, and when editing `/etc/nsswitch.conf` remove "ldap" from the passwd, group, and shadow lines; only "files" or "compat" should remain. Once that's done, you can now reboot and login as root so you can troubleshoot the problem.

Some additional resources and information:  

[http://forums.suselinuxsupport.de/index.php?showtopic=53004](http://forums.suselinuxsupport.de/index.php?showtopic=53004)  

[http://forums.fedoraforum.org/archive/index.php/t-29825.html](http://forums.fedoraforum.org/archive/index.php/t-29825.html)

Thanks again to Shannon VanWagner for sharing this information with me so that I could post it here. As always, feel free to post any questions, clarifications, or corrections in the comments below.


[1]: {{< relref "2006-11-28-unix-attributes-tab-and-nispropdll.md" >}}
