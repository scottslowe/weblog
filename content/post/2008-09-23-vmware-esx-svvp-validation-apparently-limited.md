---
author: slowe
categories: Musing
comments: true
date: 2008-09-23T22:21:56Z
slug: vmware-esx-svvp-validation-apparently-limited
tags:
- ESX
- Microsoft
- Virtualization
- VMware
title: VMware ESX SVVP Validation Apparently Limited
url: /2008/09/23/vmware-esx-svvp-validation-apparently-limited/
wordpress_id: 949
---

A few weeks ago I blogged about [SVVP validation][1] of VMware ESX 3.5 Update 2. Just last week, though, I learned that Microsoft's validation may be quite limited. According to [this article](http://blogs.lemagit.fr/2008/09/18/vmworld-2008-the-ugly-secrets-of-microsoft-svvp/), the validation is handled per CPU architecture, per memory configuration. So, apparently VMware ESX is validated on an AMD system with 4GB of RAM, but that's about it.

Does anyone have any information to back this up? Or any information on additional CPU architectures and memory configurations that are currently being tested for validation?

[1]: {{< relref "2008-09-03-vmware-esx-35-u2-validated-via-svvp.md" >}}
