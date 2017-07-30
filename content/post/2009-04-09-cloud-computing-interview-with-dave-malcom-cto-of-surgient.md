---
author: slowe
categories: Interview
comments: true
date: 2009-04-09T18:08:40Z
slug: cloud-computing-interview-with-dave-malcom-cto-of-surgient
tags:
- Virtualization
- VMware
- Cloud
title: Cloud Computing Interview with Dave Malcolm, CTO of Surgient
url: /2009/04/09/cloud-computing-interview-with-dave-malcom-cto-of-surgient/
wordpress_id: 1301
---

I recently had the pleasure of conducting an e-mail interview with Surgient's Chief Technology Officer, Dave Malcolm. I've reprinted the question and answer session here. Enjoy!

## The Interview

**_Scott Lowe (SL): The term "cloud computing" is used in a variety of different ways to describe a variety of different things. How would you define cloud computing?_**

Dave Malcolm (DM): I believe cloud computing is defined as a pool of abstracted, highly scalable, and managed computing infrastructure capable of running end customer applications and is billed or charged by consumption. Therefore I think it has five primary attributes:

1. _Dynamic Computing Infrastructure -_ This is the foundation for the cloud and should include a standardized, scalable, secure, and highly available physical infrastructure. It also must be virtualized with server, storage, and network virtualization.

2. _Self-Service Based Usage Model -_ Interacting with the cloud requires a self-service oriented approach. Self-service clouds must be intuitive and easy to use. The capabilities should include the ability to upload, build, deploy, schedule, manage, and report on the users' business services, so they can manage the lifecycle of those services.

3. _Minimally or Self Managed -_ In order for an IT team or a service provider to efficiently provide a cloud for its constituents, they must leverage a technology platform that is self managed. Best-of-breed clouds enable self-management via software automation, leveraging the following capabilities:  

  * Deployment and recovery of business services and their computing resources (in the cloud)

  * Self-service scheduling and reserving resource capacity

  * Configuration, management, and reporting of resource capacity allocations

  * Access control and policies for usage of resources by users

4. _Business Service Centric -_ Cloud computing must be business service centric. This is in stark contrast to more traditional system- or server-centric models. In most cases, users of the cloud generally want to run some business service or application for a specific, timely purpose; they don't want to get bogged down in the system and network administration of the environment. They would prefer to quickly and easily access a dedicated instance of an entire business service---such as selecting to instantiate an ERP system in the cloud (even if it is a complex multi-server networked environment).

5. _Consumption Based Charging or Billing -_ Finally, cloud computing is usage-driven. Consumers pay for only what resources they use and therefore are charged or billed on a consumption-based model. Cloud computing platforms must provide mechanisms to capture usage information that enables chargeback reporting and/or integration with billing systems.

**_SL: Based on your definition of cloud computing above, what would you say are the top three challenges to organizations seeking to embrace the cloud computing model?_**

DM: The top challenges are that most organizations face when organizations seek to embrace the cloud computing model is the lack of expertise, tools, and processes to implement, operate, and maintain a cloud computing infrastructure.

_Tools_ - Most data center automation tools are not equipped to automate and manage resources in a dynamic computing infrastructure---they don't comprehend the transient nature of servers and the associated networks and storage elements. They don't have the ability to automate capacity management, enable self-service, or the instantiation of entire business services.

_Expertise_ - Most organizations do not have deep expertise in understanding the requirements of a cloud computing environment, the deep knowledge of the technologies required, and experience with those technologies. Most organizations are learning and have mastered some pieces of the puzzle, but no one I have come across has a comprehensive understanding and the full expertise necessary.

_Processes and best Practices_ - Most IT processes and best practices have been built around static physical infrastructures and the business services that have been implemented on them. Many of these processes breakdown when business services are instantiated, recovered, moved, and re-instantiated on the same resources within the cloud infrastructure. These processes were not designed to work in such a high change environment.

**_SL: Some industry experts have stated that interoperability and well-defined standards are the key to broad adoption of cloud computing. Do you agree or disagree? If you disagree, why?_**

DM: I don't really agree. I think that typically most disruptive technologies are adopted organically by innovators and early adopters. These risk takers try to solve real business problems with the technology and help it gain a level of maturity so that others, less risk oriented, can adopt the technology as well as enable it to gain mainstream market adoption. Cloud computing isn't any different. I believe via this organic adoption that _de facto_ standards emerge from those technology leaders and can become successful solutions. I don't see many examples of broad industry standards driving technology adoption from the tops down.

However, I do believe standards play a role. Once the technology has reached a level of mainstream adoption, there are usually several market leading vendors that emerge and have significant market penetration, but are likely not interoperable. This is where standards can provide the most value in bringing interoperability to solutions that have been successfully implemented and are delivering value. Trying to introduce standards too early in the market maturation process rarely gains any traction.

**_SL: What would you identify as the most promising developments around cloud computing today? You are welcome to include your own companys products, solutions, or technologies, but try to include other companies as well._**

DM: First off, I don't think we would be having this cloud computing discussion without the invention of x86 server virtualization pioneered by VMware. This is the key to the whole cloud computing dynamic infrastructure. Microsoft and Xen have followed with alternative hypervisors, so server virtualization is key.

Storage virtualization is also a significant component of the solution, so EMC, NetApp, HP, Hitachi, and others who are leaders in the storage market have also provided a significant element to the infrastructure.

Amazon should get credit for pioneering the idea of cloud computing with EC2, but this is far from addressing the needs of enterprise organizations and the complex business services they require to run their business on.

Finally, I have to mention Surgient. We also have been a pioneer in virtualization automation solutions, hosted clouds, as well as on-site private cloud implementations for many years. Our patented Surgient Virtual Automation Platform enables a virtualized infrastructure to become a cloud for enterprise uses via our automation capabilities---dynamic infrastructure orchestration, Policy-Driven Self-Service, self management, business service centric approach, and capacity usage management and reporting. In addition, because we work with 70 of the world's largest companies, with some of most complex computing needs, we have deep expertise in the implementation, operation, and maintenance of cloud computing infrastructures, and have developed significant process and best practices as well.

## Closing

If any other virtualization or cloud computing executives wish to conduct a similar interview, feel free to contact me.

Readers, if this is something you like, let me know by speaking up in the comments. Thanks!
