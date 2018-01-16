---
author: slowe
categories: Tutorial
comments: true
date: 2018-01-16T08:00:00Z
tags:
- Azure
- Vagrant
- CLI
- Docker
title: Experimenting with Azure
url: /2018/01/16/experimenting-with-azure/
---

I've been experimenting with [Microsoft Azure][link-4] recently, and I thought it might be useful to share a quick post on using some of my favorite tools with Azure. I've found it useful to try to leverage existing tools whenever I can, and so as I've been experimenting with Azure I've been leveraging familiar tools like [Docker Machine][link-2] and [Vagrant][link-3].<!--more-->

The information here isn't revolutionary or unique, but hopefully it will still be useful to others, even if only as a "quick reference"-type of post.

## Launching an Instance on Azure Using Docker Machine

To launch an instance on Azure and provision it with Docker using `docker-machine`:

    docker-machine create -d azure \
    --azure-subscription-id $(az account show --query "id" -o tsv) \
    --azure-ssh-user azureuser \
    --azure-size "Standard_B1ms" azure-test

The first time you run this you'll probably need to allow Docker Machine access to your Azure subscription (you'll get prompted to log in via a browser and allow access). This will create a service principal that is visible via `az ad sp list`. Note that you may be prompted for authentication for future uses, although it will re-use the existing service principal once it is created.

## Launching an Instance Using the Azure Provider for Vagrant

See [this page][link-1] for complete details on using the Azure provider for Vagrant. Basically, it boils down to these four steps:

1. Install the Azure provider using `vagrant plugin install vagrant-azure`.
2. Add a "dummy" box (similar to how you use Vagrant with AWS; see this post).
3. Set up an Azure service principal for Vagrant to use to connect to Azure.
4. Run `vagrant up` and you're off to the races.

A more detailed post on [using Vagrant with Azure is available here][xref-1]; it provides a bit more information on the above steps.

## Launching an Instance using the Azure CLI

OK, maybe the Azure CLI isn't exactly an "existing tool," but given my affinity for CLI-based tools I think it's probably reasonable to include it here. To launch an instance using the Azure CLI, it would look something like this:

    az vm create -n vm-name -g group-name --image UbuntuLTS --size Standard_B1ms --no-wait

Of course, this assumes a pre-existing resource group. More details are available [here][link-5].

If you need to install the Azure CLI, see [here][xref-2] or [here][xref-3] for some additional information.

Happy experimenting!



[link-1]: https://github.com/Azure/vagrant-azure
[link-2]: https://www.docker.com/products/docker-machine
[link-3]: https://www.vagrantup.com/
[link-4]: https://azure.microsoft.com/en-us/
[link-5]: https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-cli
[xref-1]: {{< relref "2017-12-11-using-vagrant-with-azure.md" >}}
[xref-2]: {{< relref "2017-08-13-manually-installing-azure-cli-fedora-25.md" >}}
[xref-3]: {{< relref "2017-12-07-installing-azure-cli-fedora-27.md" >}}
