---
author: slowe
categories: Liveblog
comments: true
date: 2016-11-30T15:30:00Z
tags:
- AWS
- reInvent2016
title: 'Liveblog: How News UK Centralized Cloud Governance'
url: /2016/11/30/centralized-governance-policy-mgmt/
---

This is a liveblog of the AWS re:Invent session titled "How News UK Centralized Cloud Governance Using Policy Management" (DEV306). The speakers for the session are Joe Kinsella from CloudHealth Technologies and Iain Caldwell of News UK/News Corp EMEA.

Kinsella kicks things off by indicating that the session will attempt to tackle the burning question: how does one maintain the agility that brought you to the cloud in the beginning, but enforce the proper level of governance and control? Kinsella and Caldwell then spend a few minutes on introductions before diving into the content of the session.

Caldwell starts off the session content with a review of News Corp's use of AWS. News UK is currently running 69% of their workloads in the public cloud, with an aim to hit 75% by July 2017. Before they started their journey to the public cloud, News Corp ran a "global application assessment"---and Caldwell believes that this was _critical_ to the success News Corp/News UK has seen so far. News is using a wide variety of AWS services: EC2, S3, VPC, Direct Connect, Route 53, CloudFront, CloudFormation, CloudWatch, RDS, WorkSpaces, Storage Gateway.

When prompted by Kinsella, Caldwell indicates that EC2 instances were the biggest challenge for governance.

Kinsella now takes a moment to define governance, particuarly in terms of controlling cost, security, and compliance. When asked about governance, people are often ambivalent; when faced with concrete examples of what governance addresses, their stance often changes. However, they are concerned about governance not affecting the agility they've gained from moving to the cloud. Kinsella says that governance is a balancing act that must strike the delicate balance between agility and control/protection.

Governance in the cloud is, according to Kinsella, much more difficult. The speed of changes is three, sometimes four, orders of magnitude faster, and this makes governance quite challenging. Combined with powerful cloud-based servies and features, decentralized management, disparate management tools, and the need to integrate multiple products and sources of data, and cloud governance becomes very difficult, according to Kinsella. Caldwell shared that cost was a major driver for more effective governance for his organization.

So what are some common cloud governance issues?

* No tagging
* Reluctance to invest in Reserved Instances
* Reserved Instances left underutilized
* No right-sizing
* Elastic Load Balancing left unused
* Elastic Block Storage volumes left unattached
* RDS instances with no connections
* Leaving unused instances running 24x7

Organizationally, the ownership of cloud infrastructure is often handled by various lines of businesses, and IT is increasingly relegated to "influencer" instead of "owner". Caldwell echoes that this was definitely an issue for News Corp/News UK.

So where does an organization start when working on governance?

1. Establish a strategy and obtain stakeholder buy-in.
2. Evaluate and implement tool strategy.
3. Identify deliverables by stakeholder.
4. Implement, rinse, and repeat.

With regards to establishing a strategy, it's again a matter of a balancing act, and making decisions along different axes. For example, when it comes to cost an organization might be more willing to endure risk, but around security the same organization could be more risk-averse. It's also important to make sure that you have the right people to support your strategy.

The "cloud steward" is an evolving role responsible for ongoing cloud governance and optimization. Caldwell shares that this sort of role is something that News Corp/News UK is advocating within their own organization.

With regards to tools, Kinsella and Caldwell share a few different tools that News Corp/News UK are using:

* AWSGO - Enforcing instance shutdowns, snooze/suspend, and start
* Scripts to delete volumes that have been unattached for greater than 5 days
* CloudHealth (Kinsella's company)
* Consigliere
* New Relic (APM)
* Rundeck
* Puppet
* Slack integration

It's important to note, as pointed out by Caldwell, that this process of governance is a _journey,_ not a _destination._ In other words, this is an ongoing thing, something that has to be done on a regular basis. It's not a one-time task. This is what's encapsulated in step #4 ("Implement, rinse, and repeat").

At this point, Caldwell shares a few things that News Corp/News UK did during their governance/optimization efforts:

* Right-sized their instances
* Invested in Reserved Instances
* Decommissioned instances that they didn't need
* Implemented automation wherever possible
* Use the AWS Trusted Advisor service

Kinsella shares that the first 25-30% of cost savings can come relatively easily, but the next 20-25% can be more challenging.

With regards to security, Caldwell shared some ongoing tasks that News UK/News Corp has adopted:

* Regular reviews of network ACLs
* Regular review of security groups
* Audit and review of IAM roles
* Inactive users removed

What are some key metrics to determine if your efforts have been successful?

1. Adherence to architectural standards/controls
2. Increased efficiency/lifecycle management, TCO, ROI
3. Adherence to asset configuration standards
4. Compliance to security guidelines
5. Rate of adoption

Personally, I'm not sure that #5 is a meaningful metric in the absence of other metrics; for example, simply adopting new services without _also_ showing that the new services are cost-effective is not very helpful.

Five best practices that Kinsella shares with the audience:

1. Empower a centralized owner.
2. Don't give up on agility.
3. Create partnerships with strategic vendors.
4. Establish high-value policies.
5. Automate, automate, automate!

At this point, Kinsella and Caldwell wrap up and session and open up for audience questions.
