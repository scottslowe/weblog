---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-07T21:00:00Z
tags:
- OpenStack
- AWS
title: Can OpenStack Beat AWS in Price
url: /2017/11/07/can-openstack-beat-aws-in-price/
---

This is a liveblog of the session titled "Can OpenStack Beat AWS in Price: The Trilogy". The presenters are Rico Lin, Bruno Lago, and Jean-Daniel Bonnetot. The "trilogy" refers to the third iteration of this presentation; each time the comparison has been done in a different geographical region (first in Europe, then in North America, and finally here in Asia-Pacific).<!--more-->

Lago starts the presentation with an explanation of the session, and each of the presenters introduce themselves, their companies, and their backgrounds. In this particular case, the presenters are representing Catalyst (runs Catalyst Cloud in New Zealand), OVH, and EasyStack---all three are OpenStack-powered public cloud offerings.

Lago explains that they'll cover three common OpenStack scenarios:

* Private cloud
* Managed private cloud
* OpenStack-powered public cloud

Lin takes point to talk a bit about price differences in different geographical regions. Focusing on AWS, Lin points out that AWS services are about 8% higher in Europe than in North America. Moving to APAC, AWS services are about 29% higher than in North America. With this 29% price increase, I can see where OpenStack might be much more competitive in APAC than in North America (and this, in turn, may explain why OpenStack seems much more popular in APAC than in other regions).

Lago takes over now to set some assumptions for the comparisons. He provides an overview of the total cost of ownership (TCO) model used; the presenters are also making available a Google Docs spreadsheet that implements the TCO model and their assumptions. This includes things like hardware costs, power costs, number of full-time employees (FTEs) used to support OpenStack, etc. All this information is used to derive the per-vCPU cost is when running workloads on OpenStack; this enables (hopefully) fair comparisons with AWS.

This leads Lago to share a slide that compares various EC2 m4 instances to comparable OpenStack instances; in all cases, the OpenStack instance was far less expensive than EC2 (again, this comparison is based on a number of assumptions, such as a quite large cloud).

Bonnetot now takes the lead to talk about the OpenStack public cloud use case (comparing AWS against an OpenStack-powered public cloud). The comparison here was done using the AWS Simple Monthly Calculator and then compared against matching (as close as possible) OVH public cloud resources. Bonnetot recognizes that these comparisons can't be perfect due to differences between providers, and that every effort was made to try to equalize differences between providers and their offerings. Lin also takes a few minutes to talk about how they tried to account for differences in services and guarantees between providers.

Coming finally to the comparison---based on a pretty standard three-tier application architecture---Lin shows that AWS runs about $11K a year, OVH comes in at about $9K a year, and private OpenStack cloud comes in just over $3K a year. Lin drives into a few additional details, noting that additional "sysadmin" time is added to OVH and OpenStack amounts to account for the lack of managed services that are present on AWS. In APAC, the AWS price runs up to almost $15K (representing that 29% premium for AWS services in APAC regions).

A second comparison---based on a back-end for a mobile application---the AWS price is around $27K, compared to $18K for OVH and $4K for an OpenStack private cloud. Lin calls out the cost of the bandwidth when using a public cloud provider in AWS, but I don't recall any of the presenters mentioned that they accounted for network costs and network services in their models. This may represent a gap in the comparison that unfairly skews the benefits in favor of private clouds. Additional details provided by Lin show that the APAC price for AWS is again about 30% higher (up to $36K).

Bonnetot steps forward to cover a third comparison, this time an example for a deep learning use case. This example discusses the use of GPU-equipped instances. In this example, AWS costs $167K annually, compared to $126K annually for OVH (no OpenStack private cloud comparison was given here, I assume because of the hardware needs).

The last use case is an archival use case. In this case, the presenters discuss archiving 60TB of data in an object storage service (S3 versus Swift). In this case, OVH comes out first (around $1700), AWS second (just shy of $3K), and an OpenStack private cloud comes in last (just over $3K). Lago theorizes that the model may be due to some incorrect assumptions around hardware platforms and hardware configurations, and invites attendees to collaborate with them in fine-tuning the assumptions behind the model. In this last model, AWS again comes in at about 22% more expensive in APAC than in the US.

In conclusion, Lago steps up to remind attendees that it's not only about cost; it's also about risk. At sufficient scale, a private cloud can be far more cost effective than a public cloud, but the risk of using a private cloud is higher (more staff required, more training, getting the company to use the cloud, etc.). Managed private cloud increases cost, but can reduce risk. Public cloud offers a far lower risk, but also typically has a much higher cost.

The presenters wrap up by providing a URL to the TCO spreadsheet ([http://bit.ly/2dFGvfQ](http://bit.ly/2dFGvfQ)) and again invite attendees to collaborate with them on fine-tuning the assumptions and the TCO model.
