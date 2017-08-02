---
author: slowe
categories: Tutorial
comments: true
date: 2013-08-16T09:00:00Z
slug: learning-nvp-part-2-nvp-controllers
tags:
- Networking
- Nicira
- NVP
- OpenFlow
- OVS
- Virtualization
title: 'Learning NVP, Part 2: NVP Controllers'
url: /2013/08/16/learning-nvp-part-2-nvp-controllers/
wordpress_id: 3239
---

Welcome to part 2 of the Learning NVP blog series. In [part 1][1] of the series, I provided a high-level overview of the NVP architecture and components. In this post, I'll dive into a bit more detail on one key component of the architecture: the NVP controllers and controller cluster.

(Just a quick reminder: although I'm talking about NVP 3.1--our publicly available, GA product already running in production with customers---it's important to remember that the NVP architecture serves as the basis for the upcoming VMware NSX product. Time spent learning NVP will be beneficial in understanding NSX later.)

I'll start with first reviewing the role of the NVP controllers in the overall architecture.

## Reviewing the Role of the NVP Controllers

As I mentioned in [part 1][1], the purpose of the NVP controllers is to handle computing the network topology and providing configuration and flow information to create logical networks. They do this by managing all [Open vSwitch (OVS)](http://openvswitch.org/) devices and enforcing consistency between the logical network view (which is defined via the northbound NVP API) and the transport network view as implemented by the programmable edge virtual switches.

To break this down a bit more:

* If a change occurs in the transport network---this would be something like a VM starting, a VM powering off, or a VM migrating to a different host---the controllers update the necessary forwarding rules and state in the transport network so that the connectivity of a VM is consistent with the logical view. As an example, a new VM powers on that should be part of a particular logical network, the controller cluster will update the necessary forwarding rules such that the new VM has the connectivity appropriate for a member of that particular logical network.

* Similarly, if an API request changes the configuration of the logical port used by a VM, the controllers modify the forwarding rules and state of all relevant devices in the transport network to ensure consistency with the API-driven change.

NVP requires 3 controllers configured as a controller cluster; this provides the ability to distribute working tasks across the different controllers as well as provides the necessary high availability that is needed for the functions provided by the controllers.

OK, now that I've reviewed the role of the controllers, let's look at the controller build process.

## Building the NVP Controllers

VMware distributes the NVP controller software as an ISO. In a production deployment, you would use this ISO to burn optical discs that, in turn, are used to build NVP controllers on bare metal servers. The system requirements for the NVP controllers, as outlined in the NVP 3.1 release notes, are 8 CPU cores, 64-bit CPUs, 64GB of RAM, 128GB of local hard disk space, and a NIC of at least 1 Gbps. While it's certainly possible to run the NVP controllers as VMs (this is what I'm doing), this is not currently supported for production environments.

The NVP controllers run a build of Ubuntu Server 12.04, so when you boot a system from the ISO (or from an optical disc created from the ISO) it will run through a custom install of Ubuntu Server 12.04. The NVP controller software packages are slipstreamed into the install---if you're careful you'll see references to them during the install process---so when the custom Ubuntu installer is done you're left with a blank NVP controller that is ready to configure.

Once you've booted from the install media and installed the NVP controller software, getting an NVP controller up and running is actually pretty straightforward. Here are the steps:

1. Set the password for the admin user (optional, but highly recommended).

2. Set the hostname for the controller (also optional, but highly recommended).

3. Assign an IP address to the controller so that it can communicate across the network. (Obviously this is a fairly important step.)

4. Configure DNS and NTP settings.

5. Set the controller's management IP address (more on this in a moment).

6. Set the controller's switch manager and API provider IP addresses (more on that in a moment).

Let's take a look at these steps in a bit more detail. The NVP controller offers users a streamlined command-line interface (CLI) with context-sensitive help. If you get stuck with any of the commands, just press Tab to autocomplete the command, or press Tab twice to provide a list of completion options.

To set the password for the default admin user, just use this command:

    set user admin password

You'll be prompted to supply the new password, then retype it for confirmation. Easy, right? (And pretty familiar if you've used Linux before.)

Setting the hostname for the controller is equally straightforward:

    set hostname <hostname>

Now you're ready to assign an IP address to the controller. Use this command to see the network interfaces that are present in the controller:

    show network interfaces

You'll note that for each physical interface in the system, the NVP installation procedure will create a corresponding bridge (this is actually an OVS bridge). So, for a server that has two interfaces (`eth0` and `eth1`), the installation process will automatically create `breth0` and `breth1`. Generally, you'll want to assign your IP addresses to the bridge interfaces, and not to the physical interfaces.

Let's say that you wanted to assign the IP address to `breth0`, which corresponds to the physical `eth0` interface. You'd use this command:

    set network interface breth0 ip config static 192.168.1.5 255.255.255.0

Naturally, you'd want to substitute the correct IP address and subnet mask in that command. Once the interface is configured, you can use the standard `ping` command to test connectivity (note, though, that you can't use any switches to ping, as they aren't supported by the streamlined NVP controller CLI).

Note that you may also need to add a default route using this command:

    add network route 0.0.0.0 0.0.0.0 <em><Default gateway IP address></em>

Assuming connectivity is good, you're ready to add DNS and NTP servers to your configuration. Use these commands:

    add network dns-server <DNS server IP address>  
    add network ntp-server <NTP server IP address>

Repeat these commands as needed to add multiple DNS and/or NTP servers. If you mess up and accidentally fat finger an IP address (happens to me all the time!), you can remove the incorrect IP address using the `remove` command, like this:

    remove network dns-server <em><Incorrect DNS IP address></em>

Substitute `ntp-server` for `dns-server` in the above command to remove an incorrect NTP server address.

It's entirely possible that an NVP controller could have multiple IP addresses assigned, so the next few commands will tell the controller which IP address to use for various functions. This allows you to spread certain traffic types across different interfaces (and potentially different networks), should you so desire.

First, set the IP address the controller should use for management traffic (this address should be an address assigned to one of the interfaces):

    set control-cluster management-address <IP address>

Then tell the NVP controller which IP address to use for the switch manager role (this is the role that communicates with OVS devices; again, this should be an address already assigned to one of the controller's interfaces):

    set control-cluster role switch_manager listen-ip <IP address>

And tell the controller which IP address to use for the API provider role (this is the role that handles northbound REST API traffic). As before, this should be an IP address assigned to one of the controller's interfaces:

    set control-cluster role api_provider listen-ip <IP address>

Once all this is done, you're ready to turn up the controller cluster using the `join control-cluster` command. For the first controller, you'll "join" it to itself. In the event the NVP controller has multiple IP addresses assigned, the IP address to use is the IP address you specified when you set the management IP address earlier. Here's the command to build a controller cluster from the **first** controller:

    join control-cluster <Own IP address>

For the **second and third** controllers in the cluster, you'll point them to the IP address of the first controller in the cluster, like this:

    join control-cluster <IP address of first controller>

(Side note: you can actually point the third controller to _any_ available node in the cluster. I specified the first controller here just for succinctness.)

Once the process of joining the controller cluster is done, you can check the status of the cluster in a couple of different ways. First, you can use the `show control-cluster status`, which will tell you if this node is connected to the cluster majority (as well as if this controller can be safely restarted). You can also use the `show control-cluster startup-nodes` command, which lists all the controllers that are members of the cluster.

The output of both these commands is illustrated below.

![Status and startup nodes commands](/public/img/show-control-cluster-output.png)

If you want to get a feel for the types of communication the NVP controllers will use, you can also use the `show control-cluster connections` command, which produces output that looks something like this:

![Connections summary](/public/img/show-control-cluster-output-2.png)

Once the controller cluster is up and running, you're ready to move on to adding other components of NVP to the environment. In the next part, I'll walk through setting up NVP Manager, which will then allow us to continue with setting up NVP by adding gateways, service nodes, and hypervisors.

In the meantime, feel free to post any questions or thoughts in the comments below. Courteous comments (with vendor disclosure, where applicable) are always welcome.

[1]: {{< relref "2013-05-21-learning-nvp-part-1-high-level-architecture.md" >}}
