---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-06T22:30:00Z
tags:
- OpenStack
title: How to Deploy 800 Servers in 8 Hours
url: /2017/11/06/how-deploy-800-servers-8-hours/
---

This is a liveblog of the session titled "How to deploy 800 nodes in 8 hours automatically", presented by Tao Chen with T2Cloud (Tencent).<!--more-->

Chen takes a few minutes, as is customary, to provide an overview of his employer, T2cloud, before getting into the core of the session's content. Chen explains that the drive to deploy such a large number of servers was driven in large part by a surge in travel due to the Spring Festival travel rush, an annual event that creates high traffic load for about 40 days.

The "800 servers" count included 3 controller nodes, 117 storage nodes, and 601 compute nodes, along with some additional bare metal nodes supporting Big Data workloads. All these nodes needed to be deployed in 8 hours or less in order to allow enough time for T2cloud's customer, China Railway Corporation, to test and deploy applications to handle the Spring Festival travel rush.

To help with the deployment, T2cloud developed a "DevOps" platform consisting of six subsystems: CMDB, OS installation, OpenStack deployment, task management, automation testing, and health check/monitoring. Chen doesn't go into great deal about any of these subsystems, but the slide he shows does give away some information:

* The OS installation subsystem appeared to leverage IPMI and Cobbler.
* The OpenStack deployment subsystem used Ansible (it's not clear if it was using OpenStack-Ansible or just Ansible with custom playbooks).

Going into a bit more detail, Chen explains that the OS installation process uses PXE plus Kickstart, all glued together using Cobbler. For the 800-node deployment, T2cloud used a total of 3 Cobbler servers, running about 20 servers concurrently in each group. Each server took about 10 minutes to deploy. Chen mentions that they used Cobbler snippet to dynamically locate the target disk when deploying the operating system; this is a feature I hadn't heard of before, and sounds very useful (especially in large-scale environments). Chen also explains how to use Cobbler to revert to the "old style" of interface naming (i.e., `eth0` instead of `enoXXXXXX` or similar).

Coming to the OpenStack deployment portion, Chen discusses the benefits that led T2cloud to choose Ansible for deployment. T2cloud used Ansible both for _ad hoc_ commands as well as with playbooks. Chen reviews some of the key components they needed in order to use playbooks: inventory, roles, modules, group variables, and host variables. (These are all pretty standard Ansible constructs.) Chen does not mention anything about the modules that T2cloud used, other than to say they are idempotent and explain what that means.

Chen does review some performance optimizations for Ansible:

* Disable the `gather_facts` function
* Enable pipelining
* Increase the number of forks running
* Upgrade SSH (as necessary) and use ControlPersist

Next, Chen reviews some issues and their respective solutions discovered during this deployment:

* The Neutron agents were down, even though the services were running; this was later determined to be an insuficient number of file descriptors for the system because RabbitMQ was using too many descriptors
* The Cinder services failed to start, due to an maximum connection limit on the backend MySQL database(s)
* There were a number of other limits or maximum connections that also had to be increased; Chen shows a slide listing quite a few of them

This brings Chen to a discussion of the health and monitoring system, which was used before the deployment, during the deployment, and after the deployment (for different purposes in each phase, of course).

In closing, Chen provides some suggestions/recommendations to attendees:

* Do more tests to find bottlenecks.
* Always do health checks.

At this point, Chen ends the session and opens up for questions from the audience.

Some things I would've liked to hear from Chen:

* How did they get inventory from Cobbler to Ansible? Was it manual, or was there an automated process?
* Which Ansible modules did they use? If they built their own, why?
* What tools were used in the health check/monitoring system?

Without more detailed information, the session was good but not as helpful as it could have been.
