---
author: slowe
categories: Tutorial
comments: true
date: 2017-12-11T23:00:00Z
tags:
- Vagrant
- Azure
- CLI
title: Using Vagrant with Azure
url: /2017/12/11/using-vagrant-with-azure/
---

In this post, I'll describe how to use [Vagrant][link-1] with [Azure][link-5]. You can consider this article an extension of some of my earlier Vagrant articles; namely, the posts on [using Vagrant with AWS][xref-2] and [using Vagrant with OpenStack][xref-1]. The theme across all these posts is examining how one might use Vagrant to simplify/streamline the consumption of resources from a provider using the familiar Vagrant workflow.<!--more-->

If you aren't already familiar with Vagrant, I'd highly recommend first taking a look at my [introduction to Vagrant][xref-3], which provides an overview of the tool and how it's used.

## Prerequisites

Naturally, you'll need to first ensure that you have Vagrant installed. This is really well-documented already, so I won't go over it here. Next, you'll need to install the Azure provider for Vagrant, which you can handle using this command:

    vagrant plugin install vagrant-azure

You'll also (generally) want to have the Azure CLI installed. (You'll need it for a one-time configuration task I'll mention shortly.) I've published a couple posts on installing the Azure CLI; see [here][xref-7] or [here][xref-8].

Once you've installed the `vagrant-azure` plugin and the Azure CLI, you'll next need to install a box that Vagrant can use. Here, the use of Vagrant with Azure is different than the use of Vagrant with a provider like [VirtualBox][link-2] or [VMware Fusion][link-3]/[VMware Workstation][link-4]. Like when using Vagrant with AWS, when you're using Vagrant with Azure the box is a "dummy box" that doesn't really do anything; instead, you're relying on Azure VM images. You can install a "dummy box" for Azure with this command:

    vagrant box add azure-dummy https://github.com/azure/vagrant-azure/raw/v2.0/dummy.box --provider azure

You'll then reference this "dummy box" in your `Vagrantfile`, as I'll illustrate shortly.

The last and final step is to create an Azure Active Directory (AD) service principal for Vagrant to use when connecting to Azure. This command will create a service principal for you to use with Vagrant:

    az ad sp create-for-rbac

Make note of the values returned in the command's JSON response; you'll need them later. You'll also want to know your Azure subscription ID, which you can obtain by running `az account list --query '[?isDefault].id' -o tsv`. _(Note: as pointed out by reader Jochen Schissler, the command listed above creates an entity with Contributor permissions at the Subscription level. For some folks, this isn't possible/acceptable due to security policies. It is perfectly fine to create a service principal and grant permissions at the resource group level, although it must still have Contributor permissions.)_

Now that you've installed Vagrant, installed the `vagrant-azure` plugin, downloaded the dummy Azure box, and have created the Azure AD service principal, you're ready to start spawning some Azure VMs with Vagrant.

## Launching Azure VMs via Vagrant

When I use Vagrant, I prefer to keep the Vagrant configuration (stored in the file named `Vagrantfile`) as clean as possible, and separate details into a separate data file (typically using YAML). I outlined this approach [here][xref-5] and [here][xref-6]. For the purposes of this post, however, I'll just embed the details directly into the Vagrant configuration to make it easier to understand. I have examples of using a YAML data file with Vagrant and Azure in the "Additional Resources" section below.

Here's a snippet of a `Vagrantfile` you could use to spin up Azure VMs using Vagrant:

``` ruby
# Require the Azure provider plugin
require 'vagrant-azure'

# Create and configure the Azure VMs
Vagrant.configure('2') do |config|

  # Use dummy Azure box
  config.vm.box = 'azure-dummy'

  # Specify SSH key
  config.ssh.private_key_path = '~/.ssh/id_rsa'

  # Configure the Azure provider
  config.vm.provider 'azure' do |az, override|
    # Pull Azure AD service principal information from environment variables
    az.tenant_id = ENV['AZURE_TENANT_ID']
    az.client_id = ENV['AZURE_CLIENT_ID']
    az.client_secret = ENV['AZURE_CLIENT_SECRET']
    az.subscription_id = ENV['AZURE_SUBSCRIPTION_ID']

    # Specify VM parameters
    az.vm_name = 'aztest'
    az.vm_size = 'Standard_B1s'
    az.vm_image_urn = 'Canonical:UbuntuServer:16.04-LTS:latest'
    az.resource_group_name = 'vagrant'
  end # config.vm.provider 'azure'
end # Vagrant.configure
```

Naturally, this is a very generic configuration, so you'd need to supply the specific details you want use. In particular, you'd need to supply the following details:

* The SSH keypair you want to use (via the `config.ssh.private_key_path` setting)
* The Azure VM size you'd like to use (via the `az.vm_size` setting)
* The Azure VM image you want to use (via the `az.vm_image_urn` setting)
* The name of the Azure resource group you'd like to use (via the `az.resource_group_name` setting)

Also, you'll note that the `Vagrantfile` assumes you've set some environment variables that will allow Vagrant to communicate with Azure. These values are taken from the JSON output of creating the Azure AD service principal:

* The `AZURE_TENANT_ID` maps to the "tenant" key of the JSON output
* The `AZURE_CLIENT_ID` maps to the "appID" key of the JSON output
* The `AZURE_CLIENT_SECRET` maps to the "password" key of the JSON output
* The `AZURE_SUBSCRIPTION_ID` is your Azure subscription ID, as shown when running `az account show`

Set these environment variables (you can use the `export` command) before running `vagrant up`, or you'll get an error.

With the right configuration details in place, simply run `vagrant up` from the same directory where the Vagrant configuration (in `Vagrantfile`) is stored. Vagrant will communicate with Azure (using the values in the corresponding environment variables) and create/launch the Azure VMs per the details provided.

Once the instances are up, the "standard" Vagrant workflow applies:

* Use `vagrant ssh <name>` to log into one of the VMs.
* Use `vagrant provision` to apply any provisioning instructions (i.e., to copy files across or run a configuration management tool).
* Use `vagrant destroy` to kill/terminate the VMs.

One nice aspect of using Vagrant with Azure in this way is that you get the same workflow and commands to work with Azure VMs as you do with AWS instances, VirtualBox VMs, or VMware Fusion/Workstation VMs.

## Why Use Vagrant with Azure?

As I stated in my post on using Vagrant with AWS, using Vagrant with Azure in this fashion is a great fit for the creation/destruction of temporary environments used for testing, software development, etc. However, in situations where you are creating more "permanent" infrastructure---such as deploying production applications onto Azure---then I would say that Vagrant is _not_ the right fit. In those cases, using a tool like [Terraform][link-8] (see [my introductory post][xref-4]) would be a better fit.

## Additional Resources

To help make using Vagrant with Azure easier, I've created a couple learning environments that are part of [my GitHub "learning-tools" repository][link-6]. Specifically, see the `vagrant/azure` directory for a sample Vagrant configuration that uses an external YAML data file for instance details.

Additionally, see the documentation for [the `vagrant-azure` plugin on GitHub][link-7] for more information.

**UPDATE 5 Jul 2019:** See the added text regarding the creation of a service principal and the permissions needed. Thanks to reader Jochen Schissler for the added information and feedback!

[link-1]: https://www.vagrantup.com
[link-2]: https://www.virtualbox.org
[link-3]: http://www.vmware.com/products/fusion.html
[link-4]: http://www.vmware.com/products/workstation.html
[link-5]: https://azure.microsoft.com/
[link-6]: https://github.com/scottslowe/learning-tools
[link-7]: https://github.com/Azure/vagrant-azure
[link-8]: https://www.terraform.io/
[xref-1]: {{< relref "2015-09-28-using-vagrant-with-openstack.md" >}}
[xref-2]: {{< relref "2016-09-15-using-vagrant-with-aws.md" >}}
[xref-3]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}
[xref-4]: {{< relref "2015-11-25-intro-to-terraform.md" >}}
[xref-5]: {{< relref "2014-10-22-multi-machine-vagrant-with-yaml.md" >}}
[xref-6]: {{< relref "2016-01-14-improved-way-yaml-vagrant.md" >}}
[xref-7]: {{< relref "2017-08-13-manually-installing-azure-cli-fedora-25.md" >}}
[xref-8]: {{< relref "2017-12-07-installing-azure-cli-fedora-27.md" >}}
