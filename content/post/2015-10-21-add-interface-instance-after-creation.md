---
author: slowe
categories: Tutorial
comments: true
date: 2015-10-21T11:30:00Z
tags:
- Networking
- OpenStack
- CLI
- Linux
title: Adding an Interface to an OpenStack Instance After Creation
url: /2015/10/21/add-interface-instance-after-creation/
---

In this post I'll share a few commands I found for adding a network interface to an OpenStack instance _after_ launching the instance. You could, of course, simply launch the instance with multiple network interfaces from the very beginning, but these commands are handy in case you messed up or in case the requirements for the instance changed after it was launched. Please note there's nothing revolutionary or ground-breaking in the commands listed here; I'm simply trying to help share information in the event others will find it useful.

I tested these commands using OpenStack "Juno" with VMware NSX providing the networking functionality for Neutron, but (as you can tell if you check the articles in the "References" section) this functionality has been around for a while. These commands _should_ work with any supported Neutron plug-in.

First, create the Neutron network port:

	neutron port-create <Neutron network name>

If you want to attach a security group to the port (probably a good idea), then modify the command to look like this:

    neutron port-create --security-group <Security group name> \
    <Neutron network name>

Note that you can add multiple `--security-group` parameters to the command in order to specify multiple security groups on the port.

You'll note in the output of the `neutron port-create` command it will tell you the IP address that has been assigned to this new port. Use the IP address to determine the new port's unique ID with this command:

    neutron port-list | awk '/<IP address>/ {print $2}'

I find it helpful to assign the port ID to an environment variable like this:

    PID=$(neutron port-list | awk '/<IP address>/ {print $2}')

Once you have the port's ID, you can attach the port to the instance:

	nova interface-attach --port-id $PID <Nova instance name>

This command assumes you assigned the port ID to an environment variable; if you didn't, simply supply the port ID you obtained earlier.

The new port will show up almost immediately in the instance; you can see it using `ip link list`. Note that you'll still need to perform any OS-specific configuration to make the interface fully functional.

If you later find out you no longer need this additional port attached to the instance, you can detach it with a command like this:

	nova interface-detach <Nova instance name> <Port ID>

(By the way, note the different order of the parameters in the `nova interface-attach` and `nova interface-detach` commands. The position of these parameters is, in fact, important.)

## References

I found the following sites extremely helpful in figuring out the commands shown in this post:

[OpenStack Interface Hot Plugging][link-1]  
[Configure Multiple Network Interfaces on an OpenStack Instance][link-2]  



[link-1]: http://blog.aaronorosen.com/openstack-interface-hot-plugging/
[link-2]: http://thornelabs.net/2014/09/03/configure-multiple-network-interfaces-on-an-openstack-instance.html
