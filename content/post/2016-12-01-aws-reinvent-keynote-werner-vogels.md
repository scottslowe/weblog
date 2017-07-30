---
author: slowe
categories: Liveblog
comments: true
date: 2016-12-01T08:30:00Z
tags:
- AWS
- reInvent2016
title: AWS re:Invent 2016 Keynote with Werner Vogels
url: /2016/12/01/aws-reinvent-keynote-werner-vogels/
---

This is a liveblog of the Thursday keynote at AWS re:Invent 2016. Today's keynote is led by Werner Vogels, CTO of Amazon Web Services. Unlike yesterday, today I opted not to attend the keynote in the main hall, viewing the keynote instead from an "overflow" area. Turns out the "overflow" area has drinks, tables, and power! That's a far better option that being crammed in the main hall, though in the past I've found it more difficult to liveblog when not viewing the keynote directly. We'll see if that continues to hold true.

After an entertaining "remix" of Werner quotes in the pre-keynote music mix, Vogels takes the stage at 9:30. The remote viewing is, unfortunately, off-sync; the video doesn't match up to the audio. Vogels starts his keynote by looking back at the last 10 years, and seeing the sorts of transformations have occurred. He rails against the vendors, and how AWS vowed to be "the Earth's most customer-centric IT company." Vogels says customers should be in charge, not vendors, and that includes AWS.

How does AWS be a customer-centric IT company?

1. Listen closely to customers and act.
2. Give customers choice.
3. Work backwards from the customer.
4. Help customers transform.

The key with #1, according to Vogels, is the "and act" part; it's not enough to just listen to customers. You have to listen to customers _and act._ Vogels similarly expands on each of these points, highlighting how he believes AWS lives up to these guidelines. Overriding all of these, though, is the guideline to "protect customers at all costs."

Returning to the idea of transformations, Vogels calls out Twilio as an outstanding example of a success story. To talk more about Twilio's journey, Vogels brings out Jeff Lawson of Twilio. Lawson is the co-founder, CEO, and chairman of Twilio.

Lawson describes himself as a "software" person, explaining that software is a mindset not a skill set. Software agility enables competitive differentiation. However, communications is necessary for great customer experiences. Lawson's experience with communications, though, was not pleasant, and that led him to start Twilio in 2008. Lawson spends a few minutes talking about Twilio and Twilio's offerings. He wraps up with the quote: "You don't get points for using servers. You only get points for serving users."

Vogels takes the stage again, and talks about the three ways that AWS can help customers be transformers:

1. Development
2. Data
3. Compute

He starts with development, testing, and operations. Development is changing, according to Vogels. Companies have to reduce risk, move faster, build smaller and more targeted applications, deliver more quickly to customers, and react to customer needs. Companies need to not play it safe; Vogels quotes Jeff Bezos in saying "If you already know the outcome, it's not an experiment." Development and test, according to Vogels, are the _most important_ workloads for software-oriented companies (i.e., companies with a software mindset). Vogels points out the benefits of running dev/test on AWS: unconstrained access to resources; higher-fidelity testing; faster time-to-market; major productivity improvements; and significant cost improvements.

Vogels give a shout-out to the Twelve Factor App guidelines; he also points out AWS' Well-Architected Framework. The four pillars of the Well-Architected Framework are Security, Reliability, Performance Efficiency, Cost Optimization, and Operational Excellence. Vogels dives a bit deeper into the Operational Excellence aspect, which involves preparation, operation, and response.

In all these areas, one thing that keeps coming up again and again is automation. To improve security, to improve reliability, to improve efficiency, you need to use automation. Vogels says that CloudFormation is critical with regards to automation, and points out some major improvements in CloudFormation over the course of 2016 (new services, YAML support, cross-stack references, resource schemas, and deeper IAM integration, among others). While CloudFormation is important, a lot of customers are also leveraging Chef. This leads Vogels to announce a fully-managed Chef server called AWS OpsWorks for Chef Automate, which is generally available today. Vogels also announces EC2 Systems Manager, which is a collection of AWS tools to help with package installation, patching, resource configuration, and task automation.

Vogels shifts his discussion slightly to talk about continuous integration/continuous delivery (CI/CD). He states that many customers who want to move faster have moved to CI/CD models. In a CI/CD pipeline, you have source environments, build environments, and deployment environments (staging, pre-production, and production). AWS already has some tools in this space: AWS CodeCommit (for source environment) and AWS CodeDeploy (for deployment environments), linked together by CodePipeline.

However, this wasn't enough. What about build environments? To address this gap, Vogels announces AWS CodeBuild, a fully managed build service for compiling source code and running unit tests. As with so many other AWS services, you only pay for what you use.

Monitoring and managing your environment can be challenging, and Vogels talks about how CloudWatch (Metrics, Events, and Logs) and CloudTrail have been important in providing the necessary visibility to monitor and manage your environment. What about deeper visibility, deeper insight, into application and service execution? In this space, Vogels announces AWS X-Ray. X-Ray is a fully-managed service to analyze and debug distributed applications in production. X-Ray is available today as a preview (no GA date mentioned). Vogels proceeds to give a quick preview of what X-Ray looks like, showing some screenshots of the functionality this new service provides.

At this point, Vogels shifts the discussion again to talk about how AWS users can respond to customers or respond to events/actions/metrics. Once again, automation is very important here. Vogels announces the AWS Personal Health Dashboard, a personalized view of AWS service health. Customers can write Lambda functions to automatically respond to events coming from AWS Personal Health Dashboard, making this feature extensible and enabling automatic responses.

Coming back to security, Vogels re-iterates his belief that organizations need to make protecting customers their #1 priority. Security by design is critical. Automation is security is essential, according to Vogels. While Amazon offers a number of services to help with secure-by-design architectures as well as security automation, a big concern for customers is the distributed denial of service (DDoS) attack. Vogels walks through the various types of DDoS attacks (volumetric, state-exhaustion, application-layer). According to Arbor Networks, volumetric attacks are the biggest percentage, followed by state-exhaustion and then application-layer attacks. To help address this, Vogels announces AWS Shield for Everyone. This protects against all volumetric and most state-exhaustion attacks. This is turned on by default for all customers, and is generally available today. To help with more sophisticated attacks, AWS Shield Advanced is available. This is also generally available today. AWS Shield Advanced also includes cost-protection measures for services like ELB and WAF.

Vogels now introduces Chris Turvil, Head of Cloud and Platform Agility from Trainline. Trainline is the #1 train ticket retailer in Europe (I'd never heard of them before). Turvil spends a few minutes sharing how his organization has transformed over the last 18 months.

Returning to the stage, Vogels turns his attention to data. (Recall that he said he was going to focus on development, data, and compute earlier in the keynote.) Vogels sees data as a competitive differentiator. Naturally, AWS can help there with data processing, data warehousing, predictive analytics, reporting, real-time processing, etc. Vogels spends a few minutes walking through the various AWS services related to data: EMR, Athena (announced yesterday), Redshift, QuickSight, Elasticsearch, Amazon AI, and Amazon ML.

Vogels announces Amazon Pinpoint, a new service intended to help customers fine-tune interaction and engagement with customers via mobile connectivity.

To help talk about using data to transform, Vogels brings out DJ Patil, Chief Data Scientist for the White House. Patil talks about how users now expect real-time traffic and mapping, on-demand services, one-day shipping, and instant news delivery. The mission of Patil's office is to responsibly unleash the power of data for the benefits of all Americans.

Vogels returns to the stage, pointing out that 80% of what we call analytics is just hard work (and isn't analytics). Things like data acquisition, storage, security, access, governance, etc., aren't analytics, although they are often lumped into analytics. What are the components of a modern data architecture?

1. Automated data ingestion
2. Preservation of original source data
3. Lifecycle management and cold storage
4. Metadata capture
5. Managing governance, security, and privacy
6. Self-service discovery, search, and access
7. Managing data quality
8. Preparing for analytics
9. Orchestrating and job scheduling
10. Capturing data change

The reality is that all of your data sources are sitting in different silos, and this will be the case for quite some time to come. Any solution needs to deal with all these different silos. Can one address the modern data architecture with AWS? Here's how AWS services map against the 10 steps listed earlier:

1. S3 upload, S3 acceleration, Kinesis, Snowball/Snowball Edge/Snowmobile
2. S3, EFS, EBS, DynamoDB, RDS
3. S3 storage management, Glacier
5. Config, CloudTrail, and KMS
7. EMR 

There are some missing pieces, and that leads Vogel to announce AWS Glue. Glue is a fully-managed data catalog and transformation tool. It can access data sources in the cloud as well as data sources on-premises, and then prepare that data for analytics. Finally, Glue can schedule and orchestrate analysis of the data. Vogels believes that the combination of Glue and other AWS services enables users to address all 10 aspects of a modern data architecture.

Along the way, Vogels mentioned some new features in S3. Those new features in S3 included data events in CloudTrail, object tagging, analytics, CloudWatch Metrics, and S3 Inventory.

At this point, Vogels introduces Eric Gunderson, CEO of Mapbox. Gunderson talks about how Mapbox is building a platform to allow developers to integrate mapping into their applications and services. Other companies like National Geographic, CNN, AirBnB, and more are using Mapbox and Mapbox's data and services.

Vogels takes the stage again, and shifts his focus to the compute aspect of his talk (he's already covered development and data). To help address the pain points of customers who are dealing with large-scale processing needs, Vogels announced AWS Batch (preview available today), a fully-managed batch processing service to enable batch processing at any scale.

Vogels points out that AWS offers a spectrum of compute options: VMs, containers, and "serverless" (FaaS). He spends a few minutes differentiating the key aspects between VMs, containers, and serverless computing. The VM ecosystem is very strong, with a strong set of supporting services (ELB/ALB, EFS, EBS, VPC, etc.). With containers, on the other hand, it's not quite as simple---it introduces some complexity that AWS was trying to remove from the hands of customers. Amazon ECS helps with that by "bringing back the power of cloud" for container-based workloads. ECS offers deep integration into the whole range of AWS services. A new task placement engine will be available soon that will allow you to create scheduling policy based on AMI, AZs, etc., and new task placement strategies (binpacking, spread, affinity, distinct) will also be available. But customers want more flexibility to build their own solutions.

To address this desire from customers, Vogels introduces Blox, a set of open source projects for building schedulers and services for ECS. Initially available will be a cluster state project that enables you to consume event streams, and a new scheduler for scheduling daemons. More open source projects under the Blox banner are coming.

At this point, Neil Hunt of Netflix takes the stage at Vogels' introduction. Hunt talks for a few minutes about Netflix (which is, by this time, probably a very well-known story). Hunt shares how Netflix plans to adopt ECS in the near future.

As Vogels takes the stage, he turns his attention to Lambda, and offers a quick review of Lambda's features and functionality. Vogels announces C# support for Lambda, adding that language to Python, Java, and JavaScript as supported languages with Lambda. Vogels extends the pets-vs-cattle meme; in the serverless world, there are no cattle---there is only the herd. Customers are not only leveraging Lambda extensively; AWS itself is using Lambda as an extension mechanism all over AWS with a wide variety of AWS services.

Vogels announces a new extension to the use of Lambda, adding Lambda support to CloudFront via Lambda@Edge. This enables you to perform Lambda functions at the CloudFront POPs, reducing RTT time and improving latency for web-based applications. To help coordinate different functions in a Lambda function, Vogels also introduces AWS Step Functions, a way to coordinate hte components of a distributed application. Using a graphical/visual editor, you can combine different Lambda functions and coordinate those functions to operate them together (sequentially, parallel, and branching models are all available).

Following a brief recap of his announcements and his presentation, Vogels wraps up the keynote.
