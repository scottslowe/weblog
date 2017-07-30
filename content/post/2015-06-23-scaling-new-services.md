---
author: slowe
categories: Liveblog
comments: true
date: 2015-06-23T15:25:00Z
tags:
- Docker
- DockerCon2015
- Consul
title: 'Liveblog: Scaling New Services'
url: /2015/06/23/scaling-new-services/
---

This is a liveblog for the DockerCon 2015 session titled "Scaling New Services: From Container Creation to Automated Deployments". This session is being led by the Disney Systems Engineering team and will feature a discussion/demo involving Docker, Mesos, Chef, Consul, and HAProxy.

The session starts with an introduction by Alex Williams, founder of The New Stack, who quickly turns it over to the Disney staff---Brian Scott and Patrick O'Connor. Brian starts with an overview of all the various companies within Disney, and the challenges that breadth creates. He then discusses the role of Disney's Systems Engineering team, and the responsibilities of the team. That includes managing infrastructure, both on-premises as well as cloud-based infrastructure.

So, why Docker? To improve the guest experience, Disney needs to be able to move fast. They want to get away from managing VMs and cattle to managing containers and micro-bots. Brian talks about issues with onboarding developers, battling configuration drifts, and similar challenges. Disney started on their Docker journey 6-10 months ago, and lots of teams are still exploring the use cases for Docker. Some teams are already using it in the CI pipeline, and other teams are evaluating production use cases. CI is a particularly big deal for Disney's Docker deployment currently.

At this point, Patrick takes over to run a live demo. Here are some of the components that are in use in the Disney environment:

* Chef: Chef handles the hosts and laying down the configuration for the Mesos master, the Marathon framework, the ZooKeeper endpoint configurations, etc. Chef also handles the initial setup of Consul and HAProxy.
* Docker: Docker is the packaging solution. Services are packaged using Docker. The Dockerfile for an application resides in the application repo. Application builds include building the Docker image and pushing it to the appropriate registry.
* Mesos: Mesos provides cluster scheduling, placing containers onto hosts. All interaction with Mesos comes through the frameworks (i.e., Marathon, spark, Cassandra, etc.). 
* Marathon: The main framework in use is Marathon, which enables running Docker on Mesos. Marathon deployments (handled via JSON descriptions) will deploy Docker images and configure them appropriately (which image to use, which ports to expose, etc.). Health checks are also included in the Marathon deployment file. Marathon also allows Disney to define the upgrade strategy, which enables them to control how new images are deployed.
* Consul: Disney only uses the distributed key-value store functionality of Consul (not the service discovery aspect). Containers use consul-template to pull information from Consul to customize the configuration at run-time.
* HAProxy: HAProxy handles the routing of requests. Two health checks are involved: one on the HAProxy side, and one on the back-end service side. HAProxy is completely configured via Marathon's service discovery mechanism.
* Jenkins: Jenkins handles the Docker build/push/deploy.

Once he's gone over the components, Patrick moves into the live demo. In the demo, Patrick deploys a container ("Baymax") and shows that it is working. He then triggers a build of a new version. Jenkins builds it, pushes it to the Registry, and deploys an updated version of the container. Marathon updates the containers per the upgrade policy defined in the deployment file. Next, Patrick shows the deployment of a 1,000 very small containers (he calls them microbots) using Marathon and Mesos, and then does a rolling upgrade of those 1,000 containers.

The presenters try to split over to a video, but that doesn't work too well (Internet connection is a bit sketchy here at the conference), so they drop back to the demo. The demo is nearly complete with the rolling upgrade to the 1,000 containers.

At this point, the presenters wrap up the session and open the floor to questions from the audience.