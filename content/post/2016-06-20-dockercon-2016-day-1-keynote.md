---
author: slowe
categories: Liveblog
comments: true
date: 2016-06-20T00:00:00Z
tags:
- Docker
- OSS
- Linux
- DockerCon2016
title: DockerCon 2016 Day 1 Keynote
url: /2016/06/20/dockercon-2016-day-1-keynote/
---

This is a liveblog for the day 1 keynote of DockerCon 2016, taking place over the next couple of days in Seattle, WA. Before the keynote starts in earnest, Gordon the Turtle entertains attendees with some "special" Docker containers that affect the display on the main stage: showing butterflies, playing sounds, launching a Docker-customized version of Pac-Man, or initiating a full-out battle of laser-shooting kittens.

The keynote starts with Ben Golub taking the stage to kick things off. Golub begins his portion with a quick "look back" at milestones from previous Docker events and the history of Docker (the open source project). Golub calls out a few particular sessions---protein folding, data analysis in sports, and extending a video game---and then unveils that these sessions are being presented by kids under the age of 13.

This leads Golub into a review of the efforts of Docker (the company) to democratize containers:

* Increasing usability
* Enhancing portability
* Extending community

Golub gives a "shout out" to the technologies underpinning modern Linux containers (namespaces, cgroups, etc., and their predecessors) and calls out the 2,900+ contributors to the open source Docker project. He then spends the next several minutes talking about various metrics---pull requests, containers pulled, Docker-ized apps, extent of usage among enterprises, etc.---all focused on emphasizing the growth of Docker (the company) as well as Docker (the open source project).

After a brief side venture into sacrifices for the demo gods, Golub introduces Solomon Hykes onto the stage. Hykes re-iterates that the goal of Docker (the company) is to create tools of mass innovation. These tools enable more powerful things. Creating these tools, according to Hykes, has been deliberately done incrementally, in the open.

Hykes next starts focusing on some specific areas that the community has guided and directed Docker (the project) and Docker (the company) to make improvements. The first area Hykes discusses is developer experience, where the main goal was to "remove friction". How to remove friction? First, tools need to get out of the way. Second, tools need to adapt to the user (not make the user adapt to the tools). Third, tools need to make powerful tasks simple. This last point leads Hykes to discuss Docker for Mac and Docker for Windows, and that in turn leads Hykes to introduce Anand Prasad to the stage for the first demo of the session.

Prasad conducts a nice demo showing off the integration between OS X and Docker for Mac as he troubleshoots and debugs a multi-container application.

Following Prasad's demo, Hykes brings Matt Aimonetti, CTO and co-founder of Splice, to the stage to talk about what Splice is and what they're doing with Docker.

After Aimonetti finishes, Hykes takes the stage again to review the progress made on Docker for Mac/Windows, and to open the beta program up to any and all participants. (The presentation points to [beta.docker.com](https://beta.docker.com/), but that site appears to be somewhat specific to a beta announced later in the keynote.)

Wrapping up the discussion on developer experience, Hykes now shifts the presentation to focus on orchestration. According to Hykes, it's now less about the individual container and more about all the various tools that work with containers. However, it's important to make all these tools available to the non-experts. Hykes likens the effort to make orchestration available to non-experts to the effort to make containers available to non-experts. This leads Hykes to the Docker 1.12 release candidate announcement, which will incorporate orchestration directly into the Docker Engine itself.

Orchestration in Docker, according to Hykes, is made up of four major features:

1. Swarm mode: Self-healing, self-organizing clusters of Docker Engine instances that does not require an external data store and is completely infrastructure-agnostic with no single points of failure
2. Crytographic node identity: Every node is uniquely identified with its own cryptographic key and the whole cluster supports end-to-end TLS
3. Docker Service API: Enables a declarative way to tell the cluster what to support and make available, including rolling updates, application-specified health checks, and rescheduling on node failure
4. Built-in routing mesh: Swarm-wide overlay networking offering container-native load balancing (using IPVS), DNS-based service discovery with no separate cluster to setup or manage and interoperability with existing load balancers

This leads into a live demo of the new orchestration features in Docker 1.12; the demo is led by Mike Goelzer and Andrea Luzzardi. The demo shows off the live routing mesh, built-in load balancing, actual state reconciliation (using declarative models and having the cluster reconcile the actual state and the desired state), and rolling updates.

Hykes takes the stage once again as Goelzer and Luzzardi wrap up their demo of the new Docker orchestration functionality. Hykes points out that the official stable release of Docker 1.12 is expected in a few weeks, but that the new functionality is available now in the release candidate or in Docker for Mac/Windows.

To talk about how they used the new functionality in Docker 1.12, Hykes brings up two representatives (didn't catch their names) from Zenly, a company that makes a location-sharing app and service. Their application recently soared in popularity, and the increased volume of events (upwards of 500 million events being generated) was causing "challenges" in their analytics platform. By "challenges," they mean that the platform was crashing. Zenly felt that increasing their footprint in the public cloud would be too expensive, so they moved the analytics platform to bare metal and Docker 1.12, and this helped solve their scaling issues.

Hykes takes the stage after Zenly to move to the third (and final) topic: the operations (ops) experience. Saying they took lessons from the beta experience from Docker for Mac/Windows, Hykes announces beta versions of Docker for AWS and Docker for Azure. These are tools aimed at operations teams that are integrated with the cloud platforms to provide the same low-friction experience that developers enjoy.

Before starting the last demo, Hykes takes a quick sideline to introduce a new (experimental) format for bundling together multi-container applications, called the Distributed Application Bundle (DAB). DAB support is present in Docker Compose, and is supported by Docker for AWS and Docker for Azure. (I would assume it is also supported in the latest betas of Docker for Mac/Windows.)

To show off the operations experience, Hykes brings out Madhu Venugopal and Anand Prasad. Prasad is reprising his role as a new developer, and he needs to deploy his application to production. Venugopal is an operations guy, and he shows how Docker for AWS---which is a customized Amazon Machine Image (AMI) and a customized CloudFormation template along with some apparent integrations into Amazon ELB and security groups---makes it easy for operations teams to deploy large-scale Docker clusters, and how to consume the new experimental DAB format. The interplay between Venugopal and Prasad is pretty entertaining, but though the demo is touted as a live demo a few typos here and there make it clear that it's not completely live. (Semi-live?) Still, it's an entertaining and informative demo.

Hykes takes the stage once more after Venugopal and Prasad wrap up, reiterating the key points from the presentation. He once again invites attendees to take advantage of the new beta programs, and then wraps up the general session for day 1.

**UPDATE:** I spoke briefly with Madhu, who ran the Docker for AWS demo, regarding my concerns about the demo being "semi-live". Madhu did indicate that the USB copy wasn't live (that part was taking too long), but that the rest of the demo was, indeed, live. Some of the other typos I thought I'd spotted were around exposing ports and ELB integration, but---as I conceded to Madhu---I might have been mistaken. Regardless, it was a good demo, and one that I recommend watching if you weren't there in person to see it.
