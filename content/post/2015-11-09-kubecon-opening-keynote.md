---
author: slowe
categories: Liveblog
comments: true
date: 2015-11-09T00:00:00Z
tags:
- Kubernetes
- OSS
title: 'Kubecon Liveblog: Opening Keynote'
url: /2015/11/09/kubecon-opening-keynote/
---

This is a liveblog of the opening keynote at the inaugural Kubernetes conference, Kubecon, taking place this week at the Palace Hotel in San Francisco. Brendan Burns, Senior Staff Software Engineer at Google, is delivering the opening keynote. Burns is a co-founder of the Kubernetes project.

Burns starts out with a quick review of a bit of Kubernetes history, and reviews the broad diversity of submitters that are participating in the development of Kubernetes. He doesn't spend much time there, though, and quickly transitions into a "where are we going?" discussion.

He says that Kubernetes wasn't really about containers, or scheduling; it was really about making reliable, scalable, agile distributed systems a CS101 exercise. Kubernetes is really about making it easier to build distributed systems, to scale distributed systems, to update distributed systems, and to make distributed systems more reliable. Burns demonstrates how Kubernetes makes this easier by showing a recorded demo of scaling Nginx web servers up to handle 1 million requests per second, and then updating the Nginx application while still under load.

After the demo completes, Burns takes a few minutes to break down the architecture behind the demonstration. "Loadbots," managed by a Kubernetes replication controller, generated the load against an Nginx service, which in turn is backed by a number of Nginx instances. The demo also included a data aggregation layer, also managed by Kubernetes, to gather information; finally, a service existed for the UI to present the information shown in the demo. The entire demo setup was run in a single Kubernetes 1.0 cluster.

This leads Burns into a discussion of new features found in Kubernetes 1.1:

* HTTP load balancing
* Autoscaling
* Batch jobs
* Resource overcommit
* IPTables-based `kubeproxy`
* New `kubectl` tools

The new Kubernetes release will be rolled out to Google Container Engine for new clusters this week, and rolling out to existing clusters over the next few weeks.

One of the new features in the 1.1 release is HTTP load balancing, which allows you to create an "ingress point" and then map paths (like `http://api.company.com/foo` or `http://api.company.com/bar`) that point to different services (instead of requiring different services to use different IP addresses). This new functionality is lumped under the name "Ingress API." Ingress is implemented via an ingress object (exists in the API) and an ingress controller; there is a controller for GCE and one in development for HAProxy (Burns expects the community will build more controllers for more load balancing solutions).

The next new feature that Burns discusses is the horizontal pod autoscaling, which allows pods to scale automatically based on resource utilization triggers (and this is handled by the replication controller). Why the use of the name "horizontal autoscaling"? This is because it works by adding (or removing) pods on a scale-out basis. Vertical autoscaling is also something being explored, although it doesn't exist yet (vertical autoscaling would involve adding resources to individual pods).

Next up in Burns' discussion is the batch jobs API, which allows Kubernetes to better handle batch jobs (Kubernetes was much better at running long-lived workloads). The new jobs API allows you specify how many completions of a job (this is how many times this job will be run in parallel) and parallelism (how many jobs simultaneously, up to the completions). In the future, Burns anticipates looking into even more features being made available by the batch jobs API.

In the realm of improvements to Kubernetes, Burns discusses resource overcommit support. In this release, it's for memory overcommit only, with multiple resource classes (Guaranteed, Burstable, and Best Effort). The second major improvement is the replacement of the userspace `kubeproxy` implementation with an IPTables-based approach. This improves performance (in particular latency is significantly improved), and allows the source IP address to be preserved. Finally, there are some improvements to `kubectl`, including the ability to use `kubectl run -i --tty shell --image=busybox -- sh` to get a shell running inside your cluster. (Burns provided a number of other examples, but I wasn't able to capture all of them.)

There are also improvements to rolling updates, such as pausing the rolling update on a terminal failure (and allowing the user to rollback or proceed), as well as making sure that an upgrade is successful before proceeding to update other containers in the cluster.

In the final section of Burns' keynote, he goes back to the question, "Why are we all here?" It's not to run nodes, or containers, but to build applications to support our users and our companies. Burns asks the question, "What if building distributed systems was viewed as coding applications?" In particular, are there ways that Kubernetes could help developers build distributed systems by abstracting away some of the components that are needed.

Burns wraps up by "looking ahead" to Kubernetes 1.2:

* Prebuilt applications (Google Deployment Manager and Deis Helm)
* Cross-cluster management (aka "Ubernetes") to tackle things like cross-cluster service import/export
* Simplified configuration, including configuration generators

At this point, Burns wraps up his presentation.
