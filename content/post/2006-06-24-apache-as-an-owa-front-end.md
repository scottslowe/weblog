---
author: slowe
categories: Explanation
comments: false
date: 2006-06-24T11:10:29Z
slug: apache-as-an-owa-front-end
tags:
- Apache
- Exchange
- Interoperability
- SSL
title: Apache as an OWA Front-End
url: /2006/06/24/apache-as-an-owa-front-end/
wordpress_id: 277
---

A while ago, I discussed the [use of Apache to protect OWA][1] from web-based attacks. This configuration placed an Apache HTTP server in front of a server running Microsoft [Exchange Server 2003](http://www.microsoft.com/exchange/default.mspx) to protect it against web-based attacks, offload SSL encryption, and enable name-based virtual hosts (for the conservation of public DNS hostnames, especially important for smaller organizations). While this is a useful configuration, it is not without its drawbacks.

First, let's review some of the advantages of this type of configuration:

* You can use the open source mod\_security module to protect OWA against virtually all forms of URL-based attacks. Mod\_security is an extremely powerful and useful module that can greatly increase the protection against web-based attacks. See the [mod_security web site](http://www.modsecurity.org/) for more information.

* Even without mod_security, deploying Apache in front of OWA can protect the OWA server against many IIS-specific attacks.

* This configuration can be used in addition to IIS-specific protection such as URLScan.

* You can terminate the SSL connection at the Apache server instead of on the OWA server, freeing up CPU resources on the OWA server for other tasks (this is especially useful in smaller Exchange deployments where the OWA server may also be a mailbox server).

Now, for some of the disadvantages of this type of configuration:

* Apache lacks the intelligence of an Exchange server configured as a true front-end, and therefore cannot direct requests to multiple back-end mailbox servers. In this type of configuration, the Apache reverse proxy always directs requests to the same OWA server and cannot determine which mailbox server the user is homed on.

* Organizations with expertise in Microsoft products won't necessarily see any real benefit from this due to the added overhead and learning curve of supporting Linux and Apache. (Don't snicker, this is a real concern for organizations.)

I'm sure there are other disadvantages as well. Anyone care to comment and share their experiences?

[1]: {{< relref "2005-12-03-protecting-owa-with-apache.md" >}}
