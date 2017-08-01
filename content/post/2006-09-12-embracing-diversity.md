---
author: slowe
categories: Musing
comments: true
date: 2006-09-12T19:35:13Z
slug: embracing-diversity
tags:
- CentOS
- ESX
- Linux
- Macintosh
- Solaris
- UNIX
- Windows
- Interoperability
title: Embracing Diversity
url: /2006/09/12/embracing-diversity/
wordpress_id: 334
---

Hopefully it's no secret that I enjoy working with a variety of operating systems---VMware [ESX Server](http://www.vmware.com/products/vi/esx/) (OK, so maybe not "technically" a full operating system, but I'll include it anyway), Windows, [Mac OS X](http://www.apple.com/macosx/), various flavors of Linux (mostly [CentOS](http://www.centos.org/) and [Ubuntu](http://www.ubuntu.com/)), [OpenBSD](http://www.openbsd.org/), and (more recently) [Solaris](http://www.sun.com/software/solaris/) on x86 (haven't had the opportunity to do Solaris on Sparc yet). All this while I work full-time as a primarily "Microsoft engineer" specializing in Active Directory. Yet I find value in the diversity of my knowledge.

For example, a few months ago my experience in Unix/Linux-AD integration helped me to resolve an issue for a customer that was experiencing [problems with Vintela Authentication Services][1]. Even though I wasn't familiar with VAS _per se_, my knowledge and experience of PAM helped the customer find a workaround to a crippling performance issue until the vendor could identify the root cause of the problem. In addition, I was able to take that information and apply it back to open source Unix/Linux-AD integration projects to help improve performance and scalability.

Don't get me wrong; I'm not trying to brag or be boastful. It's just that someone who wouldn't take the time to become familiar with these other platforms wouldn't have been able to bridge the gap and make the connection. If for no other reason, you owe yourself as a technical professional to broaden your knowledge, extend your reach, and stretch your mind. Too many IT professionals get pigeonholed as "the Cisco guy" or "the Microsoft guy" or "the Unix guy". I don't want to be labeled that way (although often I am anyway), and somehow I don't think you do, either.

Let me share the example I experienced today. I'm working on a project where I needed to migrate about 850 security groups from one Active Directory forest to a new Active Directory forest, and I needed to rename the groups in the process. The particular migration tool we're using didn't offer that functionality, so I turned to other mechanisms. In the end, I was able to use the Directory Service command-line tools (`dsquery group`, specifically) and a Win32 version of `sed` (the stream editor) to manipulate a text file containing all the group names, then imported that back into Active Directory using LDIFDE. Using only "Windows" tools or only "Unix" tools the job wouldn't have been as easy as it was, but combining the tools---bridging the gap between the "OS wars"---allowed me to create a solution that matched the need at hand. Isn't that what it should be about, anyway?

&lt;aside&gt;If you haven't yet experienced the power of regular expressions...you need to take a look.&lt;/aside&gt;

Likewise, adding Windows and Active Directory to a heterogeneous network comprised of Windows and Unix/Linux/Unix-like operating systems brings together two very different sets of technologies, yet together they can easily reap the benefits of a centralized directory service. (Yes, yes, I know---you could do the same with an open source LDAP implementation. But would that open source LDAP implementation support the Windows clients that will inevitably be required on your network?)

If you're an IT professional (and it's likely you are if you are reading this), go expand your horizons. Learn something new that isn't necessarily related to your core expertise, and see if you can't also start to bridge the gaps and make some connections that will help you be more effective in your career.

[1]: {{< relref "2006-07-28-active-directory-and-vas.md" >}}
