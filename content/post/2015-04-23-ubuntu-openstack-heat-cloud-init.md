---
author: slowe
categories: Explanation
comments: true
date: 2015-04-23T09:25:00Z
tags:
- Linux
- Ubuntu
- OpenStack
- Cloud
title: Ubuntu, cloud-init, and OpenStack Heat
url: /2015/04/23/ubuntu-openstack-heat-cloud-init/
---

In this post I'd like to share a couple of things I recently learned about the interaction between [cloud-init][link-1] and [OpenStack Orchestration (aka "Heat")][link-2]. This may be stuff that you already know, but in the interest of helping others who may not know I'm posting it here.

One issue that I'd been repeatedly running into was an apparent "failure" on the part of Heat to properly apply cloud-init configurations to deployed Ubuntu instances. So, using a Heat template with an `OS::Nova::Server` resource defined like this would result in an instance that apparently wasn't reachable via SSH (I'd get back `Permission denied (publickey)`):

```yaml
resources:
  instance0:
    type: OS::Nova::Server
    properties:
      name: cloud-init-test-01
      image: { get_param: image_id }
      flavor: m1.xsmall
      networks:
        - port: { get_resource: instance0_port0 }
      key_name: lab
```

Deploying an instance manually from the same image worked perfectly. So what was the deal?

The first thing I learned was that, in some circumstances (more on this in a moment), Heat defaults to injecting SSH keys (like the key named `lab` specified in the template) to a user account named "ec2-user". Ah! I'd been using the default "ubuntu" account specified in the Ubuntu cloud images. As soon as I changed my SSH command to reference "ec2-user" instead of "ubuntu", then I was able to gain access to the instance.

However, the problem wasn't completely resolved. Although the hostname had been properly customized, the environment wasn't operating as expected. The shell prompt was a simple `$`, not `username@hostname` that you'd typically see. Auto-completion in the bash shell didn't work. Command history didn't work. Clearly something was still off. (Again, these issues did _not_ appear in manually deployed instances based on the exact same image.)

I thought this might be related to using "ec2-user" instead of "ubuntu", so I started digging around and found that you can specify the user account Heat should use with the `admin_user` parameter. That changed the Heat template snippet to look like this:

```yaml
resources:
  instance0:
    type: OS::Nova::Server
    properties:
      name: cloud-init-test-01
      image: { get_param: image_id }
      flavor: m1.xsmall
      networks:
        - port: { get_resource: instance0_port0 }
      key_name: lab
      admin_user: ubuntu
```

This ensured that the specified SSH key was injected for the expected user ("ubuntu", in this case), but it didn't change the odd behaviors once I got logged in. Something was still off.

Digging into the logs, I found this message being reported in the instance's console log:

```text
cloud-init-test-01 login: Cloud-init v. 0.7.5 running 'modules:final' at Thu, 23 Apr 2015 15:27:39 +0000. Up 24.12 seconds.
Provision began: 2015-04-23 15:27:39.250084

/var/lib/heat-cfntools/cfn-userdata

Userdata empty or not executable: [Errno 8] Exec format error

Provision done: 2015-04-23 15:27:39.255737
```

OK, so cloud-init was running into some sort of error when deploying the instance, which explained why the instance's environment wasn't getting configured correctly. Great---now how do I fix the problem?

I spent a couple of weeks (off and on when not traveling or working on other projects) trying to figure out what was going on here. I dug into cloud-init, carefully reviewed the cloud-init output logs (written by default to `/var/log/cloud-init-output.log`), tested the OpenStack metadata service, etc. I just couldn't seem to make any real progress.

Then I came across this snippet in the OpenStack user guide online:

>The OS::Nova::Server user_data_format property determines how the user_data should be formatted for the server. For the default value HEAT_CFNTOOLS, the user_data is bundled as part of the heat-cfntools cloud-init boot configuration data. While HEAT_CFNTOOLS is the default for user_data_format, it is considered legacy and RAW or SOFTWARE_CONFIG will generally be more appropriate.

Light bulb! Heat was defaulting to HEAT_CFNTOOLS, which is exactly what the error message in the console log was reporting. So, I changed the Heat template so that the instance was invoked like this:

```yaml
resources:
  instance0:
    type: OS::Nova::Server
    properties:
      name: cloud-init-test-01
      image: { get_param: image_id }
      flavor: m1.xsmall
      networks:
        - port: { get_resource: instance0_port0 }
      key_name: lab
      admin_user: ubuntu
      user_data_format: RAW
```

That fixed the problem---the error in the console logs went away, I was able to log in as the "ubuntu" user with the specified SSH key, and the instance was configured properly and behaved as one would expect. End of the story, right?

Not quite. Additional testing revealed that only the `user_data_format: RAW` statement was actually required to fix the problem I'd been experiencing. In other words, fixing the HEAT_CFNTOOLS error also fixed the issue of injecting the SSH key to the correct user. Using a snippet like this worked just fine:

```yaml
resources:
  instance0:
    type: OS::Nova::Server
    properties:
      name: cloud-init-test-01
      image: { get_param: image_id }
      flavor: m1.xsmall
      networks:
        - port: { get_resource: instance0_port0 }
      key_name: lab
      user_data_format: RAW
```

This produced an Ubuntu instance that was accessible via SSH, injecting the specified key for the "ubuntu" user and properly customizing and configuring the instance. (And, naturally, the earlier error that had appeared in the console logs was no longer present.)

## Key Takeaway

When using OpenStack Heat, you're almost certainly going to want to be sure to specify `user_data_format: RAW` when defining your instances. This definitely is the case for Ubuntu instances based on the Ubuntu cloud images; I haven't tested any of the other distributions to see if they behave similarly. The giveaway that this is the issue will be the HEAT_CFNTOOLS-related error in the console log for the instance(s) in question.

As I said at the beginning of this post, this might be something you already know. It might be so simple that you're in disbelief that I didn't already know this. I suspect, though, that if this is something I ran into others will run into it as well. If you end up being one of those folks, I hope this ends up being useful for you.

[link-1]: http://cloudinit.readthedocs.org/en/latest/
[link-2]: https://wiki.openstack.org/wiki/Heat
[link-3]: http://docs.openstack.org/user-guide/content/user-data-boot-scripts-and-cloud-init.html
