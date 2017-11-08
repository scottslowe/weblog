---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-07T23:30:00Z
tags:
- OpenStack
- Docker
- CLI
title: Deep Dive into Container Images in Kolla
url: /2017/11/07/deep-dive-into-container-images-in-kolla/
---

This is a liveblog of my last session at the Sydney OpenStack Summit. The session title is "OpenStack images that fit your imagination: deep dive into container images in Kolla." The presenters are Vikram Hosakote and Rich Wellum, from Cisco and Lenovo, respectively.<!--more-->

Hosakote starts with an overview of Kolla. Kolla is a project to deploy OpenStack services into Docker containers. There are two ways to use Kolla: using Ansible (referred to as Kolla-Ansible) or using Kubernetes (referred to as Kolla-Kubernetes). Hosakote mentions that Kolla-Kubernetes also uses Helm and Helm charts; this makes me question the relationship between Kolla-Kubernetes and OpenStack-Helm.

Why Kolla? Some of the benefits of Kolla, as outlined by Hosakote, include:

* Fast(er) deployment (Hosakote has deployed in as few as 9 minutes)
* Easy maintenance, reconfiguration, patching, and upgrades
* Containerized services are found in container registry
* One tool to do multiple things

Hosakote briefly mentions his preference for Kolla over other tools, including Juju, DevStack, PackStack, Fuel, OpenStack-Ansible, TripleO, OpenStack-Puppet, and OpenStack-Chef.

Other benefits of using containers for OpenStack:

* Reproduce golden state easily
* No more "Works in DevStack" but doesn't work in production
* Production-ready images (this seems specific to Kolla, not just containers for OpenStack control plane)
* Easy to override the configuration files
* Dev mode (enables faster dev/test cycles)
* Portable, tested, replicated images in well-known registries (again, this is specific to Kolla, not just using containers for the OpenStack control plane)

Next, Hosakote goes over the Kolla architecture. The "deploy node" runs Ansible, Docker, the Docker Python libraries, and Jinja. All the `Dockerfiles`, Ansible playbooks, Jinja templates, Helm charts, etc., are pulled from GitHub. By default, Kolla uses the Docker Hub for container images, but it can also use a custom registry. The deploy node deploys Docker containers with OpenStack services onto target nodes.

When Kubernetes is in play (via Kolla-Kubernetes), the architecture is similar but with the addition of Kubernetes-specific components (such as the `kubelet` on worker nodes).

Hosakote takes care to point out that Kolla can be used for a variety of "advanced" configurations, like:

* Jumbo frames
* SR-IOV (PCI passthrough)
* Ceph storage
* EFK (ElasticSearch, Fluentd, Kibana)
* Prometheus (cloud-native monitoring and alerting)
* Numerous other customizations and configuration overrides

Next, Hosakote briefly reviews how to use Kolla (Kolla-Ansible, specifically). He went through the content so quickly that I wasn't able to catch all the details.

Now, Wellum takes over the presentation to discuss Kolla-Kubernetes. Wellum mentions a few resources for using Kolla-Kubernetes, but immediately transitions into a discussion of why you may want to build your own custom Kolla images (as opposed to using the "official" Kolla images available on Docker Hub). Some of these reasons include:

* You run a "custom" or "proprietary" version of OpenStack
* You are an OpenStack contributor and would like to test in production-quality environments
* Your company develops drivers unique to hardware (this allows you to easily stand up OpenStack so you can spend more of your time focusing on hardware support/integration)

This leads Wellum into an example of how you might use Kolla to build custom images for a couple of OpenStack services, but use "vanilla" upstream code for the rest of the OpenStack services. Wellum again mentions the use of Helm, but does not provide any information on interaction between Kolla-Kubernetes and OpenStack-Helm.

Next, Wellum shows a recorded demo that shows everything he just discussed about the example of building custom images for a couple of OpenStack services. The presenters run into some permissions issues with Google Docs, but after a few minutes manage to finally resolve those so they can show the demo. The demo, unfortunately, is limited in size and has poor color contrast that makes it difficult to see from the back of the room.

After watching the demo for a while, I wrapped up the liveblog as the session was running over its time limit.
