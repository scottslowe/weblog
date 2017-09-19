---
author: slowe
categories: Liveblog
comments: true
date: 2017-09-19T12:00:00Z
tags:
- Packer
- Terraform
- HashiConf2017
title: 'Liveblog: Journey to the Cloud with Packer and Terraform'
url: /2017/09/19/journey-to-cloud-packer-terraform/
---

This is a liveblog of the HashiConf 2017 breakout session titled "Journey to the Cloud with Packer and Terraform," presented by Nadeem Ahmad, a senior software developer at Box.<!--more-->

Ahmad starts with a quick review of Box, but (thankfully) transitions quickly to his particular team at Box (the Productivity Engineering team). His team's customers are the software developers at Box, and it's his team's job to help make them more productive and efficient. One of the tools that Ahmad's team built was a tool called Cluster Runner, which is intended to streamline running unit and integration tests on the code the developers were writing.

This brings Ahmad to the crux of this presentation, which is telling the story of how Box went from a bare-metal environment to a cloud-based architecture. The purpose of this migration was to address some of the limitations of their bare-metal environment (inelastic, divergent host configurations over time, etc.). Box leveraged Platform9 to build an OpenStack-based private cloud, with the intent of switching to AWS, GCP, or Azure in the future as private cloud resources aged out.

Ahmad next goes into why Box selected the process they did; they wanted to move away from configuration synchronization (making changes to a server over time, either manually or in some automated fashion) to immutable servers (a change to the configuration triggers a new image build and new server deployment). A blend of these two approaches ("phoenix servers") means you still run configuration management tools against your servers, but deployments of fresh images occur much more frequently.

With this in mind, Ahmad moves into a description of Box's specific project to go down this path. Their requirements for this new system needed to meet the following requirements:

* Easy-to-use
* Self-service
* Fully automated
* With built-in validation

To address "Phase 1" (building and verifying images), Box selected the HashiCorp tool Packer. Ahmad pointed out that Packer's multi-cloud support was very important for his team. Packer also supports multiple provisioners; for Box, this meant Packer supported their existing Puppet manifests, which allowed them to leverage their investment and expertise in Puppet.

However, Ahmad and his team felt that Packer's JSON-based configuration was a bit too complex. This address this complexity, Box built a tool on top of Packer. An additional reason for building this tool was to add an "out-of-band" verification process, to verify that the final image meets the specifications provided. This tool---called `image-pipeline`---takes a simple YAML file that, in turn, incorporates various Packer JSON instructions in an easy-to-assemble way. Image Pipeline is executed via Jenkins jobs.

Now that "Phase 1" (building and verifying images) is complete, Ahmad moves into disussing "Phase 2" (deploying instances). This is where Terraform comes into play. Terraform's ability to launch instances allows Box to quickly and easily take the images built by Image Pipeline (their tool on top of Packer) and deploy them onto OpenStack (their private cloud) or a public cloud.

As with Packer, Ahmad and their team felt that the Terraform configurations were a bit too complex. So, they wrote a tool (the Terraform Configuration Generator). This tool takes simple YAML files and then spits out HCL-formatted Terraform configurations to reflect the instructions found in the source YAML files.

Ahmad reminds session attendees that it's important to iterate continuously over the processes found in building, verifying, and deploying workloads. Box built yet another tool, called Project Recycle, that automates the process of re-deploying fresh infrastructure (using the tools and procedures described earlier).

As Ahmad starts to wrap up, he shares some lessons learned:

* Interact with infrastructure via API.
* Make it easy to build and verify images.
* Automate on top of simple, composable tools (like Packer and Terraform).

Where is Ahmad's team headed next? There's closely evaluating auto-scaling technologies (to scale down at night and scale back up during the day). Ahmad's team is also evaluating the use of Docker containers to help with their CI pipeline needs (his team is focused quite heavily on optimizing CI infrastructure).

At this point, Ahmad wraps up and opens up for questions from the audience.
