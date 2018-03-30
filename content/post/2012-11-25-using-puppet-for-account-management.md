---
author: slowe
categories: Tutorial
comments: true
date: 2012-11-25T09:03:11Z
slug: using-puppet-for-account-management
tags:
- Automation
- Linux
- OSS
- Puppet
title: Using Puppet for Account Management
url: /2012/11/25/using-puppet-for-account-management/
wordpress_id: 2955
---

It's been over four months since my last post on [open source Puppet](http://puppetlabs.com/puppet/puppet-open-source/), when I posted some [updated multi-OS Puppet configurations][1]. I'm back from a long Puppet hiatus with some information on using Puppet for managing user accounts.

It was pretty tough coming back to Puppet after a four-month break (during which time I've been spending plenty of time on KVM, Open vSwitch, and libvirt), and it was almost like learning Puppet again from scratch. Fortunately, with the help of a few folks from Twitter and the #puppet IRC channel, I was able to get managing user accounts with Puppet to work (and learned a little bit in the process).

The key to making it work is the idea of a virtual resource. In Puppet, you are normally only allowed to define a resource once; you'd then apply that same resource against multiple systems. However, if you define a user resource for one system, you're then prevented from defining the same user resource for another system---the resource has already been defined. With a virtual resource, though, you can define the resource once (as a virtual resource; denoted with an "@" in front of the resource). Once a virtual resource has been defined, you can then _realize_ (or instantiate) that resource as many times as needed across multiple systems. (At least, that's how I understand it. I'm still wrapping my head around some of this stuff.) The Puppet Labs site does a much better job of [explaining virtual resources](http://docs.puppetlabs.com/puppet/3/reference/lang_virtual.html).

Now that you're armed with that little bit of background information, let's get started on making this work. There are 2 basic steps involved:

1. Create a class with a defined type that creates the virtual user resources.

2. Realize the virtual user resources where needed.

Let's tackle that first step: creating the class and defined type.

Essentially what you're going to do to create the class and defined type is create a module. As such, we'll want to follow [Puppet's guidelines for module structures](http://docs.puppetlabs.com/puppet/3/reference/modules_fundamentals.html). In my case, I created a directory for the class (I called mine `accounts`), with all the suggested subdirectories for a module, even if most of them weren't needed. From there, I only need to create two files, both in the `accounts/manifests` directory: `init.pp` and `virtual.pp`.

I'll come to the `init.pp` in a moment; somewhat counter-intuitively, we'll start with the `virtual.pp` manifest first. Here's the contents of that manifest (download this from GitHub [here][gist-1]):

``` puppet
# Defined type for creating virtual user accounts
#
define accounts::virtual ($uid,$realname,$pass) {

  user { $title:
    ensure            =>  'present',
    uid               =>  $uid,
    gid               =>  $title,
    shell             =>  '/bin/bash',
    home              =>  "/home/${title}",
    comment           =>  $realname,
    password          =>  $pass,
    managehome        =>  true,
    require           =>  Group[$title],
  }

  group { $title:
    gid               =>  $uid,
  }

  file { "/home/${title}":
    ensure            =>  directory,
    owner             =>  $title,
    group             =>  $title,
    mode              =>  0750,
    require           =>  [ User[$title], Group[$title] ],
  }
}
```

I used the latest version of `puppet-lint` to ensure that all stylistic recommendations were followed properly. I derived this code from [this example](http://www.craigdunn.org/2011/03/puppet-working-with-define-based-virtuals/), which---once I wrapped my head around the concept---I found quite helpful. Note that there are no virtual resources here; those come in just a moment.

Now that the defined type is done, we can use it to actually create the virtual user resources. We'll actually do that in the `accounts/manifests/init.pp` file, like this (download from GitHub [here][gist-2]):

``` puppet
# Used to define/realize users on Puppet-managed systems
#
class accounts {

  @accounts::virtual { 'johndoe':
    uid             =>  1001,
    realname        =>  'John Doe',
    pass            =>  '<password hash goes here>',
  }
}
```

You'll just repeat that snippet of test as many times as necessary to create a virtual "accounts::virtual" resource for each user account you want to manage within Puppet. Each "accounts::virtual" resource includes a user account, a group account, and a home directory, but we manage all those individual resources as one object through the class definition.

Once you're ready to actually instantiate an virtual resource, you do that with a snippet of code like this (available [here][gist-3] from GitHub):

``` puppet
node default {
}

node 'server.domain.net' {
  include accounts
  realize (Accounts::Virtual['johndoe'])
}
```

Let's break that down real quick:

* This is a standard node definition, typically found in `nodes.pp` or similar.

* Note that the `accounts` class (module) is included; this is the class (module) we created earlier, where we created the virtual user resources. This makes the virtual user resources "available" to this node.

* Then we _realize_ (or actually instantiate/create) the user account on this node with the `realize (Accounts::Virtual['johndoe'])` statement. Pay attention to the capitalized Accounts::Virtual in this code---this refers back to a resource that has already been defined. In this case, it's the virtual resource you defined in the `init.pp` file in the class you established earlier.

With this structure in place, when the node named "server.domain.net" runs Puppet and connects to the Puppet master, it will create the realized resources---user account, group account, and home directory---specified in the node definition. Pretty cool, huh?

I freely admit that I'm still relatively new to Puppet, so I'm sure there are numerous ways this approach could be improved. I tested this code on both CentOS 6.3 as well as Ubuntu 12.04, and it seems to work fine on both platforms. Feel free to submit suggestions for improvement, corrections, or clarifications in the comments below.


[gist-1]: https://gist.github.com/scottslowe/4050213
[gist-2]: https://gist.github.com/scottslowe/4050229
[gist-3]: https://gist.github.com/scottslowe/4050236

[1]: {{< relref "2012-07-09-updated-multi-os-puppet-configuration.md" >}}
