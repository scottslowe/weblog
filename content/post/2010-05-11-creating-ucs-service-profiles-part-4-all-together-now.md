---
author: slowe
categories: Tutorial
comments: true
date: 2010-05-11T10:38:10Z
slug: creating-ucs-service-profiles-part-4-all-together-now
tags:
- Cisco
- Hardware
- UCS
- Virtualization
title: 'Creating UCS Service Profiles Part 4: All Together Now'
url: /2010/05/11/creating-ucs-service-profiles-part-4-all-together-now/
wordpress_id: 1940
---

This is the fourth and final post in my series on creating Cisco Unified Computing System (UCS) service profiles. What I've tried to do so far in this series is define the various elements that I recommend you create or define before you start trying to create a service profile or service profile template. Certainly you could start creating service profiles right off the bat, but I've found it easier to have all these other elements defined first. In [part 1][1] of the series, I showed you what networking-related elements to define. In [part 2][2] of the series, I described the storage-related elements, and in [part 3][3] of the series I covered the remaining elements.

## Creating a Service Profile Template

Now that all the prerequisite elements have been defined, follow these steps to create a service profile template (creating an actual service profile would follow the same basic steps):

1. Right-click on Service Profile Templates and select Create Service Profile Template.

2. On the first screen, supply a name for the new service profile template and make the template an Updating template. (This will ensure that changes made to the template are propagated to service profiles cloned from this template. You can always unbind the service profile from the template to prevent changes from propagating.)

3. Select the UUID pool created earlier.

4. Supply a description for the service profile template. Click Next to continue.

5. On the second screen, select the desired local disk configuration policy.

6. Do not select a scrub policy.

7. Select Expert for the method of configuring SAN connectivity.

8. Select the WWNN pool created earlier.

9. Near the bottom of the screen (you might need to scroll down to see it), click Add (with the green plus symbol) to add a vHBA.

10. Specify a name for the vHBA, such as "vHBA-A". This name should match the name specified in the boot policy defined earlier.

11. Select Use SAN Connectivity Template.

12. Select the vHBA template created earlier and, optionally, an adapter performance profile. Click OK to return to the service profile template wizard.

13. Repeat to add a second vHBA (if a dual fabric configuration). When using blades that have the Cisco Virtual Interface Controller (VIC) mezzanine card (aka "Palo"), the number of vHBAs can be greater than two.

14. When you are done adding vHBAs, click Next to continue.

15. On the third screen, do not select a dynamic vNIC connection policy.

16. Select Expert for how you would like to configure LAN connectivity.

17. Click Add (with the green plus symbol) to add a vNIC.

18. Specify a name for the vNIC (like "vNIC-A") and select Use LAN Connectivity Template. If you intend to use network boot, the name specified here must match the vNIC name specified in the boot policy.

19. Select the vNIC template you created earlier and, optionally, select an adapter performance profile.

20. Click OK to return to the service profile template wizard.

21. Repeat steps 17 through 20 for each desired vNIC. When using blades that have the Cisco VIC mezzanine card the number of vNICs can be greater than two.

22. When you are finished adding vNICs, click Next to continue.

23. Do not make any changes to the vNIC/vHBA placement. Click Next to continue.

24. Select the boot policy you created earlier. Be sure that the local disk configuration policy selected in step 5 is consistent with the boot policy you just selected. (This means that if the local disk configuration policy is set for No Local Disk, then your boot policy should not specify local disk as a boot method.)

25. When prompted for server assignment, choose Assign Later and click Next.

26. At the final screen, choose the IPMI profile you created earlier and the serial-over-LAN policy you created earlier.

27. Click Finish to complete the creation of the service profile template.

And that's it! What you should have found in walking through these steps is that the process of actually creating the service profile template was really just a matter of selecting pools and policies that you'd already defined. By doing all the legwork up front, you dramatically simplify the creation of the service profile or service profile template.

Of course, one of the advantages of UCS is its flexibility; meaning you don't _have_ to do things this way. This is just one way that I've found quite useful and, to be honest, a way that makes sense to me.

Courteous comments are always welcome! Feel free to add any clarifications, corrections, or other thoughts in the comments below.

[1]: {{< relref "2010-05-05-creating-ucs-service-profiles-part-1-networking-elements.md" >}}
[2]: {{< relref "2010-05-07-creating-ucs-service-profiles-part-2-storage-elements.md" >}}
[3]: {{< relref "2010-05-10-creating-ucs-service-profiles-part-3-other-elements.md" >}}
