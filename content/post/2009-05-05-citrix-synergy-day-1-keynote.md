---
author: slowe
categories: Liveblog
comments: true
date: 2009-05-05T21:52:51Z
slug: citrix-synergy-day-1-keynote
tags:
- Citrix
- VDI
- Virtualization
- Xen
title: Citrix Synergy Day 1 Keynote
url: /2009/05/05/citrix-synergy-day-1-keynote/
wordpress_id: 1315
---

In advance of speaking at Virtualization Congress 2009 on Thursday, I'm attending Citrix Synergy. The Internet connection here in the main conference call is unavailable, so I'll publish this as soon as I have connectivity back to the site.

The keynote starts with Garr Reynolds, an associate professor at Kansai Gaidai in Japan. I'm a bit unsure why he is on-stage getting the keynote started, but it's a good presentation. After leading the audience through a self-introduction in Japanese, Garr Reynolds launches into a discussion of simplicity, the beginner's mind, and nakedness (no, not that kind!), all centered around "thinking differently". He asks a couple of good questions and makes a couple of good points: Is simplicity the same as easy? No, because easy would be doing the same thing. Doing the same thing is easy? What is simplicity? It is not stupid, it is actually quite difficult. _Simplicity_ is not the same as _simplistic._ Simplicity means the achievement of maximum effect with minimum means.

Reynolds now moves on to the second part of his talk: the beginner's mind. It means losing the fear and taking a risk. That is the basis of the beginner's mind. We need to lose the fear of being wrong so that we can come up with something original. In Zen thinking, we need to make a decision and let the rest go. In the words of Yoda, "You must unlearn what you have learned."

Finally, Reynolds moves on to nakedness. Specifically, "shizen," or naturalness. It means being open, honest, transparent. After a brief discussion, he moves back to the key themes and wraps them together into a closing idea around using these themes to seek continuous improvement.

John Gantz of IDC now takes the stage. Gantz is the Chief Research Officer for IDC and will be speaking about renaissance and crisis. Gantz believes that the economic crisis holds opportunity for IT to "sharpen its swords" and figure out the best ways to use technology to "do more with less." As he continues on with his presentation, it's more of the same. In my opinion, there's nothing being presented here that hasn't already been said. We already know that we will have the same or fewer staff to deal with more servers, more mobile users, more data, and more information. That's nothing new, to be honest.

After a 20 minute coffee break, the MGM/Mirage CTO gets on the stage, sells and pitches the MGM Grand for a bit, and then very briefly discusses MGM/Mirage's use of Citrix products. He then introduces Mark Templeton, CEO of Citrix.

Templeton's vision is to deliver every desktop and every application, and to turn every data center into a dynamic delivery center. Today's discussion will focus on users and desktops; tomorrow's discussion will center on data centers, servers, and clouds. Templeton mentions the Citrix Innovation Award and the finalists: Emory Healthcare, HDFC, and Tesco. Next he shows the video from Tesco that describes how they use Citrix products and solutions. Tesco's story of virtualizing their data center is impressive, but not terribly innovative in my opinion.

Templeton lays out a discussion on how consumer-oriented computing, coupled with Web 2.0, now has a steeper technology innovation curve than enterprise computing. Templeton challenges the idea of traditional computing and traditional ways of thinking. Why does the enterprise think that the network must be owned and controlled end-to-end? Why do enterprises think that the company must own and control endpoints? According to Templeton, the computing industry is full of dead ideas, and those dead ideas are driving the industry closer to extinction. Much in the same way the mainframe was eclipsed by the personal computer, today's enterprise computing environment will be eclipsed by consumer computing and "thinking differently" if we don't change. The fight is between the momentum of consumerization versus the inertia of dead ideas.

Templeton shares some math around complexity; reducing parts can have a very significant impact on the reliability of the overall systems. What if the number of parts in the enterprise could be reduced? The reliability of enterprise systems could be improved.

Next, Templeton explains how he thinks that the DirecTV model---controllers connected to receivers over a delivery network that operates independently of content and endpoints---is the right model for the enterprise network. Controllers in the network work with gateways and repeaters in the delivery network to connect to receivers on various endpoints.

With that, Templeton introduces the video for Emory Healthcare, the next finalist for the Citrix Innovation award.

Now it appears we will get into the real meat of today's presentation, as Mark Templeton delves into a discussion of IT's costs in supporting users and endpoints. According to Templeton, you can't think of buying desktops or installing applications; you have to think differently and think of delivering desktops and delivery applications from a virtualized controller at the head (the data center). The desktop now becomes a set of components that can be isolated and separated. This allows organizations to independently deliver, on demand, user profiles, applications, and desktop operating systems. Clearly, XenApp is a leading component in delivering applications independently of the desktop operating system. He mentions the recent announcement by SAP of using XenApp on XenServer to deliver applications to SAP users. With over 200,000 customers worldwide, Citrix calculates that about 25 million unique applications are being delivered to 100 million users daily.

XenDesktop is another key component of Citrix's vision of delivering desktops, applications, and user profiles. According to Templeton, XenDesktop delivers the best user experience on the broadest range of devices across the widest selection of delivery networks. Collier County Schools, who presented Templeton with Citrix's first PO for XenDesktop last year at Synergy, provided an update on where things stand one year later. But why isn't Citrix talking about more XenDesktop customers? Apparently Citrix has closed, with the help of CSC, a deal for 40,000 users worldwide. Templeton did not, unfortunately, disclose the identity of the customer.

Mark Templeton again refers to the partnership, or project, between Citrix and Intel regarding the developoment of a client hypervisor that would provide a solution for disconnected mobile users. He is, of course, referring to Project Independence. It's new name is XenClient. (Not unexpectedly, Pat Gelsinger will discuss the Xeon 5500 and XenServer tomorrow.) XenClient is intended to be everywhere, will plug into the rest of the Citrix architecture, and will be free.

Templeton now moves into a discussion about delivery. He seems to knock VMware by saying that virtualizing desktops is not the same as virtualizing servers because "it's all about the experience." Citrix HDX technologies is the solution to delivering applications across LAN, wireless LAN, Internet, and 3G networks regardless of the application type. He shows off HDX vs. "the other guys" by showing a series of videos comparing HDX against other delivery protocols. According to Templeton, HDX delivers a high-fidelity experience using only 1/10th of the bandwidth compared to other protocols. With that lead-in about HDX, Templeton shows the video for HDFC Bank, the third candidate for the Citrix Innovation award.

Templeton charges the audience to move away from thinking in traditional fashion to instead think of IT as publishing resources to which users can subscribe. This goes back to the DirecTV model, and it is a natural lead-in for Templeton to discuss Citrix Receiver. Citrix Receiver is described as a universal client for IT service delivery. Citrix Receiver allows IT to advertise services, using existing infrastructure, to which users can subscribe from a full range of endpoints across a variety of delivery networks.

Next up is a demo of Citrix Receiver. Templeton shows off Citrix Receiver running on both Windows Vista as well as Windows 7. In fact, the Windows 7 desktop is actually a remote desktop hosted on XenDesktop in Fort Lauderdale, FL. The demo was actually quite impressive---you probably would not have been able to guess that the desktop was a remotely hosted desktop. "Is it live, or is it HDX?" That's Templeton's question about just how well HDX works.

One new component is required on the back-end to help support this infrastructure: Citrix Merchandising Server. This component keeps clients updated, schedules plug-in distribution, advertises new services, centralizes operations, and handles reporting and logging. Merchandising Server is actually a set of virtual machines intended to run on top of XenServer (can you say virtual appliance?).

Citrix Receiver for Windows 1.0 is available today, and is free. Citrix Receiver will be coming to the iPhone, and Templeton provides a demo of Citrix Receiver on an iPod Touch over a Wi-Fi connection. The iPod is connecting to a Citrix head-end hosted on the Amazon EC2 cloud. The iPhone Receiver also offers a feature called Doc Finder, which makes it easier to find and open documents on the iPhone. The next demo actually shows a 3-D animation running on the iPhone. This is pretty impressive.

Demo accounts are available for users to use their iPhones to connect to the EC2 cloud and experience Citrix Receiver for themselves. The Citrix Receiver for the iPhone is available today on the App Store, and it is free. Windows Mobile and Symbian phones will be supported in the future, as well as Android-based phones through a partnership with Open Kernel Labs and the OLK4 Hyper-cell hypervisor.

Templeton next announces Citrix Dazzle, a self-service "app store" for employees and IT resources. It builds upon Citrix Receiver to provice self-service discovery and access to applications that are delivered via Citrix infrastructure. The Citrix tagline for Dazzle is "Putting the personal back into personal computing". Dazzle puts an iTunes/App Store interface on top of the applications that are being published and delivered by the same head-end services that drive Receiver. It's an interesting take, but I'm not yet sold on the idea.

Dazzle will be available later in the year, and yes, it will be free.

With that, Templeton closes the keynote---after an astounding four hours---and reminds everyone that tomorrow's keynote will be focused on servers and the data center.
