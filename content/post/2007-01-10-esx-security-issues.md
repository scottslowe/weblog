---
author: slowe
categories: News
comments: true
date: 2007-01-10T11:29:56Z
slug: esx-security-issues
tags:
- ESX
- Security
- Virtualization
- VMware
title: ESX Security Issues
url: /2007/01/10/esx-security-issues/
wordpress_id: 399
---

Some security vulnerabilities in VMware [ESX Server](http://www.vmware.com/products/vi/esx/) have been disclosed in the last few days. Secunia released this [advisory on multiple vulnerabilities](http://secunia.com/advisories/23680/); the related vulnerabilities include flaws in the bundled versions of [OpenSSH](http://www.openssh.org/), [OpenSSL](http://www.openssl.org/), and Python that come with the service console (which, as you may already know, is a modified form of Red Hat Enterprise Linux).

A patch to address these vulnerabilities is available for the affected versions of ESX from the [VMware](http://www.vmware.com/) web site; the links for the ESX 3.0.0 and ESX 3.0.1 patches are below.

Patch for ESX 3.0.0:  

[http://www.vmware.com/support/vi3/doc/esx-3069097-patch.html](http://www.vmware.com/support/vi3/doc/esx-3069097-patch.html)

Patch for ESX 3.0.1:  

[http://www.vmware.com/support/vi3/doc/esx-9986131-patch.html](http://www.vmware.com/support/vi3/doc/esx-9986131-patch.html)

One of the vulnerabilities mentioned in the Secunia advisory above pertains to incorrect SSL key permissions; more information on that issue can be found in [this VMware KB document](http://kb.vmware.com/vmtnkb/search.do?cmd=displayKC&docType=kc&externalId=2467205&sliceId=SAL_Public). This issue also affects some of VMware's hosted products, such as [VMware Server](http://www.vmware.com/products/server/), [VMware Player](http://www.vmware.com/products/player/), and [VMware Workstation](http://www.vmware.com/products/ws/).

In addition, a possible cross-site scripting exploit has been uncovered in Apache, which is used by ESX Server. VMware provides [more information on the possible exploit](http://kb.vmware.com/KanisaPlatform/Publishing/466/5915871_f.SAL_Public.html) on their web site. In addition, more information is available on the [CVE candidate](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2006-3918) entry.
