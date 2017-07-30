---
author: slowe
categories: Liveblog
comments: true
date: 2010-02-11T12:28:47Z
slug: partner-exchange-2010-session-techmgt0921
tags:
- Virtualization
- VMware
title: Partner Exchange 2010 Session TECHMGT0921
url: /2010/02/11/partner-exchange-2010-session-techmgt0921/
wordpress_id: 1832
---

This is a session on vCenter Chargeback deployment, configuration, and best practices. The presenter is Naeem Malik from VMware. Naeem works in VMware's PS organization and specializes in Chargeback, CapacityIQ, and Capacity Planner assessments.

There are three things to keep in mind when you are thinking about a vCenter Chargeback implementation: hierarchy, cost models, and cost templates. Malik will discuss those in more detail later.

So why chargeback? Chargeback is necessary to handle the new model of shared resources instead of dedicated physical servers. There is no longer a one application-to-one server model; now many applications run on the same server. Why is this new model necessary? Malik quotes Gartner that "the speed and flexibility of virtualization makes some form of chargeback mandatory". Otherwise, organizations run the risk of VM sprawl. After all, VMs are not free---they require CPU, memory, storage, and network capacity.

vCenter Chargeback is a resource accounting tool that helps users and organizations understand that VMs are not free. It features support for fixed, allocation, and utilization-based costing; provides the ability to charge different amounts for different tiers of infrastructure; and can schedule reports and e-mail results.

In a vCenter Chargeback implementation, Malik believes that a large part of a consultant's time is taken up helping organizations define the resource costing (if the organization has not already established those costs).

From an architectural perspective, vCenter Chargeback uses a separate database but also pulls information from the vCenter Server database. vCenter Chargeback can run as a VM and integrates with an organization's existing e-mail systems (via SMTP) and existing Active Directory/LDAP infrastructures. Both SQL Server and Oracle are supported for Chargeback. The Chargeback server will interact with vCenter Server to pull performance and utilization information.

A single Chargeback server supports up to 5 vCenter Server instances and up to 5,000 VMs/entities. An embedded data collector is found on the Chargeback server itself. When an implementation goes beyond 5 vCenter Server instances, an additional data collector is necessary. The data collectors can be easily deployed from the Chargeback server itself. Extremely large implementations (up to 75 vCenter Server instances and up to 20,000 VMs/entities) require multiple Chargeback servers behind a load balancer with multiple data collectors.

Chargeback uses HTTP over TCP port 8080, HTTPS over TCP port 443, load balancing configuration over TCP port 8009, LDAP over TCP port 389, and SMTP over TCP port 25 (the slide had a typo and listed port 24).

Earlier Malik had mentioned three things to keep in mind. The first of these is hierarchy. The hierarchy controls how reports are created. The second thing to keep in mind is the cost model. There is fixed costing (fixed cost for a VM instance), allocation-based costing (variable costs per VM based on allocated resources), and utilization costing (variable cost per VM based on actual resources utilized). Many customers are using a hybrid model that is somewhere between fixed costing and allocation-based costing. The third thing is cost templates. Cost templates combine cost accounting information with fixed costs.

Cost accounting works with the cost model to determine how resources are actually priced. If a customer hasn't already determined costs for their resources (CPU, RAM, storage, networking), VMware has a tool that can help determine this number. This can be difficult and requires "buy in" from all applicable stakeholders within the environment. Items that require costs assigned to them include CPU, memory, disk, disk I/O, and network I/O.

Fixed costs (not the same as the fixed cost model) include stuff like power/cooling, software licenses, real estate in the data center, labor, etc.

Cost templates combine the cost accounting for the various resources with the fixed costs and allow you to apply a cost multiple to the metering element (GHz of CPU cycles used, GB of RAM used, etc.). Multiple cost templates can be created to help allow for flexible costing of VMs.

vCenter Chargeback also has extensive reporting functionality. Scheduled reports are available, and reports can be generated at any point within the hierarchy (datacenter object, cluster object, host object). Reports can be customized for a company-specific look and feel. Reports are available via e-mail or via the Chargeback web UI.

At this point, Malik now moves into a product "demo", which is essentially a collection of screenshots from vCenter Chargeback that help illustrate the various components and features of Chargeback.
