---
author: slowe
categories: Liveblog
comments: true
date: 2017-10-17T10:00:00Z
tags:
- Docker
- DockerCon2017
- Kubernetes
- OSS
title: DockerCon EU 2017 Day 1 Keynote
url: /2017/10/17/dockercon-eu-day-1-keynote/
---

This is a liveblog of the day 1 keynote/general session at DockerCon EU 2017 in Copenhagen, Denmark. Prior to the start of the keynote, attendees are "entertained" by occasional clips of some Monty Python-esque production.<!--more-->

At 9:02, the lights go down and another clip appears, the first of several cliups that depict life "without Docker" and then again "with Docker" (where everything is better, of course). It's humorous and a good introduction to the general session.

Steve Singh, CEO of Docker, now takes the stage to kick off the general session. Singh thanks the attendees for their time, discusses the growth of the Docker community and the Docker ecosystem, welcomes new members of the community (including himself), and positions Docker less as a container company and more as a platform company. (Singh comes to Docker from SAP, following SAP's acquisition of Concur.) Singh pontificates for a few moments about his background, the changes occurring in the industry, and the "center stage front-row" seat that Docker has to witness---and affect/shape---these changes.

Singh pivots after a few minutes to talk about Docker growth in terms of specific metrics (21 million Docker hosts, for example). This allows him to return to the idea of Docker as a platform---a platform upon which you can build the software supply chain that will support the technology-led disruptions that are affecting every industry. This platform needs to have some essential characteristics:

1. It needs to support a variety of software types.
2. It needs to be portable and support a range of infrastructure options.
3. It needs to be secure, as the appliation layer and at the data layer.
4. It must be an open platform that allows you to "plug and play."
5. It must provide orchestration and application lifecycle management.
6. It needs to provide serverless anywhere.

Based on these essential characteristics, Singh believes Docker is the platform companies can use to build the software supply chain of the future, the one that enables the world to work in a new (better) way. How does this happen? Singh believes that focusing on modernizing traditional applications (identified by the acronym MTA) is the means whereby customers can free up funds to invest in innovation and new initiatives. Singh says customers shouldn't have to choose between maintain and innovate, and Docker enables customers to do both.

This leads to a demo of how customers are modernizing traditional applications, but Singh must first offer his sacrifice to the demo gods, this time by adding some Legos to a Lego construction on the stage.

Singh now brings out Kristie Howard and Ben Bonnefoy to do the demo. Howard and Bonnefoy engage in a bit of role-playing to demonstrate something called the Docker Application Converter (DAC), which can use a virtual machine backup to build a Docker container image. In the demo, it's as simple as running `dac discover tar` to containerize an old Java/Spring/Tomcat application. (In reality, of course, it will often be more complex than this.)

After Howard and Bonnefoy wrap up, Singh takes the stage again to bring out Jeff Murr from MetLife (didn't catch his title). Murr takes a few minutes to share how MetLife is using Docker and the internal transformation required in order for MetLife to truly take advantage of new application architectures with Docker and containers. Based on a proof-of-concept, Murr says that MetLife estimates a 66% cost reduction target as the result of modernizing 593 applications (here "modernizing" == "containerizing"). Murr concludes by reviewing the platform/pipeline that MetLife is establishing as part of their efforts to modernize traditional applications.

As Murr leaves the stage, Banjot Chanana (at least, I think it was him) sets up another demo, and Bonnefoy and Howard return for another demo. This demo shows the use of secrets and network encryption in a Docker Compose file, and the use of the Docker EE dashboard to verify the deployment.

Chanana returns to the stage following the demo to review some of the features of Docker EE that were shown in the demo. (I didn't catch all of the details here, sorry; I was engaged in a Twitter conversation with Solomon Hykes regarding some of my tweets during the keynote.) Chanana also reinforces that modernizing (containerizing) traditional applications is only step one in a larger journey. This is a sentiment with which I strongly agree, and one that I think may have been downplayed a bit too much earlier in this portion (and this was the basis for some of my tweets).

Singh now brings out Solomon Hykes, CTO and Founder of Docker, to the stage. Hykes starts by reinforcing the key principles of the Docker platform: independence, openness, and simplicity. Hykes talks for a bit about simplicity, then switches his focus to talk about the "internals" of Docker. At the bottom of the Docker stack, we have the container runtime (runC and containerD). On top of the container runtime is orchestration (in the case of Docker, this is Swarm). On top of the orchestration you'll find developer tools (like Docker Community Edition, also known as Docker CE). Finally, at the top, you have management services, which for Docker take the form of Docker Enterprise Edition (or Docker EE).

In talking about the process Docker uses to improve its platform, Hykes talks about the feedback from customers regarding orchestration. How does Docker (the company) handle integration versus choice? This leads Hykes to the announcement that the next version of Docker will natively support not only Swarm but also Kubernetes, and that native support will be tightly integrated with the Docker stack (i.e., Kubernetes and Swarm will be equally supported and integrated). Hykes reinforces that this is _not_ a fork of Kubernetes, but native upstream Kubernetes in the Docker stack.

To talk a bit more about this, Hykes brings out Brendan Burns, one of the co-founders of the Kubernetes project (and now with Microsoft). Burns reinforces the "co-evolution" path that Docker and Kubernetes have been following, but fairly quickly transitions into talking about Microsoft Azure and Microsoft's partnership with Docker.

Hykes returns to the stage to talk in more concrete terms about the integration of Kubernetes into the Docker stack. What does this look like, exactly? Docker will provide a way to test locally with Kubernetes, and will allow developers to continue to use all existing Docker tools instead of having to learn new Kubernetes tools. Developers are, of course, welcome to use Kubernetes tools.

To demonstrate how this works, Hykes brings out David Gageot to show the first demo of Kubernetes integrated into Docker. Gageot is using Docker for Mac, and he show how using standard tools like `docker stack` to deploy a sample application on Kubernetes. Gageot also shows how Kubernetes-standard tools like `kubectl` are also being extended to show "Docker" concepts like stacks.

Hykes returns to the stage to talk about how Kubernetes is integrated with Docker EE for production. The next version of Docker EE will contain a full, upstream-compatible version of Kubernetes that sits right next to the Swarm support that Docker EE already possesses.

Hykes brings out Daniel Hiltgen an Alex Mavrogiannis to demonstrate the next version of Docker EE and its Kubernetes integration. Hiltgen and Mavrogiannis walk through the Kubernetes integration in Docker EE, and show how easy it is to switch between using standard Docker tools with Kubernetes or using standard Kubernetes commands to deploy against Kubernetes as part of Docker EE.

Hykes now returns to the stage, and reinforces that Docker (the company) has put a clear emphasis on making the integration seamless for users. Hykes reinforces that this isn't a wrapper, it's not a fork---and this took a great deal of effort and coordination with the Kubernetes community. Hykes says that this integration was made possible by the transition earlier this year (announced in Austin) to the Moby Project, which is the upstream open source project that then becomes the Docker CE and Docker EE products. Hykes shares show the various components of Moby (and ultimately Docker) have been integrating and collaborating with Kubernetes for almost a year.

At this point, Hykes introduces Tim Hockin from Google and brings him out to the stage. Hockin welcomes the Docker community to the Kubernetes community, but reiterates that this is more like a "family reunion" than anything else. Hockin reinforces that the Kubernetes integration shown by Docker this morning is "pure Kubernetes"; this isn't a fork or a wrapper, as Hykes has mentioned several times already. Hockin talks for a bit about containerD and the Kubernetes Container Runtime Interface (CRI), and announces that the "cri-containerd" project has now reached alpha status (beta status expected later in the year or early next year). Hockin also points out that the Open Container Initiative (OCI) has switched to a more open governance model, which he feels is also very important. Hockin also makes brief reference to the Container Storage Interface (CSI), but doesn't go into any details.

Hykes returns to the stage, takes a quick selfie with Hockin and Burns, and expresses his thanks and appreciation to all those who worked hard to make the Docker-Kubernetes integration possible.

At this point, Hykes wraps up the general session.
