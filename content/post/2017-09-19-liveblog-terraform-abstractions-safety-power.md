---
author: slowe
categories: Liveblog
comments: true
date: 2017-09-19T14:00:00Z
tags:
- HashiConf2017
- Terraform
- AWS
- ECS
title: 'Liveblog: Terraform Abstractions for Safety and Power'
url: /2017/09/19/terraform-abstractions-safety-power/
---

This is a liveblog for the HashiConf 2017 session titled "Terraform Abstractions for Safety and Power." The speaker is Calvin French-Owen, Founder and co-CTO at Segment.<!--more-->

French-Owen starts by describing Segment, and providing a quick overview of Segment's use of Terraform. Segment is all on AWS, and is leveraging ECS (Elastic Container Service) to schedule containers. Segment's journey with Terraform started about 2.5 years ago. They now have 30-50 developers interacting with Terraform weekly, and Terraform is managing tens of thousands of AWS resources.

Digging into the meat of the presentation, French-Owens starts by answering the question, "Why is safety such a big deal?" There's more to the puzzle than just preventing downtime. To illustrate that point, French-Owens shares some conclusions from an academic paper that explores why developers choose software programs. It turns out that to scale adoption, you must reduce the risk of adoption (developers avoid programs based on risk).

Naturally, French-Owens talks about how Terraform can "feel scary" since it's so easy to destroy a bunch of infrastructure with only `terraform destroy`.

Before moving into a discussion on how to make Terraform feel less scary, French-Owens first covers some "Terraform nouns" (HCL, HashiCorp Configuration Language; variables; resources; inputs and outputs; and modules). Along the way, he shows examples of these nouns so that attendees who aren't familiar with Terraform can understand what he's discussing.

French-Owens shows how Terraform uses _state_ to be able to make the diff between your infrastructure's current state and the desired state (which is what happens when you run `terraform plan`). Boiling it all down, French-Owens explains that Terraform applies diffs to your infrastructure to achieve the desired configuration.

With this foundational information in mind, French-Owens goes on to explain how to provide safety with Terraform; specifically, how do I protect existing infrastructure and prevent Terraform from destroying it? One common approach is to use separate AWS accounts (i.e., one account for dev, one account for staging, one account for production).

Basic state management is important, says French-Owens. One key rule is to use remote state/remote backends. It doesn't matter as much which remote state you use (Terraform Enterprise, S3, Consul, etc.), it's more important that you just actually use remote state. French-Owens quickly runs through some advantages and disadvantages of the various remote state backends (S3, Consul, and Terraform Enterprise).

While remote state is important, there's more to state management according to French-Owens. One approach is breaking down state into "smaller" components, such as using different states for different services. Another approach is to use shared states grouped by team. Mastering the use of read-only remote state is also important. You can use the `terraform_remote_state` option, you can use Terraform's data sources (although at first glance I don't understand how remote state and data sources should both be lumped together---they seem somewhat unrelated), or you can use shared outputs from other Terraform modules.

Summarizing state safety:

* Use separate cloud accounts.
* Use states per environment.
* Consider states per service or per team.
* Use a remote state manager (like Terraform Enterprise or S3).
* Limit your blast radius.
* Use some sort of read-only state.

The second part of safety, according to French-Owens, pertains to Terraform modules. French-Owens believes that establishing standards/guidelines for writing Terraform modules can make Terraform "feel safe". These can include separate definitions of inputs and outputs (using separate files), using sane default for inputs (to make it easier to consume), leveraging simplified templates, and bundling resources (like IAM) along with resource definitions/modules. Along the way, French-Owens shows examples of these standards/guidelines as they are being used at Segment. Using these guidelines allows you to abstract away some of the complexity, which makes it easier---and safer---for developers to consume. Reducing complexity and risk means developers are more likely to use Terraform, according to French-Owens.

French-Owens provided a summary slide of how to provide safety for modules, but I wasn't able to capture the contents of the slide quickly enough before he advanced.

In closing, French-Owens shows how Segment has leveraged the use of Terraform to address operational issues like ensuring that alerts are created for every service (using Datadog) or how to optimize cloud provider costs by tagging resources with cost center/billing information. He finally wraps up by extending a vision of Terraform as the "package manager for the cloud" and what this means in the future.

At this point, French-Owens wraps up the session.
