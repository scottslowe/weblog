---
author: slowe
categories: Liveblog
comments: true
date: 2016-11-30T10:30:00Z
tags:
- AWS
- reInvent2016
title: AWS re:Invent 2016 Keynote with Andy Jassy
url: /2016/11/30/aws-reinvent-keynote-andy-jassy/
---

This is a liveblog of the Wednesday keynote at AWS re:Invent 2016. Today's keynote is led by Andy Jassy, CEO of Amazon Web Services. The crowd gathered for the keynote is pretty immense, despite the availability of numerous overflow locations spread across the multiple re:Invent venues.

At precisely 9am, the DJ rocking the pre-keynote music leaves the stage and AWS welcomes Andy Jassy, CEO, to the stage. This is only the 5th re:Invent conference, and Jassy confirms that this year's attendance is 32,000 with another 50,000 listening in via the live stream.

Jassy starts with an update on the AWS business. As of Q3, AWS is a nearly $13B run-rate business with millions of active customers. Jassy says that nearly every industry segment is using AWS in a "meaningful way," as is the public sector. He also calls out all the various AWS partners and systems integrators that have built practices on top of AWS, and the "thousands" of ISVs that have built (or rebuilt) products to run on AWS. AWS is, according to some statistics provided by Jassy, the fastest-growing enterprise IT technology company.

In 2014, AWS said the cloud was the "new normal." In 2015, AWS said cloud gave customers "control over your own destiny." This year, Jassy says that the cloud gives you "superpowers." The first of these is supersonic speed, which refers to the speed with which customers can both scale up (as well as scale down) very quickly and deploy workloads very rapidly. In support of this point, Jassy talks about AWS' extensive regions, availability zones, and the plethora of application services that AWS offers.

Not only is the business growing rapidly, but the platform is growing rapidly as well; Jassy indicates that AWS is on pace to launch about 1,000 new features in 2016 (up from 722 last year and 516 in 2014).

At this point, Jassy introduces Fabio Veronese, Head of Infrastructure and Technology Services at Enel. Veronese talks about changes in the energy industry, and how Enel is embracing global change. One way Enel is embracing change is a large-scale migration to public cloud services on AWS. As a result of their migration, provisioning time was reduced; power savings resulted; and Enel saw significant storage savings. Veronese indicates that Enel is committed to a 100% cloud operating model by 2018, and is embracing on an IoT initiative with AWS.

Veronese turns the stage back over to Jassy. Jassy picks up from the Enel presentation by focusing not only on AWS' breadth of services, but the depth of features offered by these services. After talking briefly about RDS and IAM, Jassy focuses on compute offerings (EC2). He walks through the spectrum of compute offerings---a total of nine instance families. Jassy offers some updates on where things stand:

* In the T2 (burstable) family, Jassy introduces two new instances: the t2.xlarge and the t2.2xlarge, to offer a larger memory footprint.
* In the R family, Jassy introduces the R4. R4 instances have twice as much memory, faster memory, larger L3 cache, and twice as many vCPUs.
* In the I/O intensive family, Jassy introduces the I3 (coming soon). The I3 instances will have 9x the IOPS, 2x the memory, 2.3x the storage, and 2x the number of vCPUs.
* In Q1, the C5 instance family will be available. The C5 family will double the number of vCPUs, performance, memory, and overall throughput.

Jassy now turns his attention to GPU-based workloads. He announces Elastic GPU for EC2, which allows customers to attach GPU to any of the nine instance families, much like you can attach EBS block storage volumes to an instance.

Going back to the loads of customers who are running T2 instances, Jassy introduces Amazon Lightsail. Lightsail is a virtual private server (VPS) offering, which is intended to make it super-simple to turn up instances on AWS. Lightsail packages will start at $5/month.

At the other end of the EC2 instance family spectrum (opposite the burstable T2 instances) are the GPGPU-powered P2 instances. With the idea of hardware acceleration in mind, Jassy introduces an entirely new instance family, the F1 family. The F1 instances are powered by FPGAs (field-programmable gate arrays). A preview of F1 instances is available today; general availability will come later (no estimated date provided).

Coming back to the "superpower" theme, Jassy talks about how AWS gives you "X-ray vision." He takes a few direct stabs at Oracle and Larry Ellison as he discusses how the cloud allows you to verify "outlandish" claims made by other vendors. Turning serious, Jassy moves his attention to how AWS analytics offerings give users better insight into their customers, their business, and their data. While Amazon Redshift and Amazon EMR are very powerful, Jassy introduces a new service: Amazon Athena. Athena is intended to make it easy to analyze data in S3 (logs, for example) using standard SQL. Athena attempts to address a need for customers who just want a simple way to run _ad hoc_ queries against data already stored in S3, and is not intended to displace or replace Redshift or EMR.

Continuing the discussion around X-ray vision, Jassy talks about how customers seek to find meaning in their data, and talks about machine learning. After speaking about the extensive work Amazon has done with deep learning/machine learning/artificial intelligence, Jassy introduces three new services under the banner of Amazon AI:

* Amazon Rekognition is an image recognition service that will allow customers to pass images into the service, and get out information about the objects in the image (a person, a woman, a car, a steering wheel, how many people, etc.), as well as facial recognition as well as sentiment analysis.
* Amazon Polly, a text-to-speech service. Customers provide text, Polly provides an MP3 (audio) stream. Polly has intelligence, like the ability to recognize abbrevations (WA) and substitute the full word (Washington). Polly will have 47 different voices in 24 different languages.
* Amazon Lex (it's what's inside Alexa). Lex is a natural language understanding and speech recognition service. Lex can accept either text or voice, and then can trigger Lambda functions, have multi-step conversations, have connections back to enterprise software/packages.

This leads into a demo of Lex. Jassy introduces Dr. Matt Wood, GM of Product Strategy at AWS, to lead the demo of the new Amazon AI services. Wood demonstrates both a voice-driven interface (similar to how you might interact with Alexa today) and a text- and image-driven interface (similar to how might interact via Slack or instant messaging).

Jassy takes over again following the Amazon AI demo, and moves on to talk about the third "superpower" that AWS gives customers, which is "immortality." How do companies live forever? By being prepared to address the ongoing change in and evolution of technology. Leveraging the power, flexibility, and cost of the cloud enables companies to be prepared to address change and evolution. Jassy announces that Workday is moving all of its workloads to AWS. To help discuss this migration, Jassy introduces Aneel Bhusri, Co-Founder and CEO of Workday.

Bhusri talks for a few minutes about the origins of Workday and Workday's core values (take care of employees and take care of customers). After a few minutes, Bhusri brings out David Clarke, SVP of Technology and Development at Workday. Clarke re-iterates the announcement from Jassy and Bhusri that all of Workday's applications will run on AWS. This decision, according to Clarke, comes after a lengthy and thorough evaluation of the available options. The Workday migration to AWS will start in Canada, with US and Europe being available later on.

Jassy takes the stage again to talk about enterprise transformation as part of his discussion about the immortality "superpower." As an example of enterprise transformation, Jassy introduces Tom Gergets, CTO of McDonald's Corporation. (Yes, _that_ McDonald's.) Gergets talks about their significant investment in technology to satisfy customers' changing demands and expectations. Unfortunately, there were lots of snickers and giggles in the audience, detracting from Gergets' talk about their technology journey.

The fourth "superpower" that Jassy discusses is flight. Flight represents freedom, including---among others---the freedom to ditch hostile database vendors. In particular, Jassy calls out Amazon Aurora. Aurora is completely MySQL-compatible, offers up to 5x the performance of MySQL, and is one of AWS' fastest-growing services. In the last year, more than 14,000 databases have migrated to Amazon Aurora.

To discuss their journey, Jassy introduces Steve Randich, SVP and CIO of Finra. Randich takes a few minutes to talk about Finra (a Wall Street regulator) and the catalyst(s) that drove Finra to partner with AWS.

Returning to the stage, Jassy praises Finra for their work with AWS. Finra is moving their database(s) to Aurora, but the number 1 customer request for Aurora is: can't you make it Postgres compatible? Jassy announces PostgreSQL for Aurora, making Aurora fully compatible with both MySQL and PostgreSQL at a tenth of the cost. AWS is also making it super-easy to move to Aurora from RDS.

The last superpower that Jassy will use in his keynote is shape-shifting. What this means is hybrid, and Jassy points out the features that AWS has introduced over the years to support hybrid architectures: VPC, Direct Connect, Storage Gateway, various tools, identity federation, etc. Not unsurprisingly, Jassy quickly starts discussing VMware as the _de facto_ leader for on-premises solutions, and how customers have been clamoring for VMware and AWS to partner in some way. This brings Jassy to the VMware Cloud on AWS announcement, which allows customers to leverage VMware's software stack on hardware in an AWS data center. At this point, Pat Gelsinger, CEO of VMware, takes the stage with Jassy.

Gelsinger and Jassy talk for a few minutes about the value of the VMware Cloud on AWS partnership.

After Gelsinger leaves the stage, Jassy turns his attention to the definition of "on-premises infrastructure." Unlike the overly-obvious definitions of the superpowers, this time Jassy provides a helpful clarification on how on-premises infrastructure will increasingly (over time) refer more and more to sensors and devices---aka the "Internet of Things" (IoT). However, this is going to present a number of challenges, and to help address some of these challenges. The answer is AWS Greengrass, which embeds AWS Lambda and other AWS services inside IoT hardware/devices. Jassy and the AWS team believe that Greengrass will address some of the limitations and inflexibility that the current IoT model presents.

Moving on, Jassy discusses Snowball, a method for migrating significant amounts of data to AWS. Demand for Snowball has been tremendous. Built based on customer feedback, Jassy announces Snowball Edge. Snowball Edge has increased capacity versus Snowball (100TB versus 50TB), integrated device encryption, clustering capabilities (sharding data across multiple devices), an S3 endpoint (allowing you to sync data with S3 while you're loading it), and Greengrass inside (the equivalent of an m4.4xlarge inside). Anticipated use cases for Snowball Edge include storing sample data at remote locations, manufacturing use cases, and patient imaging in hospitals. Another example might be to put a Snowball Edge on an airplane, gathering massive amounts of data from sensors all over the plane. The Edge can do a limited amount of processing in real-time, and after the Edge is full customers can ship it off to AWS for uploading and access to deeper analysis tools.

What about really, _really_ large data sets? What about exabytes of data? At that point, a truck---a real, live truck---drives into the expo hall. Jassy introduces AWS Snowmobile. Snowmobile is a 100PB (petabyte) container, affixed to a 45' long container (no, not a Docker container!). It comes with its own power, and you connect it to your data center using a supplied fiber cable. According to Jassy, this would enable customers to move an exabyte of data to AWS is about 6 months (down from 26 years using a 10Gbs connection).

At this point, Jassy starts to prognosticate about the next 10 years, and then wraps up the keynote.
