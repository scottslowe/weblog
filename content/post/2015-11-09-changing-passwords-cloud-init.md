---
author: slowe
categories: Tutorial
comments: true
date: 2015-11-09T00:00:00Z
tags:
- Linux
- OpenStack
title: Changing Passwords with cloud-init
url: /2015/11/09/changing-passwords-cloud-init/
---

Generally speaking, when launching instances in a cloud environment (such as AWS or an OpenStack-based cloud), the preferred/default way of accessing that instance is via SSH using an injected SSH key pair. There are times, though, when---for whatever reason---this approach won't work. (I'll describe one such situation below.) In such instances, it's possible to configure [cloud-init][link-1], the same tool used to inject SSH keys, to change passwords for user accounts. Here's how.

Please note that this is a total hack. _(Do NOT use this for any sort of production workload!)_ That being said, sometimes things like this are necessary to complete preliminary evaluations of a new technology, new product, or new architecture. In my case, I had a demo environment (using DevStack) that I needed to get up and running, and the instances would not have _any_ external connectivity. This meant I was limited to console access only---hence, SSH keys are useless. The _only_ means of access would be via password login through the console. So, I found this snippet of cloud-init code:

    #cloud-config
    chpasswd:
      list: |
        user1:password1
        user2:password2
        user3:password3
      expire: False

For this particular use case, I needed to change the default user on the Ubuntu cloud images (which is the "ubuntu" user), so I modified the above code to change only the password for the "ubuntu" user, and placed the code in the "Post-Creation" section of the Launch Instance dialog box in OpenStack. When the instance was launched, cloud-init dutifully changed the password for the specified user, and I was able to log in to the instance's console using username and password.


[link-1]: http://cloudinit.readthedocs.org/en/latest/
