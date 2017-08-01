---
author: slowe
categories: Tutorial
comments: true
date: 2013-08-21T23:38:41Z
slug: accessing-vnc-consoles-of-kvm-guests-via-ssh
tags:
- CLI
- KVM
- Linux
- SSH
- Virtualization
title: Accessing VNC Consoles of KVM Guests via SSH
url: /2013/08/21/accessing-vnc-consoles-of-kvm-guests-via-ssh/
wordpress_id: 3248
---

About a year ago, I published an entry on [working with KVM guests][1], in which I described how to perform some common operations with KVM-based guest VMs (or guest domains). While reading that article today (I needed to refer back to the section on using `virt-install`), I realized that I hadn't discussed how to access the VNC console of a KVM guest. In this post, I'll show you how to use SSH to access the VNC console of a KVM guest domain.

Here are the tools you'll need to make this work:

* An SSH client. On OS X or Linux, this installs with the OS; on Windows, you'll need a client like [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty).

* A VNC client. There are a variety of clients out there; find one that works for you and run with it. On OS X, I use Chicken (formerly Chicken of the VNC).

Basically, all we're going to do is use SSH port forwarding (additional information [here](http://www.debianadmin.com/howto-use-ssh-local-and-remote-port-forwarding.html)) to connect your local system to the VNC port of the guest domain on the KVM host. At its simplest level, the generic command to do local port forwarding with SSH looks something like this:

    ssh <username>@<remote host IP address or DNS name> -L <local port>:<remote IP address>:<remote port>

The idea of local port forwarding via SSH is really well-known and well-documented all over the Internet, so I won't go into a great level of detail here.

Let's take the generic command above and turn it into a command that is specific to accessing the VNC console of a guest domain on a remote KVM host. Let's assume in this example that the IP address of the remote KVM host is 192.168.1.4, that your username on this remote KVM host is admin, and that there is only one guest domain running on the remote KVM host. To access that guest domain's VNC console, you'd use this command:

    ssh admin@192.168.1.4 -L 5900:127.0.0.1:5900

Allow me to break this command down just a bit:

* The first part (`ssh admin@192.168.1.4`) simply establishes the SSH session to the remote KVM host at that IP address.

* The `-L` parameter tells SSH we are doing local port forwarding. In other words, SSH will take traffic directed to the specified local port and send it to the specified remote port on the specified remote IP address.

* The last part (`5900:127.0.0.1:5900`) is probably the part that will confuse folks who haven't done this before. The first port number indicates the local port number---that is, the port on your local system. The IP address indicates the remote IP address that will be contacted _via the remote host_, and the last port number indicates the destination port to which traffic will be directed. It's really important to remember that the IP address specified in the port forwarding specification will be contacted _through the SSH tunnel_ as if the traffic were originating from the remote host. So, in this case, specifying 127.0.0.1 (the loopback address) here means we'll be accessing the remote host's loopback address, not our own.

Once you have this connection established, you can point your VNC client at your _own loopback address_. If you select display 0 (the default on most clients), then traffic will be directed to port 5900, which will be redirected through the SSH tunnel to port 5900 on the loopback address on the remote system---which, incidentally, is where the VNC console of the guest domain is listening. _Voila_, you're all set.

Here are a few additional notes of which you'll want to be aware:

* The first guest domain launched (or started) on a host will listen on port 5900 of the KVM host's loopback address. (This is why we used those values in our command.) The second guest domain will listen on 5901, the third on 5902, and so forth.

* You can use the command `sudo netstat -tunelp | grep LISTEN` to show listening ports on the KVM host; this will include listening VNC consoles. The last column in the output will be the process ID; use `ps ax | grep <process ID>` to figure out which VM is listening to a particular port (the name of the VM will be in the command's output).

When you're accessing the guest domains on the remote KVM host directly, it's pretty straightforward. But what if you need to send traffic through an intermediate jump host first? For example, what if you have to SSH to the jump host first, then SSH from the jump host to the remote hypervisors---can you still access guest domain consoles in this sort of situation?

**Yes you can!**

It's a bit more complicated, but it's doable. Let's assume that our jump host is, quite imaginatively, called `jumphost.domain.com`, and that our hypervisor is called---you guessed it---`hypervisor.domain.com`. The following set of commands would allow you to gain access to the first guest domain on the remote hypervisor via the jump host.

First, run this command from your local system to the jump host:

    ssh user@jumphost.domain.com -L 5900:127.0.0.1:5900

Next, run this command from the jump host to the remote hypervisor:

    ssh user@hypervisor.domain.com -L 5900:127.0.0.1:5900 -g

The `-g` parameter is important here; I couldn't make it work without adding this parameter. Your mileage may vary, of course. Once you have both sets of port forwarding established, then just point your VNC client to port 5900 of your local loopback address. Traffic gets redirected to port 5900 on the jump host's loopback address, which is in turn redirected to port 5900 on the remote hypervisor's loopback address---which is where the guest domain's console is listening. Cool, huh? If you establish multiple sessions listening on different ports, you can access multiple VMs on multiple hypervisors. Very handy!

If you have any questions, corrections, or additional information to share, please feel free to speak up in the comments below.

[1]: {{< relref "2012-08-21-working-with-kvm-guests.md" >}}
