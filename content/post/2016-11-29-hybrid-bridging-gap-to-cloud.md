---
author: slowe
categories: Liveblog
comments: true
date: 2016-11-29T11:00:00Z
tags:
- AWS
- reInvent2016
title: 'Liveblog: Hybrid Architectures, Bridging the Gap to the Cloud'
url: /2016/11/29/hybrid-bridging-gap-to-cloud/
---

This is a liveblog of the AWS re:Invent session titled "Hybrid Architectures: Bridging the Gap to the Cloud" (ARC208). The line to get into this session, as with the previous session, was quite long---and that was for attendees who'd already registered for the session. Feedback I've heard from folks who weren't registered for sessions was that they weren't getting in, period. The speaker for the session is Jamie Butler, Manager of Solutions Architecture at AWS (focused on state/local government).

Butler starts out by establishing some expectations---attendees should be familiar with regions, AZs (this is a 200-level talk), and will focus on hybrid use cases. Butler says there will be some demos along the way. This session will _not_ focus on the VMware announcement regarding VMware Cloud on AWS.

Butler then quotes Werner Vogels in saying that adopting cloud is not an all-or-nothing proposition. With that in mind, Butler transitions into a discussion of a particular customer example. In this case, the customer had Active Directory, a file server, and a bunch of Windows-based desktops connecting back to the file server for data access.

The first thing to tackle in a scenario like this is identity. Butler says you don't want to proliferate multiple identity definitions, and points attendees to IAM (Identity and Access Management) as the solution. IAM offers fine-grained access for AWS resources, offers multi-factor authentication for highly privileged users, and can integrate with corporate directories. IAM also supports federation via SAML or OpenID. Finally, AWS also offers a directory service in three different flavors (Microsoft Active Directory, Simple AD [Microsoft-compatible; leverages Samba], and AD Connector). This leads Butler into a (recorded) demo of creating a Microsoft AD instance on AWS. The demo recording is too small to read in many places, but it gives you an idea of what's involved.

Once identity is handled, the next step---in this particular customer example---is to move some data to AWS. Butler mentions S3 as potential solutions here, and points out the three tiers of object-based storage: S3 Standard, S3 Infrequent Access, and Amazon Glacier. The Snowball---which is an actual physical device---is another option for moving large amounts of data to AWS. The Snowball is very rugged, portable, and leverages 256-bit encryption. In this particular instance, the customer was using CommVault, and was able to leverage CommVault's S3 integration to back data up to S3 directly over the Internet. Butler also points out that the AWS Storage Gateway has a number of uses in hybrid use cases. The Storage Gateway's storage is backed by S3 and offers a standard iSCSI interface.

Once you have data and identity addressed, it's time to handle networking. At the core of the networking solution is Amazon's Virtual Private Cloud (VPC). VPC allows customers to leverage IP addressing schemes that fit into their own addressing schemes, and supports ACLs, stateful firewalling (security groups), route tables, etc. There are multiple options for connecting to VPC; in this particular instance, the customer decided to leverage Direct Connect. Direct Connect is a dedicated connection to the AWS network, at speeds of up to 1Gbps.

Butler then shifts the discussion to Amazon EC2 to offer compute service. In this particular use case, just shifting the file server from the customer's premises to AWS wasn't the best solution; while it would work, it would introduce some latency that might result in a poor user experience. Instead, for this use case, the solution involved a couple of things:

* A new Active Directory domain controller, tied to the AWS AD instance, using AD replication with on-premises AD domain controller. This gives local authentication both on-premises as well as in AWS.
* A second file server, running Windows Server 2012 like the on-premises file server, running on AWS. The two file servers will leverage Distributed File System (DFS) replication (DFS-R).

Another demo ensues; this demo shows everything that Butler has been discussing regarding duplicate AD domain controllers and replicating file servers. As before, the demo is a bit too small to be readable in many parts of the demo, but it's enough to get the point across.

When the demo concludes, Butler discusses the option of using the AWS Storage Gateway in the VPC (instead of on-premises). This would allow you to offload "cold data" to S3, reducing the amount of EBS storage necessary for the file server. The trade-off, of course, is performance; S3 performance is lower than EBS performance. The upside is a reduction in costs.

Butler shares some numbers; for a 20TB file server, the cost was just shy of $2400/month. Adding the Storage Gateway allowed the customer to reduce costs to just shy of $1400/month.

Adding Amazon WorkSpaces---which is essentially Amazon's VDI offering---gives the customer to enable "BYOD" models. Butler shows how the architecture looks with WorkSpaces, and then launches into a (recorded) demo showing WorkSpaces being added to the architecture shown in previous demos.

The resulting architecture employs a number of different AWS services:

* EC2 (for AD domain controllers and file server)
* WorkSpaces (for Windows-based desktops)
* S3
* Storage Gateway
* AD Connector (to link on-premises AD with AWS)
* VPC and Direct Connect

When it's all said and done, the architecture ends up costing (using on-demand instances) about $6K/month.

At this point, Butler starts discussing some other services that may have some applicability in hybrid use cases, and then wraps up the session.
