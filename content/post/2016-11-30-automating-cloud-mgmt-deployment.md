---
author: slowe
categories: Liveblog
comments: true
date: 2016-11-30T14:30:00Z
tags:
- AWS
- reInvent2016
title: 'Liveblog: Automating Cloud Mgmt and Deployment'
url: /2016/11/30/automating-cloud-mgmt-deployment/
---

This is a liveblog of the AWS re:Invent session titled "Automating Cloud Management and Deployment for a Diverse Enterprise Application Portfolio" (DEV319). The speakers for the session are David Lowry and Amul Merchant, both from Infor Global.

Merchant kicks the session off with a brief overview of Infor and its cloud strategy. Infor's CEO, Charles Phillips, was quoted from AWS re:Invent 2014 as having said, "Friends don't let friends build data centers." Merchant spends a pretty fair amount of time (too much time, in my opinion) explaining Infor and Infor's offerings, with only minimal references to how this affects or is affected by the core topic of the presentation. He makes numerous references to "the DevOps toolset" that Infor uses, but does not provide any details or information on said toolset. Instead, the information shared is far too basic for a 300-level session.

After 15 minutes, Lowry takes the stage to talk about the DevOps toolset. The key principles Infor used in building/selecting tools were:

* Automate end-to-end
* Use AWS services wherever possible (this ties the tools closely to AWS)
* Minimally Viable Product (MVP) first, then expand

Some of the tools Infor uses:

* Faro (more information on that in a moment)
* Windsor (lets Infor perform testing against deployments/configurations)
* Hanover (handles powering down idle resources when they are not in use)
* AWS Toolbox (self-service group administration)

Lowry now turns to focus on Faro. Faro is a deployment framework borrowing from a few different paradigms (one being Linux package managers [dependency management], one being CloudFormation [inputs/outputs], and the last one being Docker [application packaging]). Faro is both a CLI as well as a web service, depending on where one interacts with Faro during the lifecycle of an application.

Why did Infor build Faro? Why not OpsWorks, or CodeDeploy, or ECS, or some other AWS service? Lowry says the reason is that Infor's CloudSuites offerings demands that their deployment framework support all of these types of mechanisms. Some applications may support containers; some applications may not. In a typical Infor deployment, one might find all of these things:

* One or more CloudFormation templates
* Various scripting languages (like PowerShell)
* Various configuration management tools (like SaltStack)
* One or more Lambda functions
* Application binaries

Faro uses parameters to pass in information like security group IDs, VPC ID, subnet IDs, availability zones, AMI IDs, Active Directory (AD) settings, and more. This prevents Infor admins from having to manually gather all this information before running a deployment.

Faro uses a construct called a "deck" to describe the components that make up a deployment. A plain text file (appears it's in YAML), called the "deck spec," defines what a deck looks like. Decks can include other decks. One a deck has been built, it's published to a web URL (this marks the transition from Faro as CLI to Faro as web service). Faro uses a Git-style hashing mechanism to ensure that only unique (non-duplicate) content is stored in Faro. The deployment side of Faro leverages API Gateway, S3, Lambda, DynamoDB, KMS, EC2, and CloudFormation. Faro supports the use of "secure parameters," which are encrypted by KMS.

Future plans for Faro include:

* Updating legacy automation (increase the migration from SaltStack to Faro)
* Eliminating manual steps (Windsor can help catch manual steps)
* Entire environment deployments (help deploy into entirely new regions)
* Reduce deployment timeframes

Merchant now takes the stage again to wrap up the session.
