---
author: slowe
categories: Rant
comments: false
date: 2006-03-02T15:34:06Z
slug: application-virtualization
tags:
- Citrix
- Networking
- Virtualization
title: Application Virtualization
url: /2006/03/02/application-virtualization/
wordpress_id: 193
---

I've worked with [Citrix](http://www.citrix.com/) products for quite a while, even dating as far back as the WinFrame products (before the relase of Windows NT Server 4.0 and WinFrame's evolution into MetaFrame). I've always been a strong supporter of their products, because let's face it---when it comes to delivering applications to remote locations in a reasonably bandwidth-sensitive way, Citrix is a great solution. But it's not just about application delivery, it's also about _application virtualization._

It's funny how you can really work with a product, understand how a product works, and even strongly recommend a product without seeing the product's full potential. That's how it was with [Citrix Presentation Server](http://www.citrix.com/English/ps2/products/product.asp?contentID=186) until just a few days ago. In talking about a solution using Citrix Presentation Server and the Citrix Program Neighborhood Agent, I realized that this solution virtualized the applications.

What do I mean? I mean that it is impossible for an end user to determine if the application is a locally-installed application or an application that is being delivered via Citrix Presentation Server. Not only is it impossible, but it is also irrelevant. Who cares if the application is local or remote? Either way, the end user accesses it the same way (via an icon on the Start Menu or on the desktop) and it behaves the same way (users can copy and paste between applications, move/minimize/resize the application window, etc.).

I'll grant you that perhaps the term "application virtualization" is not the most accurate term to use here (a [Google](http://www.google.com/) search for the phrase "application virtualization" turns up [Softricity](http://www.softricity.com/home/index.asp), whose technology is appropriately named "application virtualization"), but the idea is sound. What is happening here is that the location of the application is being abstracted, so that where the application is installed and is being executed is no longer tied to where the application's output is being displayed.

It's pretty cool stuff, if you ask me. Combine this with machine virtualization (such as that provided by [VMware](http://www.vmware.com/)) and storage virtualization (via a SAN), and you start to get a truly dynamic environment where applications can be load balanced across virtual servers, which in turn can be moved on an "as-needed" basis to make the most of the underlying physical hardware.
