---
author: slowe
categories: Information
comments: true
date: 2015-09-01T11:17:55Z
tags:
- VMworld2015
- VMware
- Virtualization
title: VMworld 2015 Day 2 Keynote Liveblog
url: /2015/09/01/vmworld2015-day2-keynote-liveblog/
---

The day 2 keynote kicks off with another Cloud Academy presentation... After the video wraps up, Sanjay Poonen takes the stage. Poonen briefly recaps yesterday's messaging, and then moves into the focus of today's keynote---focusing on the "any application and any device" part of the "Ready for Any" messaging.

According to Poonen, the core of the solution for "any application on any device" is VMware's Workspace Suite. Workspace Suite creates the magic of "enterprise computing with consumer simplicity." How? It starts by building upon the core of virtualized infrastructure, made possibe by VMware's compute, storage, and network virtualization solutions. Combined with a strong management layer and hybrid cloud solutions, this becomes the software-defined data center (SDDC). Somehow, though, this stuff needs to be connected to the end users---via desktop, mobile, content collaboration, and tying it all together with identity management. Poonen points to innovation in all of these areas.

Obviously, mobile is a category that is growing very rapidly, and Poonen talks about VMware's movement in this space via the AirWatch acquisition. And the use of mobile devices is also key to VMware's identity efforts as well. Poonen shows a video with a few customer testimonials, and then introduces Jim Alkove, Corporate VP in charge of enterprise management and security for Windows at Microsoft. Alkove talks about how he views Windows 10 as a key effort to simplify enterprise IT, and Poonen re-affirms VMware's commitment to focus on meeting customers' needs instead of allowing corporate rivalries and competition to derail efforts.

Poonen brings Noah Wasmer onto the stage to show off some new stuff. After showing a few features around identity management, Wasmer announces Project A-Squared, which combines Airwatch and VMware AppVolumes to enable application delivery to Windows 10 systems, even physical systems. What about iOS and Android devices? Wasmer next shows using Airwatch to provision applications to these types of devices and shows integration into these applications to help with identity management and single sign-on. Next, Wasmer shows Poonen how high-performance graphics have been integrated into this solution as well, using technology jointly developed with NVIDIA.

Poonen mentions security next, and talks about the integration of Airwatch, Horizon, and NSX. Wasmer demonstrates how policy settings in NSX can affect Airwatch connectivity and data access for mobile applications.

At this point, Poonen starts wrapping up. He recaps the announcements and re-iterates the key message: "enterprise security with consumer simplicity".

Poonen leaves the stage and Martin Casado, SVP and GM of the NSBU, takes the stage. Casado starts his discussion about how the application has changed---the application is now a combination of servers, clients, load balancers, firewalls, and storage repositories. He says that now, in a very non-marketing way, applications have become the network. Applications are now truly distributed systems.

Casado starts focusing on one particular aspect of applications: provisioning. Provisioning distributed systems can be extraordinarily difficult. Another challenge is troubleshooting, which Casado equates to the words "root canal." Finally, security is a key challenge. Due to the difficulty in provisioning, modifying, and troubleshooting applications, organizations often fail to properly apply or maintain security controls, which means they end up out of compliance or, even worse, completely insecure. This leads Casado into a customer video from Tribune Media.

Network virtualization offers solutions to the three challenges he outlined earlier. Network virtualization simplifies provisioning, enables new forms of troubleshooting that weren't possible before, and empowers new levels of security. Casado points to the release of NSX 6.2---which offers a host of new features and new functionality---and talks about customer momentum with VMware NSX. Additionally, Casado talks about partner integration with NSX to add new types of services, one example of which is Palo Alto Networks.

Casado introduces another customer video, this time from Global Speech Networks, and how they are using VMware NSX in their business. Following the video, Jacob Rapp joins Casado on stage to walk through a demo of some of the new things that Casado was talking about. The demo shows a development cloud in NSX, and shows how vRealize Operations Manager can show detailed information, drilling from application level view all the way down to link-level view.

Following the demo, Casado talks about a picture he took that shows the Golden Gate bridge emerging from the fog, and he views network virtualization a lot like this picture. There's so much more yet to be unveiled, and to help talk about some of the things that are emerging Casado brings Tom Corn on to the stage to talk about some of the security-focused efforts that VMware has underway.

Corn talks first about encryption, and how VMware is working to turn that into a distributed service (or, in Casado's terms, an infinite service). As a result of this developent effort, VMware aims to make encryption---where every packet coming and going into or out of a system---a simple attribute of the application. Check the box, and traffic is automatically encrypted. The reason that encryption hasn't been adopted widely yet, according to Corn, is "root canal." In other words, it's painful and difficult, and no one wants to go that route. Once you implement distributed network encryption, all those pain points go away.

Encryption at the network level is cool, but it isn't the whole story, acording to Corn. You also need encryption for data at rest, and implementing data encryption at rest as a distributed solution also address many of the pain points that are keeping enterprises from adopting and using point solutions in this space.

Casado takes a moment to thank the community as he marks 10 years of working in the networking space, and then leaves the stage. As he leaves the stage, another customer video runs. The video handles the transition from Casado's network- and security-focused talk to a talk about how to compete in today's economy, and introduces Pat Gelsinger to the stage.

Gelsinger talks about the growth of Internet connectivity, and how various areas have differing levels of connectivity. Connectivity changes how education, healthcare, and other areas are implemented, managed, and delivered to citizens. By the same token, after 20 years, we went from 0.1 devices connected per person to 2.9 connected devices today, and a projected 5.9 devices per person in the next few decades. iOS app revenue has passed US film revenues, and this doesn't even include revenue from Android applications.

Naturally, all these trends have had an enormous impact on economics. Gelsinger spends a few minutes talking about these trends, and how these trends have impacted and continue to impact businesses. Along the way, he recaps the importance of the moves that VMware has made and the product announcements from VMware, attempting to show attendees a common thread among all the announcements and all the keynotes that pulls it all together.

I had to wrap up the liveblog now due to scheduling constraints again on my end. Sorry folks, I tried to capture as much as possible---hopefully what I've captured here has been useful.