---
author: slowe
categories: Liveblog
comments: true
date: 2017-05-09T00:00:00Z
tags:
- OpenStack
- Kubernetes
- Ansible
- Networking
- Storage
title: 'Liveblog: Deploying Containerized OpenStack: Challenges & Tools Comparison'
url: /2017/05/09/containerized-openstack-challenges-tools-comparison/
---

This is a liveblog for an OpenStack Summit session on containerized OpenStack and a comparison of the tools used for containerized OpenStack. The speaker is Jaivish Kothari, from NEC Technologies. Two other speakers were listed on the title slide, but were apparently unable to make it to the Summit to present.<!--more-->

Kothari provides a brief overview of the session, then jumps into a discussion of deployment tools. As illustrated by one of his slides, there's a huge collection of tools that are used to deploy OpenStack; some are "pure" deployment tools, others are configuration management tools. In this presentation, Kothari says he will focus specifically on OpenStack deployment tools, like Juju (Canonical), Fuel (Mirantis), Crowbar (Dell), and PackStack/TripleO (Red Hat), but I'm not sure how this relates to containerized OpenStack (per the session title).

According to Kothari, some of the challenges in "traditional" (non-containerized) deployment tools are best understood by looking at the challenges in deploying OpenStack:

* Difficulty related to deployment (conflicts due to services configuration, deployment still prone to failures)
* Ongoing lifecycle management of OpenStack components

This whole first section of the presentation was setting up the argument that containerizing your OpenStack control plane will help address these challenges. Kothari briefly mentions multi-version interoperability, but it's not clear whether he's saying OpenStack needs to do more work in this area or if containers solve this problem. He next spends several minutes talking about how containers help address the challenges of deploying OpenStack. Kothari goes through several slides which show the same information in multiple ways; I get the impression he's trying to convince the audience of the value of the solution (containerized control plane), but this is something I think most of the attendees already understand. He then spends some time reviewing the components of containers and container orchestrators; again, I feel like this is material most of the attendees already know (especially given this was billed as an "Intermediate" session).

Kothari finally moves into a discussion of Kubernetes, but quickly then transitions into a table comparing various deployment tools (Stackanetes, Kolla-Kubernetes, OpenStack Helm, Fuel) and categorizes them according to the back-end tool it uses (Kubernetes, Puppet, or Ansible) and listing the source of the container images involved. Most of the tools use Kolla container images, although a few use their own repository of images.

Next, Kothari compares Kolla-Ansible, Kolla-Kubernetes, and OpenStack Helm based on things like support for continuous integration (CI), GUI support, networking support, approximate deployment time (all of them run about 20 minutes), hardware requirements, platform support, support for high availability (HA), stability (has its reached a "stable" point yet), maturity, and contribution activity.

Following the comparison---which was useful---Kothari moves back into discussing how the use of containers will help (something that was discussed extensively earlier in the presentation). He points out that it's a challenge to move from a non-containerized deployment to a containerized deployment, and that the community has some work to do in this area.

At this point, Kothari says he's going to summarizes the information shared, but launches into a discussion of Kuryr and Fuxi, two projects that he hasn't yet discussed. Kuryr is aimed at simplifying container networking, while Fuxi aims to do the same for storage.

Kothari then ends the session.
