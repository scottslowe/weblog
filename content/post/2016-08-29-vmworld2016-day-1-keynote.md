---
author: slowe
categories: Liveblog
comments: true
date: 2016-08-29T00:00:00Z
tags:
- VMware
- VMworld2016
- NSX
- AWS
- Networking
- Security
- Encryption
title: VMworld 2016 Day 1 Keynote
url: /2016/08/29/vmworld2016-day-1-keynote/
---

This is a liveblog of the day 1 keynote at VMworld 2016 in Las Vegas, NV. I managed to snag a somewhat decent seat in the massive bloggers/press/analysts area, though it filled up _really_ quickly. Based on the [announcements made this morning][link-1], it should be a great general session, and I'm really interested to see how its received by the community.

The keynote starts with a high-energy percussion/DJ session, followed by a talk about tomorrow---from where tomorrow will emerge, what tomorrow will look like, and what tomorrow will care about. Don't stand in line for tomorrow; you _are_ tomorrow, because tomorrow is about people. Which way will you face? What will you do to bring about tomorrow? All of this lines up, naturally, with VMworld's "_be_\_Tomorrow" theme.

After that talk Pat Gelsinger, CEO of VMware, takes the stage. He talks briefly about his foot injury, then thanks the 21 "Alumni Elite" who have attended every single VMworld. Gelsinger then moves into a discussion of buzzwords and "digital transformation," claiming that _all_ businesses are digital businesses, and therefore _all_ businesses need to worry about the challenges that face digital businesses. Gelsinger talks about a couple companies that have really embraced innovation: GE and CVS Pharmacy. These are examples of companies that are really leading in "digital innovation" and engaging with customers in new and innovative ways. Digital leaders aren't just identified by their technology, but also by their culture.

Gelsinger next transitions into a discussion about cloud computing, and how cloud computing has evolved and grown over the last 10 years (he contends 2006 is considered the year cloud computing was born). As of 2006, 2% of workloads were in public cloud, and this largely represents Salesforce.com. As of 2011 7% are in public cloud, 6% in private cloud, and the rest in "traditional IT." As of today, 15% of workloads are in public cloud and 12% are in private cloud. It's estimated that it will be 2021 before the 50/50 mark of workloads are clouds (public or private). As of 2030, more than 50% of all workloads will be in the public cloud.

Gelsinger is quick to point out, though, that "managed cloud"---in particular, services represented by vCloud Air and vCloud Air Network partners---still have lots of opportunity ahead of them.

On the device/access side, Gelsinger believes the market is now becoming saturated, predicting that the industry will move from 8.2 billion devices this year to 8.7 billion devices in 5 years. The "Internet of Things" (IoT), though, will see massive growth, reaching an estimated 18 billion devices in 2021. This represents, in Gelsinger's opinion, an enormous opportunity.

Gelsinger next contends the the rise of public clouds won't cause a reduction in IT investment---instead, it will cause an expansion. This is Jevons paradox at its finest: as services become more broadly accessible, then the demand/consumption for those services will expand. This is illustrated by a wide variety of industries embracing cloud, as well as every department/function within organizations embracing cloud (aka "shadow IT", now called self-starting IT).

Gelsinger then points out that although IT no longer controls the devices, the applications, or the networks, IT is still held responsible for security breaches. This is a fundamental dichotomy, the conflict between freedom and control. This naturally leads Gelsinger into a discussion of hybrid environments, including both private and public cloud, and the vision of the software-defined data center (SDDC). Gelsinger points out that it was Raghu Raghuram that "predicted" SDDC in 2011 at VMworld EMEA in Copenhagen, and then proceeds to see how Raghuram's prediction has played out over the last 5 years.

This leads into a video from a number of customer testimonials talking about VMware's SDDC products have enabled them to meet their business challenges. Following these customer testimonials, Gelsinger announces VMware Cloud Foundation and VMware Cross-Cloud Services, and talks about the first partner for providing VMware Cloud Foundation as a service: IBM. With that, Gelsinger brings Robert LeBlanc, SVP at IBM Cloud, on to the stage. Gelsinger and LeBlanc talk for a few minutes about how offering VMware Cloud Foundation as a service is important to customers. LeBlanc brings Alan Rosa, SVP at Marriott, onto the stage to talk about their use of IBM and VMware's products and services.

After a few minutes of discussion between Gelsinger, LeBlanc, and Rosa, Gelsinger shifts the focus from VMware Cloud Foundation to VMware Cross-Cloud Services (which, in my mind, is far more important and impactful). Gelsinger next brings Guido Appenzeller, CTSO of Networking and Security at VMware. (Disclaimer: he's also my manager at VMware.) Appenzeller starts his discussion by talking about VMware Cloud Foundation---while good---is not enough. Organizations need more; they need security, compliance, cost management, etc., across multiple clouds (and including on-premises environments). And this isn't limited to just VMware-based clouds; it also needs to include non-VMware-based clouds. Appenzeller brings out Motti Finkelstein from Citi onto the stage to talk about cross-cloud architectures.

Finkelstein re-iterates the need to focus on the end-goal (very important!) when designing a solution to meet those needs. In some cases, those needs are met using on-premises models; in other cases, it's using public cloud services. This means that IT organizations need a hybrid environment with cross-cloud services. In other words, it's about multiple clouds. Appenzeller and Finkelstein talk about the need for security, compliance, management, access control, etc., and the complexity of tracking and managing that in a cross-cloud architecture or environment.

After Finkelstein leaves the stage, Appenzeller asks the question: why can't VMware offer services to help address these cross-cloud challenges? It turns out, VMware can---it's Cross-Cloud Services. Appenzeller switches over the demo, showing off a technical preview. Keep in mind that Cross-Cloud Services is a set of functionality offered "as a service." The demo shows the discovery of workloads running in AWS, what their costs are, what sorts of applications are running in those instances, etc. Appenzeller then switches over to showing a set of security services that visualize how traffic is flowing among and between instances, as well as how traffic is flowing between the instances and the public Internet.

This leads Appenzeller into a discussion of networking and security, and he invites John Spiegel from Columbia Sportswear. Spiegel re-iterates the message that the transformation to "cloud" is more cultural than technological, and he plugs _The Phoenix Project_ as one way Columbia has helped with cultural transformation. Spiegel next talks extensively about the Security Insight services---based on the Arkin acquisition---and how Columbia has used it to fine-tune their firewall rules to ensure the correct security posture. This leads Appenzeller to switch back to the demo and show how NSX can protect AWS workloads, including onboarding existing workloads. Appenzeller also shows Distributed Network Encryption (DNE), discussed last year at VMworld, being applied using NSX to AWS-based workloads.

Following Spiegel and the NSX for AWS portion, Appenzeller brings out Josh Warsop at Johnson & Johnson. Appenzller and Warsop talk about the idea of a "borderless data center," which means using both on-premises workloads as well as running workloads and services provided by public cloud providers such as Amazon and Microsoft Azure. Warsop strongly believes the future is "many clouds." This leads into a demo by Appenzeller of using Cross-Cloud Services to migrate and clone AWS-based workloads among/across/between various regions and data centers.

After a brief wrap-up, Appenzeller introduces a customer testimonial from GE, who's using VMware's products and services across multiple clouds around the world to help solve IoT-related challenges and problems.

Gelsinger re-takes the stage. He recaps what's been shown in today's general session, and gives a quick preview of what to expect in tomorrow's general session (which will include some cloud-native stuff as well as some end-user computing technologies). Gelsinger next brings Michael Dell onto the stage. Dell re-iterates the need for an open ecosystem, and how the EMC acquisition won't affect or impact VMware's open ecosystem. Dell also re-iterates his excitment and support for VMware's Cross-Cloud Services and what this will mean for customers.

After a few minutes of discussion with Dell, Gelsinger reiterates the key messages from the general session, and wraps up his presentation.


[link-1]: http://www.vmware.com/company/news/releases/vmw-newsfeed.VMware-Unveils-New-Cross-Cloud-Architecture(TM)-to-Give-Customers-Cloud-Freedom-and-Control.2089557.html
