---
author: slowe
categories: Tutorial
comments: true
date: 2015-09-28T09:45:00Z
aliases: /2015/09/25/using-vagrant-with-openstack/
tags:
- Vagrant
- OpenStack
- CLI
title: Using Vagrant with OpenStack
url: /2015/09/28/using-vagrant-with-openstack/
---

In [my earlier post][xref-1] on using Docker Machine with OpenStack, I talked about combining technologies in a "provider/consumer" model. In this post, I'm going to talk about creating this provider/consumer model using a different combination of technologies: [OpenStack][link-1] as the infrastructure provider and [Vagrant][link-2] for consuming that infrastructure.

If you're unfamiliar with Vagrant, I recommend you first read [this introduction to Vagrant][xref-2] (after that you can dig into [all the other Vagrant-tagged posts][link-4]). As I explain in that first post, Vagrant leverages the idea of _providers_ (which enable Vagrant to work with various back-end virtualization platforms/solutions) as well as _boxes_ (which are essentially VM templates). In this particular case, we're leveraging an OpenStack provider for Vagrant that allows Vagrant to use OpenStack as the back-end virtualization solution. However, since OpenStack already has the equivalent of VM templates (in the form of images), there's no need to use a Vagrant box. This makes using Vagrant with OpenStack slightly different than your typical Vagrant use case.

## Prerequisites

Let's start with reviewing some prerequisites---these are the things you'll need to do/have done before you can use Vagrant with OpenStack (besides the obvious things like having Vagrant installed).

1. You'll need a working OpenStack installation with at least one Glance image uploaded and ready for use.
2. You'll need a provider network (and associated allocation pool) defined, so that OpenStack is able to allocate floating IP addresses to instances.
3. You'll need to know working OpenStack credentials, like your username, password, tenant (project) name, and authentication URL.

Once you have these prerequisites out of the way, you're ready to start using Vagrant to drive OpenStack.

## Driving OpenStack with Vagrant

Here are the details on using Vagrant with OpenStack.

First, install the OpenStack provider plugin (obviously, this only needs to be done once):

    vagrant plugin install vagrant-openstack-provider

Next, create a `Vagrantfile` with the right settings. The full list of settings for the OpenStack provider is available from [the provider's GitHub page][link-3], but here are a few of the highlights (all these fall under the `config.vm.provider` scope):

* The `os.openstack_auth_url`, `os.username`, `os.password`, and `os.tenant_name` options provide the necessary authentication details to allow Vagrant to authenticate against OpenStack.
* Instance details are supplied through the `os.server_name`, `os.flavor`, and the `os.image` settings.
* Networking information comes from the `os.floating_ip_pool`, `os.networks`, and `os.security_groups` parameters.
* Finally, logging in to the instance is enabled through two different settings. One of them is `os.keypair_name` (under `config.vm.provider`), and this points to the name of a keypair in OpenStack. The second is under `Vagrant.configure` and is the `config.ssh.private_key_path` setting.

Here's a sample snippet from a `Vagrantfile` for use with the OpenStack provider:

```ruby
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Specify the default SSH username and private key
  config.ssh.username = "ubuntu"
  config.ssh.private_key_path = "~/.ssh/private_key.pem"

  # Configure the OpenStack provider for Vagrant
  config.vm.provider "openstack" do |os|

    # Specify OpenStack authentication information
    os.openstack_auth_url = "http://controller.domain.com:5000/v2.0"
    os.username = "demo"
    os.password = "demo"
    os.tenant_name = "demo"

    # Specify instance information
    os.server_name = "vagrant-test"
    os.flavor = "m1.small"
    os.image = "Ubuntu 14.04.3 LTS x64"
    os.floating_ip_pool = "public"
    os.networks = "vagrant-net"
    os.keypair_name = "private_key"
    os.security_groups = ["default","basic-services"]
  end
end
```

With the OpenStack provider plugin installed and a properly-configured `Vagrantfile` in place, then it's a simple matter of running `vagrant up`. Vagrant will authenticate against OpenStack, spin up an instance using the specified image and flavor, allocate a floating IP address, apply the listed security groups, and inject the specified SSH key. How's that for handy?

Equally handy, when you're done with the instance(s) created by Vagrant, a simple `vagrant destroy` will clean it all up. No need to log into the OpenStack dashboard---just a simple, quick CLI command and you're done.

## Why Vagrant with OpenStack?

As I did in [the post on Docker Machine with OpenStack][xref-1], I want to take a brief moment to talk about _why_ I think this particular combination could be useful. Aside from creating a clean "provider/consumer" relationship between infrastructure being orchestrated by OpenStack and resources being consumed by users via Vagrant, this enables users to apply the same model of consuming resources locally (via Vagrant's CLI commands) to consuming resources from a cloud implementation (using the same Vagrant CLI commands). In my opinion, anything that makes it easier and simpler for users to consume OpenStack resources is a good thing.

## More Details

I tested this combination using the following software versions:

* The client system was running OS X 10.9.5 with Vagrant 1.7.4.
* I was using version 0.7.0 of the Vagrant OpenStack provider.
* The OpenStack cloud was running the Juno release on Ubuntu 14.04 LTS, KVM hypervisors, and VMware NSX for networking.

As always, feel free to hit me up on Twitter (or drop me an e-mail) if you have any questions or comments.


[link-1]: http://www.openstack.org/
[link-2]: http://www.vagrantup.com/
[link-3]: https://github.com/ggiamarchi/vagrant-openstack-provider/
[link-4]: /tags/vagrant/
[xref-1]: {{< relref "2015-09-24-using-docker-machine-with-openstack.md" >}}
[xref-2]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}
