---
author: slowe
categories: Liveblog
comments: true
date: 2017-08-28T07:00:00Z
tags:
- VMworld2017
- VMware
- vSphere
- AWS
title: 'Liveblog: VMworld 2017 Day 1 Keynote'
url: /2017/08/28/vmworld-2017-day-1-keynote/
---

This is a liveblog of the day 1 keynote at VMworld 2017 in Las Vegas, NV. There was a bit of a kerfluffle regarding seating (the local facilities staff didn't want to let me sit in the bloggers' area because "you're not a blogger"), but I managed to snag a seat anyway.<!--more-->

Prior to the keynote, a number of announcements were released; here's a quick look at a few of them:

* [VMware and AWS Announce Initial Availability of VMware Cloud on AWS][link-1]
* [VMware Integrated OpenStack 4.0 and vRealize Network Insight (vRNI) 3.5][link-2]
* [Availability of VMware Cloud Services (Discovery, Cost Insight, Network Insight, AppDefense, NSX Cloud, Wavefront)][link-3] (this has been a big part of my work life for the last year, great to see them available)

Pat Gelsinger takes the stage at about 9:05am, after a very cool AR/VR demo. Gelsinger welcomes the crowd, and takes a minute to reflect upon his time at CEO of VMware. Gelsinger also takes a moment to talk about the devastation caused by Hurricane Harvey, and urges attendees to help support the recovery of that effort.

Gelsinger talks how "science fiction" is becoming "science fact": exoskeletons, teleportation, and genetically modified organisms via CRISPR. He goes on to talk about how the rapid development of technology has changed people's expectations and frame of reference (example given was the intuitive nature little kids just "know" how to use handheld devices). From self-driving cars to digital banking and healthcare, technology is breaking out of the technology sphere and affecting all areas of business.

Gelsinger uses the transformation of digital media and entertainment as an example of how technologies can totally change an industry and how consumers interact with that industry. (There were a number of chuckles from the audience when he mentioned WinAmp and RealPlayer.) He goes on to talk about e-commerce (only 1/10th changed according to Gelsinger) and healthcare (still spending 81% on "keeping the lights on"), and then shifts his focus to talk about how transformation of these industries is fundamentally affecting the nature of labor/work.

Coming back to the transformation of industries, Gelsinger says "it's all about the app"---using apps to help businesses transform their industries through technology and software. This brings him to reviewing VMware's vision: any cloud, any application, any device. Using cloud and edge computing as a framework, Gelsinger talks about the pendulum between centralization and decentralization. IoT and edge computing are just the latest result of this constantly-swinging technology pendulum, according to Gelsinger.

Digging in a bit deeper into VMware's any cloud-any application-any device vision, Gelsinger talks about VMware's goal to support any application across any modality of access, and this leads him to a mention of Workspace ONE. Workspace ONE comprises three areas: apps and identity; management and security; and desktop and mobile. Gelsinger announces a deepening of the relationship between HP and VMware around VMware Unified Endpoint Management solution will be integrated into HP's device-as-a-service platform. (I'm not really sure what this means, but it seems significant.)

At this point, Gelsinger brings out Sanjay Poonen (of VMware) and Jennifer Manry of Capital One to talk about Workspace ONE and end-user computing. There is an obligatory "What's in your wallet?" reference, but the discussion focuses on how Capital One sees itself as a technology company that is also a bank. Manry talks first and foremost about organizational transformation, which strikes me as very telling (that you can't ignore organizational transformation when adopting new technologies).

Gelsinger takes the stage again to shift the focus from end-user computing to cloud computing (albeit he focuses primarily on virtualization). The evolution of virtualization, according to Gelsinger, lead to the virtualization of the entire data center. However, there are still challenges, and Gelsinger announces VMware Skyline, a service to provide better service and support to customers. (He very much glossed over the announcement.) He also announces version 2.0 of VMware Cloud Foundation, which is a foundational (pardon the pun) technology for what VMware is doing not only in the private cloud but also in the public cloud. From Cloud Foundation, Gelsinger moves on to hyperconvergence---specifically, vSAN---which is seeing tremendous success and uptake in the market. And, not surprisingly, there's mention of Dell's VxRail and VxRack offerings.

Gelsinger talks briefly about vSphere 6.5 and the numerous improvements there, but quickly shifts his focus to the public cloud. He brings out Andy Jassy, CEO of Amazon Web Services, to the stage. With Jassy on the stage, Gelsinger announces the immediate availability of VMware Cloud on AWS. Jassy talks a bit about the journey that led VMware and AWS to the deep partnership that led to the VMware Cloud on AWS announcement, and talks a bit about the unique value of VMware Cloud on AWS. Together, Gelsinger and Jassy talk about a few customers who have been successful early customers for VMware Cloud on AWS.

What about the future of VMware Cloud on AWS? Gelsinger talks about the goal to have VMware Cloud on AWS _globally available_ by the end of next year. Jassy reinforces that the roadmap for VMware Cloud on AWS will be driven by customer feedback and customer demand. Jassy states that this partnership is more than just a marketing partnership---there's actually deep engineering collaboration occurring.

VMware is delivering on its vision (any cloud, any app, any device) in three ways:

1. Make private cloud easy.
2. Build deep relationships with major cloud providers.
3. Expand the VMware Cloud Partner Network.
4. Build a portfolio of native cloud services.

This leads Gelsinger into a discussion of VMware Cloud Services, which aims to provide consistent operational environments across VMware-based and non-VMware-based clouds. Today, Gelsinger announces the availability of the first seven VMware Cloud Services (Discovery, Cost Insight, Wavefront, Network Insight, NSX Cloud, AppDefense, and Workspace ONE).

At this point, Gelsinger brings out Medtronic. Karine Semmer is the customer representative for Medtronic, and Poonen again helps facilitate the conversation. Semmer talks for a few minutes about how Medtronic is having to change its business and how VMware products have helped in Medtronic's efforts.

Gelsinger takes the stage again now to dig into networking (one of my favorite topics). VMware NSX is at the heart of everything that VMware is doing, according to Gelsinger, becoming a "connective tissue" that ties everything together. NSX is stretching into the public cloud (via NSX Cloud), stretching into cloud-native app frameworks (teaser about an announcement tomorrow), stretching into the core telco networks. NSX is a "secret sauce" for VMware, says Gelsinger.

Gelsinger brings Sysco onto the stage, again with Sanjay Poonen, to talk about NSX. Matt Nikolaiev with Sysco joins Poonen on stage to talk about networking with NSX. Nikolaiev talks about the company needed greater agility, and felt that VMware NSX has really accelerated their ability to provision networking services and enhance Sysco's security via microsegmentation. He also makes mention of integration between Workspace ONE and NSX, which has been quite useful for Sysco. Nikolaiev is especially excited by the multi-cloud possibilities that NSX enables.

Gelsinger returns to the stage, and changes the focus on his presentation to security. The security industry is incredibly large and complex, keeping systems secure is very difficult, and data breaches are too costly. Gelsinger says that there are three parts to fixing this issue:

1. Secure the infrastructure.
2. Deeply architect in controls, context, capabilities, and functionality in conjunction with the ecosystem.
3. Return to the basics of cyber-hygiene.

Talking about cyber-hgyiene, Gelsinger says there are 5 pillars to cyber-hygiene:

1. Least privilege
2. Microsegmentation
3. Encryption
4. Multi-factor authentication
5. Patching

These simple principles are being discussed in more details in a white paper being released by VMware today. Gelsinger refers to government support from the US federal government (Congress, specifically) in the form of bipartisan legislation to support cyber-hygiene.

Returning to securing the infrastructure, Gelsinger talks about Workspace ONE, AirWatch, VMware NSX, and vSAN Encryption. That addresses users, devices, networking, and storage---but what about compute? In Gelsinger's words, "Today all that changes." This leads him into a discussion of the difficulty in "chasing bad" to find malicious software. What about "ensuring good" instead of "chasing bad"? it's flipping the security model on its head. This is what VMware AppDefense does. AppDefense captures intent, detects unwanted behavior, and reacts.

At this point, Marc Van Zadelhoff from IBM Security joins Gelsinger on the stage as part of the AppDefense discussion. Zadelhoff talks about how AppDefense is not only good for helping with cyber-hygiene, but it also is a huge boon for security analytics (like IBM Security). Zadelhoff announces a partnership between IBM Security and VMware that will bridge AppDefense and IBM Security's functionality to provide greater context, more analysis, and the ability to take action. The app is expected to be finished by the end of the year.

Gelsinger talks about how Tom Corn will do a live demo of AppDefense in the first Spotlight session after this morning's keynote.

The discussion comes back now to VMware's vision (any cloud, any application, any device). As a means of tying everything together and how it enables us to make an impact, Gelsinger introduces American Red Cross and shows a quick video about the impact of American Red Cross. Following the video, Poonen and Dave Bullamore of American Red Cross take the stage.

The first topic of their discussion is, naturally, Hurricane Harvey. Bullamore talks about the work that American Red Cross is doing right now in Texas and Louisiana, and not just now in the months to come. Bullamore talks about the employees _and_ the volunteers of American Red Cross (20K employees and 300K volunteers). Shifting a bit to talk about technology, Bullamore talks about how American Red Cross is a very mobile organization, and the use of various VMware technologies (Workspace ONE, AirWatch, etc.) have enabled American Red Cross has been quite powerful for American Red Cross. Finally, Poonen and Bullamore wrap up with a quick discussion of how VMworld attendees can help the American Red Cross.

Poonen stays on stage following the discussion with the American Red Cross to talk about how VMworld's content is structured around feedback from customers and CIOs. The three focus areas are cloud, mobile, and security. From these three focus areas, there are four tracks: modernize data centers, integrate public clouds, empower the workforce, and transform security (I might be off on those last two topics, the presentation moved on too quickly).

Poonen talks about 200 customers presenting this week, and its his goal to drive that number even higher for future events. Tomorrow's general session will have a "fireside chat" between Michael Dell and Pat Gelsinger, and Poonen alludes to a very special announcement happening tomorrow.

At this point, the general session wraps up.



[link-1]: https://www.vmware.com/company/news/releases/vmw-newsfeed.VMware-Advances-Software-to-Help-Customers-Modernize-Data-Centers.2184711.html
[link-2]: https://www.vmware.com/company/news/releases/vmw-newsfeed.VMware-Advances-Software-to-Help-Customers-Modernize-Data-Centers.2184711.html
[link-3]: https://www.vmware.com/company/news/releases/vmw-newsfeed.VMware-Helps-Enterprises-Succeed-in-the-Multi-Cloud-Era.2184714.html
