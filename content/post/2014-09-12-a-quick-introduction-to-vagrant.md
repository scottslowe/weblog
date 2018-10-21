---
author: slowe
categories: Education
comments: true
date: 2014-09-12T11:24:08Z
slug: a-quick-introduction-to-vagrant
tags:
- CLI
- Linux
- Virtualization
- Vagrant
title: A Quick Introduction to Vagrant
url: /2014/09/12/a-quick-introduction-to-vagrant/
wordpress_id: 3534
---

This post will provide a quick introduction to a tool called [Vagrant](http://www.vagrantup.com). Unless you've been hiding under a rock---or, more likely, been too busy doing real work in your data center to pay attention---you've probably heard of Vagrant. Maybe, like me, you had some ideas about what Vagrant is (or isn't) and what it does (or doesn't) do. Hopefully I can clear up some of the confusion in this post.

In its simplest form, Vagrant is an _automation tool with a domain-specific language (DSL) that is used to automate the creation of VMs and VM environments._ The idea is that a user can create a set of instructions, using Vagrant's DSL, that will set up one or more VMs and possibly configure those VMs. Every time the user uses the precreated set of instructions, the end result will look exactly the same. This can be beneficial for a number of use cases, including developers who want a consistent development environment or folks wanting to share a demo environment with other users.

Vagrant makes this work by using a number of different components:

* Providers: These are the "back end" of Vagrant. Vagrant itself doesn't provide any virtualization functionality; it relies on other products to do the heavy lifting. _Providers_ are how Vagrant interacts with the products that will do the actual virtualization work. A provider could be VirtualBox (included by default with Vagrant), VMware Fusion, Hyper-V, vCloud Air, or AWS, just to name a few.

* Boxes: At the heart of Vagrant are _boxes_. Boxes are the predefined images that are used by Vagrant to build the environment according to the instructions provided by the user. A box may be a plain OS installation, or it may be an OS installation plus one or more applications installed. Boxes may support only a single provider or may support multiple providers (for example, a box might only work with VirtualBox, or it might support VirtualBox and VMware Fusion). It's important to note that multi-provider support by a box is really handled by multiple versions of a box (i.e, a version supporting VirtualBox, a version supporting AWS, or a version supporting VMware Fusion). A single box supports a single provider.

* Vagrantfile: The _Vagrantfile_ contains the instructions from the user, expressed in Vagrant's DSL, on what the environment should look like---how many VMs, what type of VM, the provider, how they are connected, etc. Vagrantfiles are so named because the actual filename is `Vagrantfile`. The Vagrant DSL (and therefore Vagrantfiles) are based on Ruby.

Once of the first things I thought about as I started digging into Vagrant was that Vagrant would be a tool that would help streamline moving applications/code from development to production. After all, if you had providers for Vagrant that supported both VirtualBox and VMware vCenter, _and_ you had boxes that supported both providers, then you could write a single `Vagrantfile` that would instantiate the same environment in development and in production. Cool, right? In theory this is possible, but in talking with some others who are much more familiar with Vagrant than I am it seems that in practice this is not necessarily the case. Because support for multiple providers is handled by different versions of a box (as outlined above), the boxes _may_ be slightly different and therefore may not produce the exact same results from a single `Vagrantfile`. It is possible to write the `Vagrantfile` in such a way as to recognize different providers and react differently, but this obviously adds complexity.

With that in mind, it seems to me that the most beneficial uses of Vagrant are therefore to speed up the creation of development environments, to enable version control of development environments (via version control of the `Vagrantfile`), to provide some reasonable level of consistency across multiple developers, and to make it easier to share development environments. (If my conclusions are incorrect, please speak up in the comments and explain why.)

OK, enough of the high-level theory. Let's take a look at a _very_ simple example of a `Vagrantfile`:

```ruby
VAGRANTFILE_API_VERSION = "2"
 
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
 
  # Box
  config.vm.box = "ubuntu/precise64"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"
 
  # Shared folders
  config.vm.synced_folder ".", "/vagrant"
 
end
```

(Click [here](https://gist.github.com/scottslowe/0f82601987bf1f39c941) for a downloadable GitHub Gist with this code.)

This `Vagrantfile` sets the box ("ubuntu/precise64"), the box URL (retrieves from Canonical's repository of cloud images), and then sets the "/vagrant" directory in the VM to be shared/synced with the current (".") directory on the host---in this case, the current directory is the directory where the `Vagrantfile` itself is stored.

To have Vagrant then use this set of instructions, run this command from the directory where the `Vagrantfile` is sitting:

    vagrant up

You'll see a series of things happen; along the way you'll see a note that the machine is booted and ready, and that shared folders are getting mounted. (If you are using VirtualBox and the box I'm using, you'll also see a warning about the VirtualBox Guest Additions version not matching the version of VirtualBox.) When it's all finished, you'll be deposited back at your prompt. From there, you can easily log in to the newly-created VM using nothing more than `vagrant ssh`. That's pretty handy.

Other Vagrant commands include:

* `vagrant halt` to shut down the VM(s)

* `vagrant suspend` to suspend the VM(s), use `vagrant resume` to resume

* `vagrant status` to display the status of the VM(s)

* `vagrant destroy` to destroy (delete) the VM(s)

Clearly, the example I showed you here is extremely simple. For an example of a more complicated `Vagrantfile`, check out [this example](https://github.com/bunchc/Couch_to_OpenStack/blob/master/Vagrantfile) from Cody Bunch, which sets up a set of VMs for running OpenStack. Cody and his co-author Kevin Jackson also use Vagrant extensively in their _OpenStack Cloud Computing Cookbook, 2nd Edition_, which makes it easy for readers to follow along.

I said this would be a quick introduction to Vagrant, so I'll stop here for now. Feel free to post any questions in the comments, and I'll do my best to answer them. Likewise, if there are any errors in the post, please let me know in the comments. All courteous comments are welcome!
