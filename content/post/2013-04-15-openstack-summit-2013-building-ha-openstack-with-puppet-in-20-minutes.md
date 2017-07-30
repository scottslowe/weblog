---
author: slowe
categories: Liveblog
comments: true
date: 2013-04-15T19:11:47Z
slug: openstack-summit-2013-building-ha-openstack-with-puppet-in-20-minutes
tags:
- Automation
- OpenStack
- Puppet
title: 'OpenStack Summit 2013: Building HA OpenStack with Puppet in 20 Minutes'
url: /2013/04/15/openstack-summit-2013-building-ha-openstack-with-puppet-in-20-minutes/
wordpress_id: 3138
---

This is a liveblog of a session on using Puppet to build a highly available (HA) installation of OpenStack. The presenter is Boris Renski of Mirantis.

Boris believes that you need to know many things in order to successfully create the build architecture for an OpenStack deployment:

* Linux
* Networking
* Virtualization
* Python
* Ruby
* Puppet
* Cobbler (or Razor)
* mCollective/Salt

Boris introduces Fuel, which is an automation library for OpenStack (that is supposedly easy enough for a goat to use---a play on Mirantis' geographical location and the Borat movies).

Fuel essentially includes the following items:

* An OpenStack reference architecture with HA

* Puppet manifests for deploying OpenStack

* Cobbler-based bare metal provisioning

* OpenStack packages

* Support for CentOS, RHEL, and Ubuntu

* Support for OpenStack Essex, Folsom, and (in May) Grizzly

* A detailed configuration guide

Fuel supports a number of different deployment configurations: single node (pretty straightforward, much like DevStack); multi-node (including compact Swift and standalone Swift); and multi-node HA (with compact and standalone Swift and Quantum elements). "Swift compact" is for when Swift will be used only as back-end storage for VMs. "Quantum compact" is running Quantum on the controller node, even with high availability.

Fuel was specifically created in the form of a library so that users could easily modify and adopt the scripts to fit their particular OpenStack deployment. This gives users more flexibility when using Fuel to deploy OpenStack.

For the HA architecture:

* They use HAProxy for most of the OpenStack services

* For the message queue, they use an active/active RabbitMQ cluster

* For the database, they use an active/active Galera MySQL cluster (this forces a minimum of four physical nodes)

* The architecture uses `keepalived` for VIP (virtual IP) management

The overall process for deploying OpenStack with Fuel goes like this:

1. Build the Fuel "master node," which runs Cobbler and Puppet master

2. Enter hardware info into Cobbler

3. Cobbler installs the base OS (CentOS, RHEL, Ubuntu)

4. Puppet picks up the node and installs/configures OpenStack

Next, Boris goes into a more light-hearted section on how he taught a goat to use Fuel. For us humans, this means the "Fuel portal," which provides step-by-step instructions on using Fuel. They (Mirantis) also created "Fuel Web," which is an easy GUI for Fuel. A private beta for Fuel Web is starting today.

Boris now turns the stage over to Roman, who shows a live demo of using Fuel to turn up a 2-node OpenStack deployment. Overall, Fuel looks like a very interesting and useful tool.

Looking ahead, what's on the Fuel roadmap? Roman wants to add screens for the management of disks and NICs, which don't exist in Fuel Web today. There's also no support for Cinder in the web UI today, which is another item they'd like to add in future releases. They are also considering some level of monitoring and performance metrics for the OpenStack environments deployed using Fuel. Finally, they want to extend Fuel to help with OpenStack upgrades as well.

Fuel is available for download at [http://fuel.mirantis.com](http://fuel.mirantis.com).
