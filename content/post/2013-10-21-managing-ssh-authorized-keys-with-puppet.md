---
author: slowe
categories: Explanation
comments: true
date: 2013-10-21T12:53:25Z
slug: managing-ssh-authorized-keys-with-puppet
tags:
- Automation
- Linux
- Puppet
- SSH
title: Managing SSH Authorized Keys with Puppet
url: /2013/10/21/managing-ssh-authorized-keys-with-puppet/
wordpress_id: 3313
---

In this post, I'll show you how I extended my solution for [managing user accounts with Puppet][1] to include managing SSH authorized keys. With this solution in place, user accounts managed through Puppet can also include their SSH public key, and that public key will automatically be installed on hosts where the account is realized. All in all, I think it's a pretty cool solution.

Just to refresh your memory, here's the original Puppet manifest code I posted in the original article; this code uses define-based virtual user resources that you then realize on a per-host basis (click [here](https://gist.github.com/scottslowe/4050213) for an option to download this code snippet).

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

Since I posted this original code, I've made a few changes. I switched some of the hard-coded values to parameters (stored in a separate subclass), and I made a few stylistic/syntactic changes based on running the code through `puppet-lint`. But, by and large, this is still quite similar to the code I'm running right now.

Here's the code after I modified it to include managing SSH authorized keys for user accounts (again, click [here](https://gist.github.com/scottslowe/7064759) for an option to download the code):

``` puppet
define accounts::virtual ($uid,$realname,$pass,$sshkeytype,$sshkey) {
  include accounts::params

  # Pull in values from accounts::params
  $homepath =  $accounts::params::homepath
  $shell    =  $accounts::params::shell

  # Create the user
  user { $title:
    ensure            =>  'present',
    uid               =>  $uid,
    gid               =>  $title,
    shell             =>  $shell,
    home              =>  "${homepath}/${title}",
    comment           =>  $realname,
    password          =>  $pass,
    managehome        =>  true,
    require           =>  Group[$title],
  }

  # Create a matching group
  group { $title:
    gid               => $uid,
  }

  # Ensure the home directory exists with the right permissions
  file { "${homepath}/${title}":
    ensure            =>  directory,
    owner             =>  $title,
    group             =>  $title,
    mode              =>  '0750',
    require           =>  [ User[$title], Group[$title] ],
  }

  # Ensure the .ssh directory exists with the right permissions
  file { "${homepath}/${title}/.ssh":
    ensure            =>  directory,
    owner             =>  $title,
    group             =>  $title,
    mode              =>  '0700',
    require           =>  File["${homepath}/${title}"],
  }

  # Add user's SSH key
  if ($sshkey != '') {
    ssh_authorized_key {$title:
      ensure          => present,
      name            => $title,
      user            => $title,
      type            => $sshkeytype,
      key             => $sshkey,
    }
  }
}
```

Let's walk through the changes between the two snippets of code:

* Two new parameters are added, `$sshkeytype` and `$sshkey`. These parameters hold, quite naturally, the SSH key type and the SSH key itself.

* Several values are parameterized, pulling values from the `accounts::params` manifest.

* You can note a number of stylistic and syntactical changes.

* The `accounts::virtual` class now includes a stanza using the built-in `ssh_authorized_key` resource type. This is the real heart of the changes---by adding this to the virtual user resource, it makes sure that when users are realized, their SSH public keys are added to the host.

With this code in place, you'd then define a user like this:

``` puppet
@accounts::virtual { 'jsmith':
  uid             =>  5001,
  realname        =>  'John Smith',
  pass            =>  '<insert password hash here>',
  sshkeytype      =>  'ssh-dss',
  sshkey          =>  '<insert SSH key here>',
  require         =>  Class['accounts::config'],
}
```

The requirement for `Class['accounts::config']` is to ensure that various configuration tasks are finished before the user account is defined; I discussed this in more detail in this post on [Puppet, user accounts, and configuration files][2]. Now, when I realize a virtual user resource, Puppet will also ensure that the user's SSH public key is automatically added to the user's `.ssh/authorized_keys` file on that host. Pretty sweet, eh? Further, if the key ever changes, you need only change it on the Puppet server itself, and on the next Puppet agent run the hosts will update themselves.

I freely admit that I'm not a Puppet expert, so there might be better/faster/more efficient ways of doing this. If you _are_ a Puppet expert, please feel free to weigh in below in the comments. I welcome all courteous comments!


[1]: {{< relref "2012-11-25-using-puppet-for-account-management.md" >}}
[2]: {{< relref "2013-01-29-puppet-user-accounts-and-configuration-files.md" >}}
