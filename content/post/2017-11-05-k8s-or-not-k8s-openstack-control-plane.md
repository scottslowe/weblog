---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-05T23:15:00Z
tags:
- OpenStack
- Kubernetes
title: To K8s or Not to K8s Your OpenStack Control Plane
url: /2017/11/05/k8s-or-not-k8s-openstack-control-plane/
---

This is a liveblog of the Monday afternoon OpenStack Summit session titled "To K8s or Not to K8s Your OpenStack Control Plane". The speaker is Robert Starmer of Kumulus Technologies. This session is listed as a Beginner-level session, so I'm hoping it's not too basic for me (and that readers will still get some value from the liveblog).<!--more-->

Starmer begins with a quick review of his background and expertise, and then proceeds to provide---as a baseline---an overview of containers and Kubernetes for container orchestration. Starmer covers terminology and concepts like Pods, Deployments (and Replica Sets), Services, StatefulSets, and Persistent Volumes. Starmer points out that StatefulSets and Persistent Volumes are particularly applicable to the discussion about using Kubernetes to handle the OpenStack control plane. Following the discussion of Kubernetes components, Starmer points out that the Kubernetes architecture is designed to be resilient, talking about the use of etcd as a distributed state storage system, multiple API servers, separate controller managers, etc.

Next, Starmer spends a few minutes talking about Kubernetes networking and some of the components involved, followed by a high-level discussion around persistent volumes and storage requirements, particularly for StatefulSets.

Having covered Kubernetes, Starmer now starts talking about the requirements that OpenStack has for its control plane, mapping these requirements to the functionality provided by Kubernetes.

At this point, Starmer transitions into talking about whether Kubernetes is appropriate for the OpenStack control plane. If you're building a multi-tenant, multi-site production grade service, Starmer believes Kubernetes adds a great deal of value. For single-tenant production implementations or development environments, Starmer isn't convinced that the functionality of Kubernetes outweighs the additional complexity.

If you do decide to proceed with using Kubernetes for the OpenStack control plane, Starmer reviews a couple of options for doing exactly that. One option is OpenStack-Helm, which leverages Helm (as the name would imply). Another option is Kolla-Kubernetes, which leverages work done by the Kolla project along with some fine-grained Helm charts.

Starmer next reminds attendees that leveraging Kubernetes for the OpenStack control plane means some additional skills are needed: Kubernetes itself, containers (Docker or LXC, typically Docker), Helm, monitoring (often Prometheus), and new languages (like Golang). Ansible is probably something else you'll need, since most of the container generation tools leverage Ansible to build the container images.

Returning to the discussion of whether someone should use Kubernetes for the OpenStack control plane, Starmer summarizes his findings by saying that for organizations that won't have someone owning the Kubernetes side of the house, they should probably just stick with containerized OpenStack control plane components. However, if the expertise and skills are there to support Kubernetes, then it is a perfectly valid approach.

At this point, Starmer wraps up the session.
