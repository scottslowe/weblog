---
author: slowe
categories: Information
comments: true
date: 2017-02-24T00:00:00Z
tags:
- Vagrant
- JSON
- Networking
- Virtualization
title: Adding Metadata to the Arista vEOS Vagrant Box
url: /2017/02/24/adding-metadata-arista-veos-vagrant-box/
---

This post addresses a (mostly) cosmetic issue with the current way that Arista distributes its [Vagrant][link-2] box for vEOS. I say "mostly cosmetic" because while the Vagrant box for vEOS is perfectly functional if you use it [via Arista's instructions][link-1], adding metadata as I explain here provides a small bit of additional flexibility should you need multiple versions of the vEOS box on your system.

If you follow Arista's instructions, then you'll end up with something like this when you run `vagrant box list`:

```
arista-veos-4.18.0    (virtualbox, 0)
bento/ubuntu-16.04    (virtualbox, 2.3.1)
centos/6              (virtualbox, 1611.01)
centos/7              (virtualbox, 1611.01)
centos/atomic-host    (virtualbox, 7.20170131)
coreos-stable         (virtualbox, 1235.9.0)
debian/jessie64       (virtualbox, 8.7.0)
```

Note that the version of the vEOS box is embedded in the name. Now, you _could_ not put the version in the name, but because there's no metadata---which is why it shows `(virtualbox, 0)` on that line---you wouldn't have any way of knowing which version you had. Further, what happens when you want to have multiple versions of the vEOS box?

Fortunately, there's an easy fix (inspired by the way CoreOS distributes their Vagrant box). Just create a file with the following contents:

``` json
{
  "name": "arista-veos",
  "description": "Arista vEOS",
  "versions": [
    {
      "version": "4.18.0",
      "providers": [
        {
          "name": "virtualbox",
          "url": "file:///home/slowe/Downloads/vEOS-lab-4.18.0-virtualbox.box"
        }
      ]
    }
  ]
}
```

Obviously, you'll want to make sure the "version" value is correct for whatever version of the vEOS box you're using, and you'll want to update the "url" value to the correct location of the file you downloaded according to [Arista's instructions][link-1]. Save this file (using a name like "arista-veos.json" or similar), then follow these steps:

1. If you've already added the box using `vagrant box add`, remove it using `vagrant box remove`.

2. Re-add the box using `vagrant box add arista-veos.json` (substituting the name you gave the JSON file you just created, naturally).

Vagrant will re-add the box, but this time using metadata from the JSON file you created. When you run `vagrant box list`, you'll see something like this:

```
arista-veos           (virtualbox, 4.18.0)
bento/ubuntu-16.04    (virtualbox, 2.3.1)
centos/6              (virtualbox, 1611.01)
centos/7              (virtualbox, 1611.01)
centos/atomic-host    (virtualbox, 7.20170131)
coreos-stable         (virtualbox, 1235.9.0)
debian/jessie64       (virtualbox, 8.7.0)
```

Ah, much better! Not only is it cosmetically better, but you can now have multiple versions of the vEOS Vagrant box on your system, all with the same name and differentiated only by version. This, in turn, simplifies your ability to test the same Vagrant environment with different vEOS versions.

Mostly, though, I like it because it _looks_ better.



[link-1]: https://eos.arista.com/using-veos-with-vagrant-and-virtualbox/
[link-2]: https://www.vagrantup.com/
