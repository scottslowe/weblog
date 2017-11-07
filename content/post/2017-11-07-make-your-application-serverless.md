---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-07T20:00:00Z
tags:
- OpenStack
- Serverless
- FaaS
title: Make Your Application Serverless
url: /2017/11/07/make-your-application-serverless/
---

This is a liveblog from the last day of the OpenStack Summit in Sydney, Australia. The title of the session is "Make Your Application Serverless," and discusses Qinling, a project for serverless (Functions-as-a-Service, or FaaS) architectures/applications on OpenStack. The presenters for the session are Lingxian Kong and Feilong Wang from Catalyst Cloud.<!--more-->

Kong provides a brief background on himself and his co-presenter (Wang), and explains that Catalyst Cloud is an OpenStack-based public cloud based in New Zealand. Both presenters are active technical contributors to OpenStack projects.

Kong quickly transitions into the core content of the presentation, which focuses on serverless computing and Qinling, a project for implementing serverless architectures on OpenStack. Kong points out that serverless computing doesn't mean there are no servers, only that the servers (typically VMs) are hidden from view. Functions-as-a-Service, or FaaS, is a better term that Kong prefers. He next provides an example of how a FaaS architecture may benefit applications, and contrasts solutions like AutoScaling Groups (or the equivalent in OpenStack) with FaaS.

Some key characteristics of serverless, as summarized by Kong:

* No need to think about servers
* Run your code, not the whole application
* Highly available and horizontally scalable
* Stateless/ephemeral
* Lightweight/single-purpose functions
* Event-driven style
* Pay only for what you use

Some use cases for serverless/FaaS:

* Scheduled (`cron`) jobs
* Microservices-based applications
* Data processing tasks
* IoT
* Mobile backends
* Chatbots
* Side tasks like sending a follow-up email in response to an order, etc.

Some existing serverless/FaaS solutions include AWS Lambda or Azure Functions; in the open source space, there are projects like Apache OpenWhisk, Fission, or Kubeless. Most of these solutions/projects are leveraging container technologies.

This leads Kong to introduce Qinling, which is an OpenStack project intended to provide FaaS. Qinling came out of some of Kong's work in Mistral, and the name has no special meaning other than it is from Kong's home province in China.

Qinling uses Kubernetes on the backend to orchestrate containers that will, in turn, be used to execute functions. Architecturally, Qinling has only two components: the `qinling-api` service and the `qinling-engine` service. Some of the features present in Qinling include:

* RESTful API (API objects include runtimes, functions, executions, and jobs)
* Integrates with other OpenStack services
* Supports code/Docker images/Swift object as a function
* Sync/Async/Periodic execution
* Scale up/scale down
* Supports OpenStack CLI

Qinling can also consume/integrate with Aodh, Zaqar, and Swift (as mentioned above). 

Kong now moves into a pair of demos. The first demo models a common Lambda use case surrounding an action executing after an object is uploaded to S3. In this demo, a Qinling function consumes an alarm from Aodh generated from an object being uploaded to Swift. The demo works, but is a bit slow; Kong explains that this is due to _cold start_ considerations (first-time invocation takes longer than subsequent invocations).

Kong now hands it over to Wang to show the second demo. The second demo shows how to use Zaqar with Qinling. (Zaqar, for those who aren't aware, is multi-tenant "messaging-as-a-service" for OpenStack, similar in nature to Amazon SQS.) This example shows how you might accomplish communications between various functions. Although it takes a few minutes, the demo is successful in sending an SMS to the presenters' phones in response to a service being down in an OpenStack cloud.

Some additional resources on Qinling are shared by the presenters:

* [https://github.com/openstack/qinling](https://github.com/openstack/qinling)
* [http://qinling.readthedocs.io](http://qinling.readthedocs.io)
* IRC channel is #openstack-qinling

At this point, the presenters end the session.
