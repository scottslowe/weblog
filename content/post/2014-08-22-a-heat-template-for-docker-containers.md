---
author: slowe
categories: Explanation
comments: true
date: 2014-08-22T17:10:36Z
slug: a-heat-template-for-docker-containers
tags:
- Automation
- Docker
- Linux
- OpenStack
- Interoperability
- Virtualization
title: A Heat Template for Docker Containers
url: /2014/08/22/a-heat-template-for-docker-containers/
wordpress_id: 3503
---

In this post, I'll share a simple template for deploying [Docker](http://www.docker.com/) containers in an OpenStack environment using Heat. Given that Docker is targeted at application deployment, then I felt that using Heat was a more appropriate way of leveraging Docker in an OpenStack environment as opposed to treating Docker as a form of a hypervisor. Later in this post, I'll compare this approach to using a more container-aware solution such as fleet.

I assume you're already familiar with OpenStack Heat and Docker. If you aren't, take a look at these articles first:

* [An Introduction to OpenStack Heat][1]

* [Another Look at an OpenStack Heat Template][2]

* [A Quick Introduction to Docker][3]

## Prerequisites

Before you can actually use Heat to orchestrate Docker containers, there are some prerequisites you'll need to have done first:

1. You'll need to have the Docker plugin for Heat installed. This can be tricky; see [here][5] for some instructions that worked for me. To verify that the Docker plugin is working as expected, run `heat resource-type-list` and check the output for "DockerInc::Docker::Container". If that resource type is included in the output, then the Docker plugin is working as expected.

2. Any Docker hosts you're running must have Docker configured to listen on a network-accessible socket. I was running CoreOS in my environment, so I followed the instructions [to make Docker on CoreOS listen on a TCP socket](https://coreos.com/docs/launching-containers/building/customizing-docker/#enable-the-remote-api-on-a-new-socket). (In case the link doesn't take you to the right section, see the section titled "Enable the Remote API on a New Socket.") In my case, I selected TCP port 2345. Make note of whatever port you select, as you'll need it in your template.

3. Any Docker hosts that will be orchestrated by Heat must have an IP address assigned that is reachable from the server where Heat is running (typically the cloud controller). In my case, I use Neutron with NSX, so I had to assign floating IPs to the instances with which Heat would be communicating.

4. You'll need to be sure that the TCP port you select for Docker to use (I used TCP port 2345) is accessible, so modify any security groups assigned to the instances to allow inbound TCP traffic on that port from the appropriate sources.

Once these prerequisites are addressed---Docker plugin installed and working, Docker listening on a TCP port, instance reachable from cloud controller on selected TCP port---then you're ready to go.

## Template for Docker Orchestration

Here is a sample template that will create a Docker container on an existing instance:

```yaml
heat_template_version: 2013-05-23
description: >
  Heat template to deploy Docker containers to an existing host
resources:
  nginx-01:
    type: DockerInc::Docker::Container
    properties:
      image: nginx
      docker_endpoint: 'tcp://192.168.1.207:2345'
```

(Click [here](https://gist.github.com/scottslowe/2c22e5548910e5717f12) for a downloadable version of the code block above.)

As I said, this is pretty simple. The `image` property is the name of the Docker image you want to use; in this case, I'm using an image containing the popular Nginx web server. The `docker_endpoint` property should be a URL that specifies the protocol (TCP), IP address (in my case, a floating IP address assigned to the instance), and the port number on which the Docker daemon is listening. Note that the format for this property isn't documented _anywhere_ I've found.

In the "stable/icehouse" branch of the Docker plugin (required if you're using distro packages for your OpenStack installation, as I am), there are some additional properties available as well. Unfortunately, without **any** documentation on what these properties should look like, I was unable to make it work with any of those properties included. In particular, the `port_specs` property, which controls how ports in a Docker container are exposed to the outside world, would have been very useful and applicable. However, I was unable to make it work with the `port_specs` attribute included. If anyone has information on the exact syntax and format for the `port_specs` property in the "stable/icehouse" branch of the plugin, please speak up in the comments.

Naturally, you could embed this portion of YAML code into a larger HOT-formatted template that also launched instances, created Neutron networks, attached the instances to Neutron networks, created a logical router, and mapped a floating IP address to the instance. I leave the creation of such a template as an exercise for the reader, but I will point out that I've already shared with you almost all the pieces necessary to do exactly that. (See the blog posts I provided earlier.)

## Summary

I mentioned at the start of this post that I'd provide some comparison to other methods for deploying containers in an automated fashion. With that in mind, here are a few points you'll want to consider:

* _There is no container scheduling in this solution._ Containers are statically mapped to a container host (the VM instance, in this case, although this could be a bare metal host running Docker as well). Other solutions, like fleet, at least let you just point to a cluster of systems instead of a specific system. (See [this write-up on fleet][4] for more information.)

* _Docker must be listening on a TCP socket._ This isn't Docker's default configuration, so this is an additional change that must be incorporated into the environment. Fleet doesn't have this requirement, although other solutions such as Mesos might (I haven't tested any other solutions--_yet_.)

* _There is **very** little documentation available right now._ Note that this may be true for other solutions as well (this entire space is relatively new and growing/evolving rapidly). Regardless, until someone can at least figure out how to expose Docker containers to the network via a Heat template, this isn't very useful.

My initial assessment is that OpenStack needs container scheduling, not static assignment, in order for Docker integration into OpenStack to be truly useful. Proponents of the Nova-Docker approach (treating Docker as a hypervisor and Docker images as Glance images) point to their approach as superior because of the integration of Nova's scheduling functionality. It will be interesting to see how things develop on this front.

If you have any questions, have more information to share, or have corrections or clarifications to any of the information presented here, please speak up in the comments.

[1]: {{< relref "2014-05-01-an-introduction-to-openstack-heat.md" >}}
[2]: {{< relref "2014-05-02-another-look-at-an-openstack-heat-template.md" >}}
[3]: {{< relref "2014-03-11-a-quick-introduction-to-docker.md" >}}
[4]: {{< relref "2014-08-20-coreos-continued-fleet-and-docker.md" >}}
[5]: {{< relref "2014-07-31-installing-the-docker-plugin-for-heat.md" >}}
