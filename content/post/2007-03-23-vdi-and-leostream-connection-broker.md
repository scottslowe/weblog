---
author: slowe
categories: Explanation
comments: true
date: 2007-03-23T18:10:09Z
slug: vdi-and-leostream-connection-broker
tags:
- ActiveDirectory
- ESX
- Networking
- Virtualization
- VMware
title: VDI and Leostream Connection Broker
url: /2007/03/23/vdi-and-leostream-connection-broker/
wordpress_id: 433
---

For those that aren't familiar with this latest TLA (three-letter acronym), VDI stands for "Virtual Desktop Infrastructure" and it's an alternate way of utilizing virtualization in the datacenter. Instead of virtualizing server instances, we virtualize desktop instances, and then provide a means for users to connect in to one of these available instances. (You can learn more at VMware's [Virtual Desktop Infrastructure web page](http://www.vmware.com/solutions/desktop/vdi.html).)

VDI isn't a perfect fit in all situations; in some cases, a more "traditional" approach such as [Citrix Presentation Server](http://www.citrix.com/English/ps2/products/product.asp?contentID=186&ntref=hp_nav_US) or [Microsoft Windows Terminal Services](http://www.microsoft.com/windowsserver2003/technologies/terminalservices/default.mspx) may be a better fit. However, in some cases, VDI is a good fit. That's the kind of situation I'm working in right now, where a customer of mine needs to deploy an application that needs fast access to corporate data, but can't use Citrix (for a variety of reasons). So we're looking at VDI.

Part of any VDI solution is a _connection broker._ The connection broker, at its most basic level, performs the following tasks:

* Accepts incoming connection requests from clients

* Finds an available hosted desktop

* Brokers the connection between the client and the available hosted desktop

There are a number of connection brokers (CBs) out there from a variety of vendors. I haven't had the opportunity to use all of them; only [Propero](http://www.propero.com/) and [Leostream](http://www.leostream.com/productVHDC.html). I found Propero's product to be much too complicated; it seemed as if the application was really designed for something else and acting as a VDI connection broker was kind of an afterthought. Leostream's product, on the other hand, seems really streamlined and really focused on the CB market. With Leostream's product, I was able to fairly quickly setup and test the following functions:

* Integrate CB authentication with Active Directory, so that users authenticated to the CB using their AD username and password

* Make policy assignments within the Leostream CB based on AD group memberships, so that members of different groups were assigned to different policies

* Control access to different pools of hosted desktops based on policy assignment

* Create pools of hosted desktops by assigning "tags" to them; these tags create pools of hosted desktops by collecting instances with similar characteristics

* Integrate the CB into [VirtualCenter](http://www.vmware.com/products/vi/vc/), so that the CB could suspend a VM when a user disconnected, then resume the VM when another user needed it

It may be that some of the other CBs out there also work as well as Leostream; I don't know since I haven't had the opportunity to work with all of them (note to vendors: I **will delete** blatant marketing pitches in the comments). I do know that the Leostream product works well thus far.

It took me a little bit of time to get accustomed to how the Leostream broker works (different terminology, I suppose), but once I understood how it works I found it pretty easy to make it do what I wanted it to do. The pieces are all interconnected, though, so allow me to walk through a set of steps in the event you find yourself using the Leostream product in the future.

Let's say you wanted to create a pool of workstations running Windows XP Professional, and you only wanted members of the "Hosted XP Users" group in Active Directory to have access to that pool of hosted workstations. The process is actually pretty straightforward:

1. Configure the VC integration, so that the CB can automatically retrieve the list of available VMs from VirtualCenter and import them automatically.

2. Tag the VMs running Windows XP Professional with a tag, such as "WinXP".

3. Create a policy, perhaps called "WinXP Policy", that defines the pool by selecting all systems with the "WinXP" tag. (If you wanted further limit the VMs inside that pool, you could add additional tags and use the "AND" logic to say that all VMs must have all selected tags.)

4. Configure the AD authentication server to assign members of the "Hosted XP Users" group to the "WinXP Policy" using the Assignments section of the authentication server configuration.

That's it! When an AD user who is a member of the selected group authenticates to the CB, he or she will automatically be brokered to an available system in the pool of systems with the selected tag(s). Further, since this is WinXP (and of course we selected RDP as our connection protocol), sign-in to the hosted XP desktop is automatic---the user will not get prompted again for credentials. Cool, eh?
