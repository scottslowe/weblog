---
author: slowe
categories: Tutorial
comments: true
date: 2018-01-25T23:00:00Z
tags:
- Azure
- CLI
- Docker
- Ubuntu
title: Using Docker Machine with Azure
url: /2018/01/25/using-docker-machine-with-azure/
---

I've written about using [Docker Machine][link-1] with a number of different providers, such as [with AWS][xref-1], [with OpenStack][xref-2], and even [with a local KVM/Libvirt daemon][xref-3]. In this post, I'll expand that series to show using Docker Machine with Azure. (This is a follow-up to my earlier post on [experimenting with Azure][xref-4].)<!--more-->

As with most of the other Docker Machine providers, using Docker Machine with Azure is reasonably straightforward. Run `docker-machine create -d azure --help` to get an idea of some of the parameters you can use when creating VMs on Azure using Docker Machine. A full list of the various parameters and options for the Azure drive is [also available][link-2].

The only _required_ parameter is `--azure-subscription-id`, which specifies your Azure subscription ID. If you don't know this, or want to obtain it programmatically, you can use this Azure CLI command:

    az account show --query "id" -o tsv

If you have more than one subscription, you'll probably need to modify this command to filter it down to the specific subscription you want to use.

Additional parameters that you can supply include (but aren't limited to):

* Use the `--azure-image` parameter to specify the VM image you'd like to use. By default, the Azure driver uses Ubuntu 16.04.
* By default, the Azure driver launches a Standard_A2 VM. If you'd like to use a different size, just supply the `--azure-size` parameter.
* The `--azure-location` parameter lets you specify an Azure region other than the default, which is "westus".
* You can specify a non-default resource group (the default value is "docker-machine") by using the `--azure-resource-group` parameter.
* The Azure driver defaults to a username of "docker-user"; use the `--azure-ssh-user` to specify a different name.
* You can customize networking configurations using the `--azure-subnet-prefix`, `--azure-subnet`, and `--azure-vnet` options. Default values for these options are 192.168.0.0/16, "docker-machine", and "docker-machine", respectively.

So what would a complete command look like? Using Bash command substitution to supply the Azure subscription ID, a sample command might look like this:

    docker-machine create -d azure \
    --azure-subscription-id $(az account show --query "id" -o tsv) \
    --azure-location westus2 \
    --azure-ssh-user ubuntu \
    --azure-size "Standard_B1ms" \
    dm-azure-test

This would create an Azure VM named "dm-azure-test", based on the (default) Ubuntu 16.04 LTS image, in the "westus2" Azure region and using a username of "ubuntu". Once the VM is running and responding across the network, Docker Machine will provision and configure Docker Engine on the VM.

Once the VM is up, all the same `docker-machine` commands are available:

* `docker-machine ls` will list all configured machines (systems managed via Docker Machine); this is across all supported Docker Machine providers
* `docker-machine ssh <name>` to establish an SSH connection to the VM
* `eval $(docker-machine env <name>)` to establish a Docker configuration pointing to the remote VM (this would allow you to use a local Docker client to communicate with the remote Docker Engine instance)
* `docker-machine stop <name>` stops the VM (which can be restarted using `docker-machine start <name>`, naturally)
* `docker-machine rm <name>` deletes the VM

Clearly, there's more available, but this should be enough to get most folks rolling.

If I've missed something (or gotten it incorrect), please [hit me up on Twitter][link-3]. I'll happily make corrections where applicable.



[link-1]: https://www.docker.com/products/docker-machine
[link-2]: https://docs.docker.com/machine/drivers/azure/
[link-3]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2016-03-22-using-docker-machine-with-aws.md" >}}
[xref-2]: {{< relref "2015-09-24-using-docker-machine-with-openstack.md" >}}
[xref-3]: {{< relref "2017-11-24-using-docker-machine-kvm-libvirt.md" >}}
[xref-4]: {{< relref "2018-01-16-experimenting-with-azure.md" >}}
