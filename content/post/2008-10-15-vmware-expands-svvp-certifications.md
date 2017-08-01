---
author: slowe
categories: News
comments: true
date: 2008-10-15T09:16:38Z
slug: vmware-expands-svvp-certifications
tags:
- Citrix
- ESX
- Microsoft
- Virtualization
- VMware
title: VMware Expands SVVP Certifications
url: /2008/10/15/vmware-expands-svvp-certifications/
wordpress_id: 989
---

As I discussed a few weeks ago, VMware's [original SVVP certification was limited][1] to x86 guests with up to 4GB of RAM running on AMD Opteron-based systems. So, while VMware was the [first third-party hypervisor validated under SVVP][2], their validation was a bit limited.

Thanks to Mike DiPetrillo via Twitter, I learned that VMware's SVVP validation has now been expanded to include the following:

* Both AMD Opteron and Intel-based systems

* Both x64 and x86 guests

* Validation of x64 guests running up to 16GB of RAM

The full list is found [here](http://windowsservercatalog.com/item.aspx?idItem=fb304f90-92ed-4bed-ae4f-96805c16b61c&bCatID=1521).

Unfortunately, x86 guests are still limited to 4GB of RAM, but I don't really foresee that being a major problem as I suspect the vast majority of x86 guest workloads will have 4GB or less of RAM.

Let's hope that VMware continues to expand the SVVP certifications to include more processors (currently limited to 4 CPUs, which I'm guessing is 4 vCPUs) and more memory for both x86 and x64 workloads. Right now VMware's list of SVVP certifications is trounced by Citrix's list, which boasts higher CPU and higher RAM limits.

[1]: {{< relref "2008-09-23-vmware-esx-svvp-validation-apparently-limited.md" >}}
[2]: {{< relref "2008-09-03-vmware-esx-35-u2-validated-via-svvp.md" >}}
