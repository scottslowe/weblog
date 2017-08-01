---
author: slowe
categories: Musing
comments: true
date: 2006-12-28T17:07:05Z
slug: personal-computing-as-a-collection-of-vms
tags:
- ESX
- Linux
- Macintosh
- Virtualization
- VMware
- Windows
title: Personal Computing as a Collection of VMs?
url: /2006/12/28/personal-computing-as-a-collection-of-vms/
wordpress_id: 393
---

There appear to be basically two views on how virtualization will affect the future development of operating systems and computing environments _in the personal computing space_. One camp believes that virtualization functionality will be present within the operating system. Whether that virtualization functionality comes bundled with the operating system or is a third-party add-on to the operating system is, quite frankly, irrelevant to this particular discussion. The other camp believes that virtualization will be outside the operating system, perhaps in the form of a hypervisor or thin virtualization layer that resides "below" the OS and governs access to hardware. Again, the discussion of whether this virtualization layer comes bundled with the hardware or comes from a third-party vendor is an interesting discussion (and one that I'd like to have), but is not relevant right at this moment.

My discussions of application agnosticism puts me in the camp that places virtualization functionality in the operating system. On the desktop side (not speaking of servers here), that kind of makes sense to me. It seems to me that the _simplest approach_---placing virtualization functionality within the operating system---is likely to be the approach that most people will accept. We have to keep in mind that millions of users out there are not nearly as technical as we are, and for them simple is good. It may not be the most technologically advanced approach but rather the simpler approach that wins out (especially on the consumer side).

Now, having said all that, I'd like to take a closer look at the alternate approach to having virtualization placed within the operating system. In this scenario, there is virtualization functionality that sits below the idea of today's general purpose OS. For those of you familiar with [ESX Server](http://www.vmware.com/products/vi/esx/), think of it that way---some sort of bare metal virtualization layer that controls the hardware. From there, a collection of VMs will cooperatively provide the various services that are today provided by the general purpose OS. This idea is expressed [in this article by Ron Oglesby](http://www.brianmadden.com/content/content.asp?id=623) (also linked to by [this VMTN Blog entry](http://blogs.vmware.com/vmtn/2006/12/blogscottloweor.html) as well).

In this approach, you might have a networking VM that is responsible for scanning inbound and outbound traffic, managing security policies, interacting with corporate networks and network access controls, etc. You can think of this as the "firewall" component of the general purpose OS (Windows Firewall on [Windows](http://www.microsoft.com/windows/), ipfw on [Mac OS X](http://www.apple.com/macosx/), iptables on Linux), but more feature-rich and more isolated (the idea being that it is therefore more secure and harder to bypass or disable). Likewise, you might have a VM designed specifically for running sensitive corporate applications, a VM for surfing the Internet, and a VM that provides anti-virus services to the other VMs. Taken individually, none of these VMs could replace today's general purpose OS; taken as a whole, the collection of VMs provides the services and functionality of a general purpose OS, but with greater isolation, encapsulation, and protection between these "service" VMs.

Is this a viable approach? Not today, in my opinion, but certainly in the future. (To be completely fair, Ron's article was written in the context of the long-term impact of virtualization, so we can't really look at today's feasibility.) As [Intel](http://www.intel.com/) and [AMD](http://www.amd.com/) continue to add virtualization support in hardware and performance draws nearer to "native" performance, this definitely becomes a more viable approach. A couple questions persist in my mind, though:

* What is the mechanism whereby a user adds new functionality to their computing environment, i.e., how does a user add a new service VM?

* What kind of mechanism or tools are provided to the user to help manage, operate, or configure these service VMs?

Let's say that the user's "normal" working environment exists in a VM that runs Windows, Linux, or the like. We'll call this VM-Home. From VM-Home, the user needs to be able to access the functionality of a networking/firewall VM (we'll call this VM-FW) and a corporate applications VM (we'll call this VM-Corp). How does the user go about switching between these VMs, like between VM-Home and VM-Corp? Does each VM provide its own windowing environment? How is switching between these windowing environments handled? Is there a common windowing environment provided by the virtualization layer? Is there some internal networking connectivity between VM-Home and VM-FW that allows the user to manage the VM-FW functionality? Where does the user go, or what program does the user run, to add a new VM (say, an anti-virus VM) to his/her environment?

"Scott, stop being so picky," you say. "This is all being talked about in theory, anyway. It's not like we need to have all the answers right now."

You're right, we don't. But as we look at how these questions may be answered (someone's got to answer them sometime), it seems that we'll need to add some functionality to the virtualization layer in order to make it easier/more seamless to switch between the VM environments. Users will want a seamless UI to work with, so we may need to add a windowing environment to the virtualization layer. Either that or we'll have to enable some sort of mechanism whereby other VMs can display windows inside another VM, and now we're breaking down the isolation/protection boundaries that we originally found to be desirable. Users will want to be able to copy and paste between the VM environments, so we'll need to add that functionality to the virtualization layer. Users will want to be able to double-click an icon and have it launch in the appropriate VM environment, so now we've got to add some links and communications channels between the various VM components in our computing solution. As each of these pieces of functionality is added, the virtualization layer starts to look more like a general purpose OS---just one that's leaner, meaner, and free of years of legacy code.

As this virtualization layer starts to resemble a general purpose OS and as the general purpose OS starts to incorporate technologies such as virtualization, application-specific subsystems, and the like, these two start to look a lot like each other. That brings us back to a central question in this discussion, a question I asked when I first started discussing [the future of the OS][1]:

>So I guess the future of the operating system depends on your perspective. If you're an operating system guy, you'll say that the OS has a bright future, and point to developments such as built-in paravirtualization and bundled hypervisors to prove your point. If you're a virtualization guy, you'll say that the OS is dead, and you'll point to developments such as third-party paravirtualization and independent hypervisors to prove your point. Which of these two is correct?

Indeed! Which approach do you think desktop computing will take? Application agnosticism, in which virtualization and other technologies are placed within the operating system, or groups of virtual machines ("VM cooperatives"? "VM federations"? "OS agnosticism"? Need a fancy marketing term again...) coordinated by a hardware/firmware virtualization layer?

What do _you_ think?

[1]: {{< relref "2006-12-05-the-future-of-the-os.md" >}}
