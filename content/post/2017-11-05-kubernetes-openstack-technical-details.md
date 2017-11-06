---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-05T23:45:00Z
tags:
- OpenStack
- Kubernetes
- Cinder
title: "Kubernetes on OpenStack: The Technical Details"
url: /2017/11/05/kubernetes-openstack-technical-details/
---

This is a liveblog of the OpenStack Summit session titled "Kubernetes on OpenStack: The Technical Details". The speaker is Angus Lees from Bitnami. This is listed as an Advanced session, so I'm hoping we'll get into some real depth in the session.<!--more-->

Lees starts out with a quick review of Bitnami, and briefly clarifies that this is _not_ a talk about OpenStack on Kubernetes (i.e., using Kubernetes to host the OpenStack control plane); instead, this is about Kubernetes on OpenStack (OpenStack as IaaS, Kubernetes to do container orchestration on said IaaS).

Lees jumps quickly into the content, providing a "compare-and-contrast" of Kubernetes versus OpenStack. One of the key points is that Kubernetes is more application-focused, whereas OpenStack is more machine-focused. Kubernetes' multi-tenancy story is shaky/immature, and the nature of containers means there is a larger attack surface (VMs provide a smaller attack surface than containers). Lees also points out that Kubernetes is implemented mostly in Golang (versus Python for OpenStack), although I'm not really sure why this matters (unless you are planning to contribute to one of these projects).

Lees next provides an overview of the Kubernetes architecture (Kubernetes master node containing API server talking to controller manager and scheduler; kubelet, cAdvisor, and kube-proxy on the worker nodes; etcd as a distributed key-value store for storing state in the Kubernetes master; pods running on worker nodes and having one or more containers in each pod).

Next, Lees shows another Kubernetes diagram, but this time the diagram illustrates the "connection points" between Kubernetes and the underlying cloud (OpenStack, in this particular case).

Lees spends some time reviewing the basics of Kubernetes networking, reviewing the core constructs leveraged by Kubernetes. In the process of reviewing Kubernetes networking, Lees points out that there are _lots_ of solutions for pod-to-pod (east-west) traffic flows. Traffic flows for internet-to-pod (north-south) are handled a bit differently; Kubernetes assumes each pod has outbound connectivity to the Internet. For inbound connectivity, this is where Kubernetes Services come into play; you could have a Service of type NodePort (unique port forwarded by kube-proxy on every node in the Kubernetes cluster) or a Service of type LoadBalancer (which uses a cloud load balancer with nodes & NodePorts as registered backends).

Having now covered the Kubernetes concepts, Lees shifts his focus to concentrate on the "connection points" between Kubernetes and OpenStack (the underlying cloud provider in this particular case). These connection points are provided by the OpenStack cloud provider in Kubernetes (enabled via the `--cloud-provider` and `--cloud-config` flags). Lees shares the story of how his experimentation with OpenStack and Kubernetes in 2014 led to implementing the OpenStack provider for Kubernetes in 2014.

The first connection point Lees discusses in detail regards instances (compute). This requires the Nova Compute v2 API. One challenge in the Kubernetes provider is that the instance ID isn't necessarily unique and isn't resolvable via DNS (generically). To help address this, the Kubernetes provider requires the node name to be the same as the OpenStack instance name (which is not the same as the hostname or the instance ID). This is due, in part, to how the Kubernetes provider determines IP addresses for the node. (Side note: Kubernetes isn't yet very IPv6-friendly, so Lees recommends avoiding putting IPv6 addresses on Kubernetes nodes.)

The next connection point is zones, where Kubernetes looks up the region and availability zone. This information is copied into a label (Lees doesn't say but I assume these labels are assigned to the nodes).

Load balancing is the next connection point that Lees reviews. This integration is based on Service objects specified of "type=LoadBalancer" (as described earlier). LBaaS v1 and v2 are supported, but Kubernetes 1.9 removes LBaas v1 support. Lees points out that this portion of code is quite complex.

Next up, Lees talks about the "routes" portion of the provider. This requires the Neutron "extraroute" extension, and implements "kubenet" networking using Neutron routers. This adds routes to the Neutron router for each node's pod subnet, and adds entries into the node's allowed-address pairs. (It's worth noting that kubenet is deprecated in favor of CNI.)

The Cinder volume plugin isn't technically part of the Kubernetes provider, but use the Kubernetes provider to gather information and support OpenStack integration. The plugin doesn't yet support Cinder v3; for v1 and v2 implementations, it attaches/detaches volumes from a VM as required for scheduled pods. This plugin does support dynamic provisioning (creating/deleting volumes on the fly).

The last connection point that Lees discusses is the Keystone password authentication plugin that sets up an authenticator to work against Keystone. Lees points out that this is pretty much a terrible idea, and recommends not using this approach. Lees is also careful to point out that this integration point does _not_ bring multi-tenancy to Kubernetes.

OK, so what does all this mean? Lees shifts focus now to try to pull all this information together. First, Lees recommends using Magnum if it's available in your OpenStack cloud. If Magnum isn't available, Lees says that the OpenStack Heat `kube-up` script is unmaintained and probably should be avoided.

Next, Lees provides some recommendations for deployment:

* A dedicated Kubernetes cluster for each (hostile) tenant (don't mix tenants in a single Kubernetes cluster)
* Use three controller VMs (ideally spread across availability zones; minimum of three in order to form a majority for etcd)
* Spread worker VMs across availability zones, and put them all into one Neutron private network
* Use fewer, larger VMs for worker nodes (instead of many smaller VMs)
* Set up an LBaaS load balancer to handle the API access (since there are multiple master VMs)---this is necessary for the worker nodes to come up properly
* Set up a separate LBaaS and a floating IP network for service access
* Try to avoid Flannel or Weave when running on Neutron; instead, shoot for Kubenet (deprecated), Calico, or Flannel (Host-GW)
* Lees seems less enthusiastic about Calico as opposed to Flannel with the host gateway backend

Lees provides a sample `cloud.conf` configuration (used to configure the OpenStack provider for Kubernetes), and mentions the Cinder API support and how to work around it (this won't be necessary in a year). 

Looking ahead to future work, Lees talks about efforts to make things more automatic with smart defaults. Development efforts are also working to move stuff out of `cloud.conf` into per-object annotations. Within the Kubernetes community, a lot of work is happening around moving cloud providers out of the core Kubernetes code base, and obviously the OpenStack provider would be affected by this effort.

Lees points out some ways to provide feedback and contact the team that is working on the OpenStack provider for Kubernetes. He then wraps up the session.
