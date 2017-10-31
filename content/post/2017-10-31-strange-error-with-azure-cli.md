---
author: slowe
categories: Information
comments: true
date: 2017-10-31T13:00:00Z
tags:
- Microsoft
- Azure
- CLI
title: Strange Error with the Azure CLI
url: /2017/10/31/strange-error-with-azure-cli/
---

Over the last week or so, I've been trying to spend more time with [Microsoft Azure][link-3]; specifically, around some of the interesting things that Azure is doing with containers and [Kubernetes][link-4]. Inspired by articles such as [this one][link-1], I thought it would be a pretty straightforward process to use the Azure CLI to spin up a Kubernetes cluster and mess around a bit. Simple, right?<!--more-->

Alas, it turned out not to be so simple (if it had been simple, this blog post wouldn't exist). The first problem I ran into was the upgrading the Azure CLI from version 2.0.13 to version 2.0.20 (which is, to my understanding, the minimum version needed to do what I was trying to do). I'd installed the Azure CLI using [this process][xref-1], so `pip install azure-cli --upgrade` should take care of it. Unfortunately, on two out of three systems on which I attempted this, the Azure CLI failed to work after the upgrade. I was only able to fix the problem by completely removing the Azure CLI (which I'd installed into a virtualenv), and then re-installing it. First hurdle cleared!

With the Azure CLI upgraded, I proceeded to ensure that I was appropriately logged in (via `az login`), created a resource group in "west us 2" (via `az group create`), and then tried to launch an ACS cluster:

    az acs create --orchestrator-type=kubernetes --resource-group=my-rg \
    --name=my-k8s-cluster --generate-ssh-keys

The Azure CLI created a service principal for me (as expected), but then errored out with a permissions-related error referencing a _different_ service principal.

Using `az ad sp list`, I looked at the service principal in question; it was not the service principal created automatically by the `az acs create` command, but a built-in service principal named "AzureContainerService". Thinking I'd done something wrong, I deleted everything I'd done so far---removed the resource group, deleted the automatically-created service principal---and tried again.

No joy; same error. Dave Strebel ([@dave_strebel][link-2] on Twitter) offered some assistance but couldn't reproduce the error. OK, let's try deleting the "AzureContainerService" principal. Nope, that just made things worse. Thinking that perhaps something had gone wrong with my subscription, I deactivated that subscription and created a new one.

That didn't work either; same error (I had to correct a few subscription-related issues via `az account` first). At this point, Dave offered to take a deeper look for me (thanks Dave!), so I sent him some information. While I was waiting to hear back from Dave, I tried installing the Azure CLI on Windows 10, just to see if there was some sort of platform issue at play here. I ran into the same error there.

After a while, Dave contacted me and suggested I run a few commands. After some trial and error, the correct set of commands that ultimately enabled `az acs create` to work as expected were these:

    az provider register -n Microsoft.Compute
    az provider register -n Microsoft.Network
    az provider register -n Microsoft.Storage

After running these commands and giving some time for the registrations to fully complete (the `az provider show` command doesn't really help much, to be honest, despite the suggestion from the Azure CLI otherwise), then creating a Kubernetes-powered ACS cluster using `az acs create` worked as expected. Hurray!

So, what are the lessons I learned from this experience?

* If you're trying to use `az acs create` and getting a strange permissions error related to the "AzureContainerService" service principal, try the `az provider register` commands listed above. (I still don't know what these commands actually do, or why they were required.)
* If you installed the Azure CLI using `pip`, upgrading it via `pip` may not work as expected. I recommend using a virtualenv to make removing the Azure CLI and re-installing it a simple process. (This doesn't apply to Windows, naturally, where the typical installation method is an MSI package.)
* Dave Strebel is a great example of community advocacy.

On to more adventures!



[link-1]: http://hypernephelist.com/2017/10/17/getting-started-with-traefik-and-k8s-using-acs.html
[link-2]: https://twitter.com/dave_strebel
[link-3]: https://azure.microsoft.com/en-us/
[link-4]: https://kubernetes.io
[xref-1]: {{< relref "2017-08-13-manually-installing-azure-cli-fedora-25.md" >}}
