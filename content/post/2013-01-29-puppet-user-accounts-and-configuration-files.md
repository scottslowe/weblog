---
author: slowe
categories: Tutorial
comments: true
date: 2013-01-29T10:04:14Z
slug: puppet-user-accounts-and-configuration-files
tags:
- Automation
- Linux
- OSS
- Puppet
title: Puppet, User Accounts, and Configuration Files
url: /2013/01/29/puppet-user-accounts-and-configuration-files/
wordpress_id: 3079
---

A short while ago I posted an article that described [how to use Puppet for account management][1]. In that article, I showed you how to use virtual user resources to manage user accounts on Puppet-managed systems. In this post, I'm going to expand on that configuration so that we can also manage the initial account configuration files, and do so in the proper order.

One of the things the configuration in [my first post][1] didn't handle was the Puppet configuration of the files in `/etc/skel` and making sure those files were in place _before_ the user accounts were created. As a result, it was possible that the user account could be created on a system before the `/etc/skel` files were updated, and then that user account would have "unmanaged" copies of the initial configuration files. Further Puppet agent runs wouldn't correct the problem, because the files in `/etc/skel` are only copied over _when the account is created._ If the account has already been created, then it's too late---the files in `/etc/skel` _must_ be managed before the accounts are. To fix the issue, you have to ensure that the resources are processed in a specific manner. In this post, I'll show you how to manage that.

There are two parts to extending the Puppet accounts module to also manage some configuration files:

1. Add a subclass to manage the files.

2. Create a dependency between the virtual user resources and this new subclass.

Let's look at each of these.

## Adding a Subclass

To add a subclass to manage the configuration files, I created `config.pp` and placed it in the `manifests` folder for the accounts module. Here's a simplified look at the contents of that file (click [here](https://gist.github.com/scottslowe/4274021) if you'd like to download this code snippet):

``` puppet
class accounts::config {

  # Place a file in /etc/profile.d to manage the prompt
  file { '/etc/profile.d/prompt.sh':
    ensure      =>  'present',
    source      =>  'puppet:///modules/config/profiled-prompt.sh',
    mode        =>  '0644',
    owner       =>  '0',
    group       =>  '0',
  }
}
```

This is pretty straightforward Puppet code; it creates a managed file resource and specifies that the file be sourced from the Puppet master server. The full and actual `accounts::config` subclass that I'm using has a number of managed file resources, including files in `/etc/skel`, but I've omitted that here for the sake of brevity. (The other file resources that are defined look very much like the example shown, so I didn't see any point in including them.) The `config.pp` also uses some values from an `accounts::params` subclass and some conditionals to manage different files on different operating systems.

To really put the subclass to work, though, we have to include it elsewhere. So, in the accounts module's `init.pp`, we add a line that simply states `include accounts::config`. However, the problem that occurs if you stop there is the problem I described earlier: Puppet might create the user account before it places the file resources under management, and then the user account won't get the updated/managed files.

To fix that, we create a dependency.

## Creating a Dependency

Before running into this situation, I was pretty familiar with creating dependencies. For example, if you were defining a class for a particular daemon to run on Linux, you might use the Puppet package-file-service "trifecta", and you might include a dependency, like this (entirely fictional) example. Note in this example that the file resource is dependent on the package resource, and the service resource is dependent on the file resource (as denoted by the capitalized Package and File instances):

``` puppet
class foo {
  package { 'foo':
    ensure      =>  'present',
  }

  file { '/etc/foo.conf':
    ensure      =>  'present',
    source      =>  'puppet:///modules/foo/foo_conf',
    mode        =>  '0600',
    require     =>  Package['foo'],
  }

  service { 'foo':
    ensure      =>  'running',
    require     =>  File['/etc/foo.conf'],
  }
}
```

(My apologies if my syntax for this fictional example isn't perfect---I didn't run it through `puppet-lint`.)

The problem in this particular case, though, is that I didn't need a dependency on a single file; I needed a dependency on a whole group of files. To further complicate matters, the files on which the dependency existed might change between operating systems. For example, I might (and do) have different files on RHEL/CentOS than on Ubuntu/Debian. So how to accomplish this? The answer is actually quite simple: **create a dependency on the subclass, not the individual resources.**

With the dependency on the subclass, the code to define the virtual users looks like this:

``` puppet
# Used to define virtual users on Puppet-managed systems
# Includes subclass dependency on accounts::config
#
class accounts {
 
  @accounts::virtual { 'johndoe':
    uid             =>  1001,
    realname        =>  'John Doe',
    pass            =>  '<password hash goes here>',
    require         =>  Class['accounts::config'],
  }
}
```

The use of the `require` statement creates a dependency not on a single resource but instead to an entire subclass---the `accounts::config` subclass, which in turn has a variety of file resources that are managed according to operating system.

It's such a simple solution I can't believe I didn't see it at first, and when it was pointed out to me (via the #puppet IRC channel, thanks), I had a "Duh!" moment. Even though it is a simple and straightforward solution, if I overlooked it then others might overlook it as well---a problem that hopefully this blog post will help fix.

As always, I welcome feedback from readers, so feel free to weigh in with questions, clarifications, or corrections. Courteous comments are always welcome!


[1]: {{< relref "2012-11-25-using-puppet-for-account-management.md" >}}
