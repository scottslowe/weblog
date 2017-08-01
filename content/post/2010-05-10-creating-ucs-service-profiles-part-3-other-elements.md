---
author: slowe
categories: Tutorial
comments: true
date: 2010-05-10T13:52:38Z
slug: creating-ucs-service-profiles-part-3-other-elements
tags:
- Cisco
- Hardware
- UCS
- Virtualization
title: 'Creating UCS Service Profiles Part 3: Other Elements'
url: /2010/05/10/creating-ucs-service-profiles-part-3-other-elements/
wordpress_id: 1926
---

This is part 3 of a series of posts on the various elements involved in creating UCS service profiles. In [part 1][1], I discussed the networking-related elements. In [part 2][2], I covered the storage-related elements. In this post, I'll discuss all other elements. The next and final post will bring all the elements together and create a service profile or service profile template.

In addition to the networking and storage elements I've discussed thus far, there are four more elements left to define before you create your service profile or service profile template:

1. At least one UUID suffix pool

2. An IPMI profile

3. A serial over LAN policy

4. A boot policy

Instructions and more information on each of these items is found in the following sections.

## Creating a UUID Suffix Pool

_Location in UCSM: Servers tab  Servers > Pools > UUID Suffix Pools_  
_Prerequisites: None_

Here are the steps to create a UUID suffix pool:

1. Right-click on UUID Suffix Pools and select Create UUID Suffix Pool.

2. Supply a name and a description for the UUID suffix pool.

3. Normally the Prefix should be set to Derived; this means that UCSM will automatically generate the prefix. If you want to specify the prefix manually, select Other.

4. Click Next.

5. You will now need to add one or more UUID blocks; by default, there are none in this pool. Click Add (with the green plus symbol) to add a UUID suffix pool.

6. Specify a starting value and the size of the UUID suffix pool, then click OK.

7. The UUID block you created is now listed in the UUID suffix pool. Repeat steps 5 and 6 to add additional blocks as needed. When you are done, click Finish.

## Defining an IPMI Profile

_Location in UCSM: Servers tab  Servers > Policies > root > IPMI Profiles_  
_Prerequisites: None_

To define an IPMI policy---which can be used by VMware DPM to control blade power state---follow these steps:

1. Right-click on IPMI Profiles and select Create IPMI Profile.

2. Specify a name and a description for this IPMI profile, then click the green plus symbol to add an IPMI profile user.

3. Supply a username and password for the new IPMI profile user and select to make the new user an admin user.

4. Click OK to add the IPMI profile user.

5. Click OK to create the new IPMI profile.

## Creating a Serial over LAN Policy

_Location in UCSM: Servers tab  Servers > Policies > root > Serial over LAN Policies_  
_Prerequisites: None_

Here are the steps to create a serial over LAN policy:

1. Right-click on Serial over LAN Policies and select Create Serial over LAN Policy.

2. Specify a name and description for the serial over LAN policy, whether to enable or disable serial over LAN access, and the desired serial speed.

3. Click OK to complete the creation of the serial over LAN policy.

## Creating a Boot Policy

_Location in UCSM: Servers tab  Servers > Policies > root > Boot Policies_  
_Prerequisites: None_

To define a boot policy, use these steps:

1. Right-click on Boot Policies and select Create Boot Policy.

2. Specify a name and description for this boot policy.

3. Add boot devices from the list of potential devices on the left side of the window. For example, to add SAN boot, expand vHBAs and click on SAN boot.

4. Specify the vHBA name and whether it should be primary or secondary. Make a note of the vHBA name specified here; it must match the vHBA name you specify later in the service profile template. Click OK to add the vHBA to the boot policy.

5. Click Add SAN Boot Target.

6. Specify the boot target LUN ID (generally should be zero) and the boot target WWPN, which would be the WWPN of a port on the storage array.

7. Select whether this target should be primary or secondary, then click OK.

8. Repeat this process to add LAN boot or local boot selections. If you add LAN boot, then (just as with SAN boot) be sure to make a note of the vNIC name you specify in the boot policy. You'll need to make sure it matches information in the service profile or service profile template later.

9. Click OK when you are finished specifying the boot devices and the order of the boot devices. (You can use Move Up and Move Down to change the desired order of the boot devices.)

That wraps up part 3 of the series. I've now covered all the different elements that I recommend creating before creating an actual service profile or service profile template. In the next and final post in the series, I'll walk through creating a service profile and show you how easy it is once you've defined all these other objects.

[1]: {{< relref "2010-05-05-creating-ucs-service-profiles-part-1-networking-elements.md" >}}
[2]: {{< relref "2010-05-07-creating-ucs-service-profiles-part-2-storage-elements.md" >}}
