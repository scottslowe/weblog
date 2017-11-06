---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-06T20:00:00Z
tags:
- OpenStack
title: Battle Scars from OpenStack Deployments
url: /2017/11/06/battle-scars-openstack-deployments/
---

This is the first liveblog from day 2 of the OpenStack Summit in Sydney, Australia. The title of the session is "Battle Scars from OpenStack Deployments." The speakers are Anupriya Ramraj, Rick Mathot, and Farhad Sayeed (two vendors and an end-user, respectively, if my information is correct). I'm hoping for some useful, practical, real-world information out of this session.<!--more-->

Ramraj starts the session, introducing the speakers and setting some context for the presentation. Ramraj and Mathot are with DXC, a managed services provider. Ramraj starts with a quick review of some of the tough battles in OpenStack deployments:

* Months to deploy OpenStack at scale
* Chaos during incidents due to lack of OpenStack skills and knowledge
* Customers spend lengthy periods with support for troubleshooting basic issues
* Applications do not get onboarded to OpenStack
* Marooned on earlier version of OpenStack
* OpenStack skills are hard to recruit and retain

Ramraj recommends using an OpenStack distribution versus "pure" upstream OpenStack, and recommends using new-ish hardware as opposed to older hardware. Given the last bullet, this complicates rolling out OpenStack and resolving OpenStack issues. A lack of DevOps skills and a lack of understanding around OpenStack APIs can impede the process of porting applications to OpenStack, which in turn makes it harder to justify the cloud project.

Ramraj then spends a few minutes talking about how and why a managed services provider might be able to help address some of the challenges around an OpenStack deployment. Ramraj and the other presenters do not, unfortunately, provide any suggestions for resolving the challenges described above other than recommending the use of a managed services provider, making this session far less useful than it might otherwise have been.

Ramraj closes out her portion of the session with a quote from Sun Tzu's _Art of War_: "Every battle is won before it is fought."

At this point, Ramraj brings up Farhad Sayeed to tell the story of deploying OpenStack at American Airlines. Sayeed spends a couple of minutes talking about the goals American Airlines hoped to achieve with an OpenStack-based private cloud deployment. Then Sayeed describes a couple "road bumps" they experienced:

* Had to work closely with the networking group
* Tested a number of different distributions

I'm not clear how either of these are actual problems; rather, they seem like perfectly natural parts of deploying something like OpenStack (which affects lots of different aspects of IT).

Next Sayeed describes some of their target workloads; American Airlines is pushing a "PaaS"-first strategy leveraging Cloud Foundry. For IaaS, American is supporting RHEL and currently testing Ubuntu. American Airlines is still in a very exploratory stage with containers.

Looking back, Sayeed shares a few lessons learned:

* Need a collaborative culture
* Lots of training and retooling required
* Less is more (less customization makes it easier)
* Use a good service provider

I can absolutely agree with the first three lessons learned shared by Sayeed; the fourth _might_ be the right approach for some organizations, but not necessarily all organizations.

Sayeed closes out his portion of the session by encouraging attendees to embrace automation and orchestration, and hands it over to Rick Mathot (also of DXC). Mathot spends a few minutes talking about managing the "toughest" battle: managing client expectations. Mathot talks about topics like focusing on business value (asking "is it useful?" and be able to quantify business value), lead from the front (but keep an eye on the rearview mirror), and make sure what you're doing is relevant.

At this point, Mathot wraps up the content and opens the session up for questions from the audience.
