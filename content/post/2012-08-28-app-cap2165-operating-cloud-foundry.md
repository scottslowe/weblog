---
author: slowe
categories: Liveblog
comments: true
date: 2012-08-28T13:33:16Z
slug: app-cap2165-operating-cloud-foundry
tags:
- Virtualization
- VMware
- VMworld2012
title: 'APP-CAP2165: Operating Cloud Foundry'
url: /2012/08/28/app-cap2165-operating-cloud-foundry/
wordpress_id: 2796
---

This is a session blog for APP-CAP2165, Operating Cloud Foundry. The speakers are Mark Kropf, Brian McClain, and Dave McCrory (formerly of VMware and now with Warner Music Group). I have no previous experience with Cloud Foundry so I'm really looking forward to this session.

The agenda reveals that the majority of the discussion today will focus on BOSH, which is an open source project that was developed largely with Cloud Foundry in mind. The key takeaways are that BOSH is designed to deploy complex systems; BOSH is itself a complex system (also a distributed system); BOSH has powerful capabilities to deploy, manage, and scale complex systems.

First, what is Cloud Foundry? Cloud Foundry is an open source Platform as a Service (PaaS) solution that presents runtimes, frameworks, and services to applications for easier deployment and scaling of applications. Cloud Foundry has the ability to dynamically instantiate services that applications require (McCrory uses the example of a MySQL database as a service that could be instantiated automatically). Runtimes can be considered programming languages, like a specific JRE (Java Runtime Environment). Frameworks can be considered libraries. Services would be things like MySQL, RabbitMQ, or other associated functionality.

What is BOSH? BOSH is a tool chain for deploying, managing, and scaling large, complex systems (and Cloud Foundry is an example of a complex system). What does that mean? BOSH helps deal with the dependencies among various components that need to be deployed and/or booted up, and that things work as expected. The primary way of interacting with BOSH is the BOSH CLI. In addition to Cloud Foundry, other complex systems that can be deployed by BOSH include Gerrit/Jenkins as well as BOSH itself. (That will make your head hurt.)

When using BOSH to deploy BOSH, we start with the BOSH deployer. The BOSH Deployer is a Ruby GEM. The BOSH deployer deploys Micro-BOSH in a single VM (think of Cloud Foundry micro-cloud). The micro-BOSH then offers you the option to deploy the "full-sized" BOSH instance as a distributed cluster.

What is in a BOSH release? Five things: source, packages, jobs, stemcell, and manifest. In the source is found things like the Cloud Controller, Redis, pre-compiled binaries like MySQL). Packages include items like scripts to compile/extract each package, bundler commands, compiled versions of nginx, Ruby, Redis, etc. Jobs include monit scripts for starting/stopping/restarting services, the list of required packages and dependencies, and various configuration files. Example onfiguration files include YAML and ERB teplates. The stemcell is a base VM image to build the various Cloud Foundry nodes in a consistent fashion. It is possible to build your own stemcell. The manifest is a YAML file tha describes all the deployment-specific parameters. This includes stuff like VM size, network settings, and job configuration.

Generally, the first four portions of a BOSH release are provided for you; it's up to you to provide the manifest.

BOSH currently supports vSphere, OpenStack, AWS, and something else I wasn't able to catch in time. Some commands to know:

* `bosh create release` - bundles all the pieces

* `bosh upload release` - uploads the BOSH release

* `point deployment <manifest.yml>` - select the YAML manifest

They provided a fourth command but I wasn't able to capture that command.

As an example of using BOSH, the team shows a demonstration of deploying a complex instance of WordPress with multiple web front-ends and a back-end database. The demo video leverages vSphere, but through the CPI (Cloud Provider Interface) this could be a different platform if desired. While WordPress isn't necessarily the best example, it is one that the attendees can understand and relate to.

The next example is Cloud Foundry itself. According to the presenter, the biggest challenge was deploying additional DEAs. Adding extra DEAs involved changing the manifest, deploying the BOSH release, and spinning up new VMs to accommodate the changes in the manifest.

At this point, the presenters opened up for questions.
