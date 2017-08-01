---
author: slowe
categories: Tutorial
comments: true
date: 2006-11-21T17:03:44Z
slug: greater-ad-integration-via-nfs-and-automounts
tags:
- ActiveDirectory
- CentOS
- Interoperability
- Kerberos
- LDAP
- Linux
- NFS
- Solaris
- SSH
- UNIX
title: Greater AD Integration via NFS and Automounts
url: /2006/11/21/greater-ad-integration-via-nfs-and-automounts/
wordpress_id: 367
---

This solution builds upon the integration of Solaris 10 and Linux (again, CentOS specifically, but I'll use Linux here instead of mentioning the specific distribution) into Active Directory for authentication and authorization as outlined in these related articles:

[Refined Solaris 10-AD Integration Instructions][1]  
[Linux, Active Directory, and Windows Server 2003 R2 Revisited][2]

In these articles, I describe a configuration whereby you can use Kerberos against Active Directory for authentication, and LDAP against Active Directory for user and group lookups. This allows you to centralize the user account information (username, UID number, home directory, login shell, etc.) into Active Directory so that when an account is provisioned in AD it is also available to associated UNIX and Linux systems (as applicable). Using the information in this article, you can extend that integration to auto-mounted shared home directories between Solaris and Linux.

I'll accomplish this by using the UNIX interoperability tools included in [Windows Server 2003 R2](http://www.microsoft.com/windowsserver/default.mspx); namely, the Server for NFS, Server for NIS, and the User Name Mapping Server. On the Solaris and Linux side, I'll use autofs.

OK, let's dive right in.

### Configuring Solaris

Configuring Solaris is pretty straightforward; it's pretty much designed out of the box to use auto-mounted home directories. This is one area where Solaris shows its enterprise pedigree (that's not a knock against Linux, just an observation about Solaris).

The only real change that needs to be made is to the `/etc/auto_home` file, which specifies the auto-mounted home directory mappings. The easiest thing to do is use a wildcard mapping, like this:

    *   nfssrvr.example.com:/home/&

Here, the asterisk (*) and the ampersand (&) will be replaced with the appropriate information. So, if a user name "bjones" logs on, then the the `/home/bjones` NFS share on the server nfssrvr.example.com would be mounted on `/home/bjones` on the Solaris server. Useful, eh?

Note that autofs _may_ be disabled in your installation of Solaris; it wasn't in mine, but I've seen references elsewhere that this is not typical. Use these commands to check the status and enable autofs, if necessary:

    svcs -a | grep autofs  
    svcadm -v enable svc:/system/filesystem/autofs

At this point, Solaris should be ready to go. Let's look at the Linux configuration.

### Configuring Linux

The Linux configuration is only slightly more complicated than the Solaris configuration. Whereas Solaris uses `/etc/auto_master` to define the automounted filesystems, autofs on Linux uses `/etc/auto.master`, and it does not (by default, as far as I can tell) define a map for automounted home directories.

So we must first edit `/etc/auto.master` and add the following line:

    /home   /etc/auto.home

This tells autofs to look in the file `/etc/auto.home` to find the automounted directories under `/home`. Then we create the `/etc/auto.home` file with the following line:

    *   nfssrvr.example.com:/home/&

(Just like the Solaris box in this case.) Finally, we can check for the autofs startup status, fix it, and start autofs with the following commands, respectively:

    chkconfig --list autofs  
    chkconfig autofs on  
    service autofs start

In fact, you will need to restart the autofs daemon (with `service autofs restart`) in order for the changes to `/etc/auto.master` and `/etc/auto.home` to be recognized. (The same goes for Solaris, too, only using the `svcadm -v restart svc:/system/filesystem/autofs` command instead.)

To further streamline any differences between Solaris and Linux, I also created a `/export/home` on the Linux server, in the event I wanted/needed local home directories as well. Please note that I would need to modify the `/etc/auto.home` (on Linux) or `/etc/auto_home` (on Solaris) as well if this were the case.

Now that Linux and Solaris are both done, let's have a look at configuring the UNIX interoperability components on Windows.

### Configuring Windows (NFS, NIS, and User Name Mapping)

Surprisingly enough, this part was the hardest part for me. I don't know if it was the documentation (if it can be called that) or something else, but I had a hard time with this. Hopefully the information I share here can help someone else avoid this headache.

#### Configuring the Domain Controllers

On the domain controllers in your Active Directory domain, install the following components:

* Server for NIS and Administration Components (under Active Directory Services > Identity Management for UNIX)

* Microsoft Services for NFS Administration, RPC External Data Representation, RPC Port Mapper, Server for NFS Authentication, and User Name Mapping (under Other Network File and Print Services > Microsoft Services for NFS)

It may be possible to get by with less than that installed, but I couldn't get it working until all of those are installed. If you've followed my instructions for AD integration, the Server for NIS and Administration Components are probably already installed, as those expose the "UNIX Attributes" tab in Active Directory Users & Computers.

Once these are installed, use these steps to configure them appropriately:

1. Using the Services MMC, make sure that Server for NIS is running.

2. In the Microsoft Services for NFS MMC, right-click on "Microsoft Services for NFS" and select Properties. Specify the name of the DC as the "User Name Mapping Server", check the box labeled "Active Directory Lookup", and specify the name of the Active Directory domain. Click OK to save those changes and close the dialog box.

3. Still in Microsoft Services for NFS MMC console, right-click User Name Mapping and select Properties. Go to the "Simple Mapping" tab, check the box marked "Use simple maps", and then add one of the DCs from the domain that's running Server for NIS at the bottom of the dialog box.

4. Right-click on both User Maps and Group Maps and select "Show simple maps," then click Refresh. You should see a list of the users in Active Directory that have been UNIX-enabled, i.e., have UNIX attributes specified for their account. At this point, you can close the Microsoft Services for NFS MMC.

The DC should now be ready to go. It's time to move to the file server.

#### Configuring the File Server

We need to install the Server for NFS component on the file server that will host our automounted home directories. The Server for NFS is found under "Other Network File and Print Services" in Add Windows Components.

Once that has been installed, repeat step #2 above (configuring the User Name Mapping Server and Active Directory lookup) on the file server.

We're now ready to create some NFS shares. Right-click the folder you'd like to share via NFS (I created a folder named "home"---I know, not very creative) and shared that via NFS (there's an NFS Sharing tab when you open the properties for the folder). When specifying the share name, be wary of the case since UNIX/Linux are cAsE sEnSiTiVe. (It's probably best to stick to all lowercase.) If you want to allow anonymous access and/or root access, configure it appropriately at this point.

Word of warning: be sure to include UNIX-enabled groups and/or users in the NTFS access control list for the folder and containing files! This one threw me for a loop. At first, I didn't have any UNIX-enabled groups in the ACL and I kept getting "insufficient permissions" errors when trying to mount the share via NFS. It wasn't until I ensured that at least one UNIX-enabled group had read permission to the folder that things started working as expected.

To test your NFS share, use this command from Solaris or Linux, respectively (the first command is for Solaris, the second is for Linux):

	mount -F nfs nfssrvr.example.com:/home/nfs /mnt/nfs  
    mount -t nfs nfssrvr.example.com:/home/nfs /mnt/nfs

If this mounts the NFS share without any problems, then you should be ready to roll.

(Note: In case you hadn't already picked this up, one advantage of using a Windows NFS server is that we can also share the same folder via CIFS/SMB and create unified home directories for Solaris, Linux, and Windows.)

Now, upon logging in to either Solaris or Linux (I tested this via SSH), you should get dropped into `/home/<username>` (assuming that's where you configured the automounter to mount the NFS share) automatically.

### How I Tested

I tested this procedure using Solaris 10 6/06 and CentOS 4.3. These systems were configured for Active Directory integration and native Kerberos logons via SSH. I used Windows Server 2003 R2 Standard Edition for the NFS server, and Windows Server 2003 R2 Standard Edition for the Active Directory domain controllers. All of these systems were created as virtual machines hosted on a [VMware Virtual Infrastructure 3](http://www.vmware.com/products/vi/) server farm with servers running ESX Server 3.0.1.

[1]: {{< relref "2006-10-16-refined-solaris-10-ad-integration-instructions.md" >}}
[2]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
