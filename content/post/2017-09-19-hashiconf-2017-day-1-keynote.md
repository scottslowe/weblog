---
author: slowe
categories: Liveblog
comments: true
date: 2017-09-19T09:00:00Z
tags:
- HashiConf2017
- Vagrant
- Terraform
- Consul
title: HashiConf 2017 Day 1 Keynote
url: /2017/09/19/hashiconf-2017-day-1-keynote/
---

This is a liveblog from the day 1 keynote (general session) at HashiConf 2017 in Austin, TX. I'm attending HashiConf this year as an "ordinary attendee" (not working or speaking), and so I'm looking forward to being able to actually sit in on sessions for a change.<!--more-->

At 9:43am, the keynote kicks off with someone (I don't know who, he doesn't identify himself) who provides some logistics about the event, the Wi-Fi, asking attendees to tweet, etc. After a couple minutes, he brings out Mitchell Hashimoto, Founder and co-CTO of HashiCorp, onto the stage.

Hashimoto starts out his talk by reviewing a bit of the history and growth of both HashiConf (and, indirectly, HashiCorp). Last year, HashiCorp has grown from about 50 employees to now over 130 employees. HashiCorp has also seen significant community growth, Hashimoto says, and he reviews the growth in in the use of HashiCorp's products (Vagrant, Packer, Terraform, Vault, Consul, and Nomad). Hashimoto also reviews the growth in their commercial products (Consul Enterprise, Vault Enterprise, and Terraform Enterprise). Hashimoto also discusses HashiCorp's commitment to open source software and the desire to properly balance commercial (paid) products versus free (open source) projects.

Hashimoto now transitions his discussion into a look at how HashiCorp (from here on referred to just as HC) products are deeply involved in companies' transition to cloud-based operational models. 

Starting his review of product updates within the four major categories, Hashimoto starts with Terraform (a favorite project of mine). Terraform has seen tremendous growth, both in use and in functionality, and Hashimoto says he believes Terraform is emerging as the "standard" for provisioning infrastructure. This leads to the announcement of the Terraform Registry, a place to find Terraform modules. Terraform Registry will include partner modules (from HashiCorp partners), as well as Verified modules (modules that HashiCorp has reviewed, vetted, and approved). He next shows a recorded demo of how easy it is to publish a Terraform module in the Registry. The Registry is available immediately at [https://registry.terraform.io][link-1].

Hashimoto now brings out Corey Sanders from Microsoft Azure. Sanders talks about the eight different Terraform modules that Microsoft is actively working on/maintaining and that will be available via the Terraform Registry. Sanders also announces that moving forward Terraform will be pre-installed in the Linux containers that power Microsoft's Azure Cloud Shell (a CLI environment for Azure). This is really powerful, in my humble opinion. Finally, Sanders talks about updates to Azure's Terraform provider, such as adding support for Azure EventGrid and Azure Container Instances (ACI).

Hashimoto now returns to the stage to discuss the role of collaboration within the area of deploying infrastructure. Examples of things that HC has done to improve collaboration include remote state backends and state locking. This leads into a discussion of HC's rearchitecture of Terraform Enterprise (TFE). Workspaces---available in both Terraform and TFE---are a key part of collaboration. Hashimoto reviews improvements in the "Plan" and "Apply" stages within TFE, including integrations with GitHub, BitBucket, and GitLab. TFE was also rebuilt on top of an API-based architecture for more flexibility. The new version of TFE is in beta today, and is expected to be available by the end of the year.

Hashimoto now invites Armon Dadgar (HC Founder and co-CTO) to the stage to talk about more product updates. Dadgar starts his presentation with a focus on Vault, a project for solving secrets management. Adoption of Vault has been faster/broader than expected, according to Dadgar, and Dadgar believes that this is due in part to the evolution of security away from "perimeter only" to more "defense in depth". Dadgar says there are three separate axes involved: secrets management, encryption as a service, and privilege access management. Dadgar invites Dan McTeer from Adobe to the stage to talk about how they're using Vault.

McTeer reviews the role of Adobe Digital Marketing, which builds and maintains over 23 different digital marketing tools across a number of different teams. These tools are distributed across multiple geographical regions and multiple public cloud providers, and McTeer reviews some of the scale that he and his team has to manage. In reviewing the goals and requirements for his team, McTeer points out that any tool they select _must_ have a REST API, and how this led his group to Vault Enterprise. McTeer's team maintains multiple Vault clusters in different geographic regions and on different cloud providers, and how this has greatly streamlined some of their security processes (like reducing the time taken to do privileged account password rotation for thousands of hosts down to less than a minute).

Dadgar now returns to the stage, and quickly reviews the Google Auth backend that was released earlier in the year. This leads to a discussion of Kubernetes support within Vault, and how it works with Vault (leverages JWT for integration). This Kubernetes integration will be available in Vault 0.8.3, which should be available today (or later today).

Moving on, Dadgar now transitions to Consul, HC's service discovery tool (among other things). Dadgar reviews the many features that HC is adding to Consul, but also reviews HC's commitment that Consul's strong foundation in academia (with regard to Consul's underlying subsystems and protocols). Within the last year, HC launched HashiCorp Research as a way of "giving back" information and research on protocol, design, algorithms, and such. The result of this work was a paper on "Lifeguard," which is a set of extensions to Consul's protocols to reduce false positives in failure detection and reduce the latency of failure detection.

After reviewing Consul's (long and storied) history, Dadgar announces the availability of Consul 1.0. Consul 1.0 will be available today in beta form.

Next, Dadgar shifts his attention to Nomad, a batch/cluster scheduler. HC has been busy adding lots of features to Nomad (as driven by customer demand), and Dadgar introduces Mohit Arora from Jet.com to the stage to talk about how Jet is using Nomad.

Arora briefly reviews Jet's value proposition, but quickly transitions into a discussion of their use of cluster schedulers and Nomad. According to Arora, they chose Nomad because it was cross-platform, flexible, easy to use, and offered integration with Consul and Vault.

Dadgar returns to the stage following Arora's presentation, and he announces a native user interface (UI) for Nomad. The UI is enabled by a single flag and served out of the same set of processes that Nomad already runs. The Nomad UI is available now.

Following his discussion of the Nomad UI, Dadgar starts to address how Nomad helps address both the concerns of developers as well as the concerns of operators. Asking the question, "How can we do this at scale?" To answer that question, Dadgar announces Nomad's ACL (access control list) system that will help organizations to define roles/permissions in a fine-grained basis. Dadgar mentions that this is related to the ACL systems in Consul and Vault, but subtly different (perhaps due to the different roles of the various tools). The ACL system is available in the Nomad UI. ACLs and the Nomad UI are present in the Nomad 0.7 beta, which is currently available.

Dadgar talks about the "Million Container Challenge", and that leads him into a discussion of Nomad Enterprise (joining Vault Enterprise, Consul Enterprise, and Terraform Enterprise as commercial products). One feature of Nomad Enterprise is native support for namespacing, which helps with the mult-tenancy needed for many larger organizations. In conjunction with namespaces, users can attach quotas to those namespaces to preserve the overall quality of service (QoS) of the entire clsuter. All this is supported without sacrificing Nomad's multi-region support.

Dadgar now transitions back to Hashimoto. Hashimoto reviews the evolution of HC's tools as the industry has evolved (VMs powered by Vagrant moving to cloud infrastructure orchestrated by Terraform moving to containers and microservices executed by cluster schedulers). Hashimoto talks about adding some "safety guards" to automated environments, like forbidding changes outside working hours, or ensuring that all services have associated health checks. Other examples include ensuring proper key size for TLS certificates or making sure AWS instances are tagged with a billing entity.

This leads Hashimoto to announce Sentinel, a tool for defining "policy as code." Sentinel is an enterprise feature that is being integrated into HC's various Enterprise products (so if I'm understanding correctly Sentinel will not work with/be available for the non-Enterprise versions). Hashimoto walks through examples of how Sentinel's "policy as code" functionality addresses each of the examples he outlined earlier (walking through examples in Consul, Nomad, Vault, and Terraform).

So how does Sentinel work? It's built upon the idea of "infrastructure as code," but this time applied to policy (hence "policy as code"). Sentinel leverages an "easy-to-use" policy language to make it easy to write policies. Sentinel is described by Hashimoto as an "embedded framework" that enables active enforcement and doesn't require you to run a separate application/process to verify policy. (It is capable of running in a passive enforcement mode.) Sentinel supports multiple enforcement levels (advisory, soft mandatory, and hard mandatory), and offers components to help test policies to clearly identify failures. These components were explicitly designed to run in continuous integration (CI) environments. Sentinel also supports external data sources via plugins called "imports"; this allows Sentinel to leverage external data sources (such as ServiceNow, for example) when evaluating policy. An SDK (software development kit) is available to help develop Sentinel imports.

Hashimoto reiterates that Sentinel is integrated into the Enterprise products, and will be available in the next version of each Enterprise product. Hashimoto shows some examples of the integration:

* Integration into Terraform Enterprise to prevent `terraform apply` if policy is violated
* Integration into Consul Enterprise to determine which service registration is allowed to happen (based on policy definition)
* With Vault Enterprise, Sentinel policies could control access to requests, tokens, identities, or multi-factor authentication (MFA)
* When used with Nomad Enterprise, Sentinel can ensure that all jobs that are deployed to a cluster are sourced from an internal artifact repository

Hashimoto next reads a quick blurb about how Barclays will be using Sentinel (Barclays was a design customer who helped shape Sentinel).

At this point, Hashimoto brings out Dave McJannet, CEO of HashiCorp, to the stage. McJannet talks about the company's investments in products and engineering, support structures, and greater regional presence in more geographies/areas. McJannet also reviewed the keynote's announcements:

* Terraform Enterprise updates and Terraform Module Registry
* Vault support for Kubernetes
* Nomad Enterprise and the Nomad 0.7 release
* Consul 1.0 announcement
* Sentinel "policy as code" framework

McJannet talks about how the company focuses on workflows, not technologies; automation through codification; and being open and extensible.

McJannet wraps up the keynote by pointing out the breakout sessions, the Hub (sponsor area), and a preview of tomorrow's keynote speakers.


[link-1]: https://registry.terraform.io
