---
author: slowe
categories: Explanation
comments: true
date: 2012-07-05T17:06:26Z
slug: using-puppet-with-multiple-operating-systems
tags:
- Automation
- BSD
- Interoperability
- Linux
- Puppet
- UNIX
title: Using Puppet with Multiple Operating Systems
url: /2012/07/05/using-puppet-with-multiple-operating-systems/
wordpress_id: 2668
---

Over the last few days, I've been working with [the open source edition of Puppet](http://puppetlabs.com/puppet/puppet-open-source/), the configuration management/automation tool. I've learned quite a bit, but I still have a long, long way to go. What I wanted to share in this post is what I learned in using Puppet with clients using different operating systems (OSes). If you are a Puppet expert, I'd love to hear any tips and tricks you might have to help me improve.

For now, I won't go into the all the details involved in setting up a Puppet master and Puppet client systems, as that process is reasonably well covered elsewhere. (Although not directly related to Puppet specifically, Jonas Rosland has some instructions for installing Puppet on Ubuntu Server 12.04 LTS [here](http://purevirtual.eu/2012/07/how-to-get-started-with-razor-and-puppet-part-1/).) If you're not already familiar with getting Puppet up and running---in a very basic configuration, at least---then have a look at one of the many tutorials that are available.

For my testing, I used the following environment:

* I chose to use Ubuntu Server 12.04 LTS for my Puppet master server. It was assigned a static IP address.

* I used client systems running OpenBSD 5.1, Ubuntu Server 12.04 LTS, and CentOS 5.8. These systems were assigned dynamic IP addresses via DHCP.

* Both the Puppet master server as well as the clients were VMs running under VMware Fusion 4.1.2 on Mac OS X 10.6.8.

* I already had a solid DNS infrastructure in place (using BIND on OpenBSD as the master with a slave running Mac OS X Server 10.6.8), so I leveraged that for my test environment.

For the purposes of this first attempt at using Puppet, I decided I would try to automate the configuration of NTP on the client systems. Before starting the testing, I took care of the SSL certificates (using `puppet agent --test` on the client side to perform an initial connection to the Puppet master server, then `puppet cert sign` on the Puppet master server to sign the client certificates). This accomplished two things:

1. It verified that DNS resolution was working and that the clients could communicate with the Puppet master server.

2. It took care of getting the SSL certificates signed and in place.

Now that all the preliminaries are out of the way, let's get into my specific configuration, and what I want to share with you. If you've looked at Puppet at all, you'll recall that the "trifecta," so to speak, of Puppet manifests (a manifest is Puppet's declarative configuration file) is the use of the file-package-service combination of resources, like this simple example:

``` puppet
file { "ntp.conf" :
  path            => "/etc/ntp.conf",
  owner           => "root",
  group           => "root",
  mode            => 644,
  source          => "puppet:///modules/ntp/ntp.conf",
}

package { "ntp":
  ensure          => installed,
}

service { "ntp":
  require         => File["ntp.conf"],
  subscribe       => File["ntp.conf"],
  ensure          => running,
}
```

Obviously, this is just an example; you'd want/need to customize the values specified above for the various resources for your particular installation. The point is that it's very common (as I understand it) to have this file-package-service combination in Puppet manifests.

This is all well and good, but what about when you need to manage multiple OSes with Puppet? What do you do then? Consider, for example, the OSes involved in my environment: OpenBSD, Ubuntu, CentOS. I know for certain that OpenBSD's implementation of NTP uses a different configuration file than Ubuntu's NTP package. How would one handle that?

Dominic Cleal's blog gave me [the kickstart I needed](http://m0dlx.com/blog/Puppet_manifests__a_multi_OS_style_guide.html) to get started down the path to a multi-OS manifest. At first I experimented with conditionals in the manifest, like this:

``` puppet
file { "ntp.conf":
  path            => $operatingsystem ? {
    "OpenBSD"     => "/etc/ntpd.conf",
    default       => "/etc/ntp.conf",
  },
  owner           => "root",
  group           => $operatingsystem ? {
    "OpenBSD"     => "wheel",
    default       => "root",
  },
  mode            => 644,
  source          => [ "file:///modules/ntp/ntpd.conf.$operatingsystem",
                       "file:///modules/ntp/ntp.conf", ],
}
```

That worked fine for the file definition, but it broke down when I got to the package definition. Why? OpenBSD packages require that you define a source, but other platforms don't necessarily need (or want) a source defined.

After futzing around with various conditionals, I finally read the rest of Dominic's post, which suggested the use of subclasses for various operating systems. Now that I had read through pages and pages of Puppet manuals and visited site after site trying to figure out how to make this work, the idea of subclasses made sense. So I created an `ntp` module (or class) with subclasses named `ntp::common` and `ntp::openbsd`, using the syntax Dominic shared.

Sadly, it didn't work. When the Puppet agent tried to apply the configuration, it complained that the "ntp.conf" file resource had already been defined once in the `ntp::common` section and couldn't be defined again in the `ntp::openbsd` section.

Never one to give up so easily (and thanks for encouraging words on Twitter, Cody--"Simple is for the weak. You've got this!"), I turned to the #puppet IRC channel, where I was enlightened as to my mistake---my syntax was off. Instead of using the syntax to define a resource in the subclasses, I needed to use the syntax to refer back to an already-defined class.

Let's assume I used this sort of definition in `ntp::common`:

``` puppet
file { "ntp.conf":
  path            => "/etc/ntp.conf",
  owner           => "root",
  group           => "root",
  mode            => 644,
  source          => "puppet:///modules/ntp/ntp.conf",
}
```

If that's the case, then _this_ is the syntax I needed in the `ntp::openbsd` subclass:

``` puppet
File["ntp.conf"] {
  path            => "/etc/ntpd.conf",
  group           => "wheel",
  source          => "puppet:///modules/ntp/ntpd.conf.openbsd",
}
```

See what that does? By using the `File["ntp.conf"]` syntax instead of the `file { "ntp.conf" }` syntax, I simply referred back to an existing resource and overrode the previously-defined values. This allowed me to create OS-specific subclasses with OS-specific settings. In turn, that allows me to manage different OSes using a single module with subclasses. If I ever need to add a new OS to be managed, I can simply add another subclass with the OS-specific settings and I'm off to the races.

Cool, huh? (And not too terribly shabby for a beginner, I think!)

If you're interested, you can get the full manifest (or module) that I created [here](http://pastebin.com/kmvNE1A4). You'll note that I'm using the Puppet file serving mechanism, which I understand is not "best practices" any longer; I'll probably update it soon to the new module-based mechanism. Puppet experts are encouraged to share any ideas/tips/tricks that might further improve this method, and everyone is welcome to share their (courteous) comments. Enjoy!
