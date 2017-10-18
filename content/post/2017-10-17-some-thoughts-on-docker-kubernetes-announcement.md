---
author: slowe
categories: Musing
comments: true
date: 2017-10-17T20:30:00Z
tags:
- Docker
- DockerCon2017
- Kubernetes
title: Some Thoughts on the Docker-Kubernetes Announcement
url: /2017/10/17/some-thoughts-on-docker-kubernetes-announcement/
---

Today at DockerCon EU, Docker announced that the next version of Docker (and its upstream open source project, the Moby Project) will feature integration with Kubernetes (see [my liveblog of the day 1 general session][xref-1]). Customers will be able to choose whether they leverage Swarm or Kubernetes for container orchestration. In this post, I'll share a few thoughts on this move by Docker.<!--more-->

First off, you may find it useful to review some details of the announcement [via Docker's blog post][link-5].

Done reviewing the announcement? Here are some thoughts; some of them are mine, some of them are from others around the Internet.

* It probably goes without saying that this announcement was largely anticipated (see [this TechCrunch article][link-6], for example). So while the details of how Docker would go about adding Kubernetes support was not clear, many people expected some form of announcement around Kubernetes at the conference. I'm not sure that folks expected this level of integration, or that the integration would take this particular shape/form.
* In looking back on the announcement and the demos from today's general session and in thinking about the forces that drove Docker to provide Kubernetes integration, it occurs to me that this almost _necessitated_ the direction/manner of integration. Think about it: had Docker chosen to "separate" Docker and Kubernetes under different software stacks, they would have perpetuated the (perceived) battle between Docker and Kubernetes. However, by positioning Docker "above" Kubernetes, Docker instead emphasizes that the true value of Docker isn't the runtime (`runC` or `containerD`) and it isn't the orchestration (Swarm and Kubernetes). It is, instead, the developer-friendly workflow, ease of use, and management functionality. It also reinforces Docker as a platform, a message that was definitely hammered home in the general session.
* Integrating Kubernetes is, in my opinion, just a continuation of the effort that originated in the spin-out of `runC` to the OCI (and later the donation of `containerD` to the CNCF)---all these steps are necessary to allow Docker to divorce itself from the perception that "Docker == containers". (VMware faces a similar challenge, in that folks think "VMware == VMs/hypervisor".)
* Naturally, there are questions now regarding what will happen to Swarm. The general consensus (see [this post][link-1] by Nigel Poulton and [this post][link-2] by Laura Frank) is that Swarm---as an orchestration mechanism---isn't going anywhere anytime soon, and I'd agree with this assessment. At the same time, given the industry momentum around Kubernetes---Rancher's [rebuild on top of Kubernetes][link-3] is one example, [VMware's joint announcement][link-4] (with Google and Pivotal) of Pivotal Container Service is another---it's also fairly apparent that leveraging Kubernetes instead of Swarm is probably a better long-term choice. (It would be interesting to think about how Swarm/SwarmKit might be used to enhance Kubernetes.)

As others have said, this is definitely an interesting time in which to work in the technology field. Look for more liveblogging from DockerCon EU 2017 tomorrow, where I'll be covering the general session in the morning and as many breakout sessions as I can cram into my schedule.



[link-1]: http://blog.nigelpoulton.com/kubernetes-vs-swarm-and-the-winner-is/
[link-2]: https://blog.codeship.com/docker-and-kubernetes/
[link-3]: http://rancher.com/rancher2-0/
[link-4]: https://www.vmware.com/radius/pivotal-container-services/
[link-5]: https://blog.docker.com/2017/10/kubernetes-docker-platform-and-moby-project/
[link-6]: https://techcrunch.com/2017/10/17/docker-gives-into-invevitable-and-offers-native-kubernetes-support/
[xref-1]: {{< relref "2017-10-17-dockercon-eu-day-1-keynote.md" >}}
