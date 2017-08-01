---
author: slowe
categories: Explanation
comments: true
date: 2015-11-23T00:00:00Z
tags:
- Ansible
- Linux
- OpenStack
- CLI
- Ubuntu
title: Bootstrapping Cloud Instances into Ansible
url: /2015/11/23/bootstrapping-cloud-instances-ansible/
---

A while ago, I wrote an article about [bootstrapping servers into Ansible][xref-1]---in other words, how to prepare servers to be managed via [Ansible][link-1]. In order for a server to be managed via Ansible, you usually must first create a user account for Ansible, populate the appropriate SSH keys, and grant the new Ansible user sudo permissions. The process I described in my earlier blog post works great for manually-built servers (physical or virtual), but I recently needed to revisit this process for cloud instances. Was it possible to use the process I'd found to bootstrap cloud instances into Ansible?

Cloud instances are a slightly different beast than manually-built servers primarily because password authentication isn't an option---generally speaking, you're required to use SSH keys when working with cloud instances. Ansible is SSH-based, as you probably already know, so this _shouldn't_ be an issue, but it was still something I hadn't tested or verified. After a bit of testing, I found the bootstrap process I described in my earlier post can be easily adapted for cloud instances.

For reference, here's the command I use when bootstrapping manually-built servers into Ansible:

    ansible-playbook bootstrap.yml -k -K --extra-vars \
    "hosts=newhost.domain.com user=admin"

The `bootstrap.yml` playbook simply creates an Ansible user, populates the appropriate SSH keys, and sets `sudo` permissions. The playbook relies upon the variables passed on the command-line, which tell it what hosts should be affected and what user account (and password, via the `-k` and `-K` parameters) to use to modify the remote server.

With a cloud instance, this needed to change. First, you don't know the password; you're required to use an SSH key for authentication. You generally _do_ know the user account, but not the password. After a short bit of trial and error, I found the following command-line worked with a freshly-booted cloud instance:

    ansible-playbook bootstrap.yml --extra-vars \
    "hosts=172.16.6.7 user=ubuntu" --private-key=~/.ssh/cloudkey.pem

The command is largely the same, with two major changes:

* The `-k` and `-K` parameters are gone. The password is unknown, so there's no point in telling Ansible to prompt for any passwords.
* The `--private-key` parameter supplies the path and name of the SSH keypair (obtained from your cloud platform) that allow initial authentication to the new cloud instance.
* You'll note I omitted the `-i` parameter to specify an inventory file; this is because I created an `ansible.cfg` in this directory and specified the inventory file there. (It makes the command-line easier and simpler.)

As before, I use the `--extra-vars` parameter to supply the specific host being bootstrapped into Ansible as well as the name of the initial user account (in this case, I'm bootstrapping an Ubuntu cloud image, so the initial user is "ubuntu"). Once the initial bootstrap process completes successsfully, I can then run subsequent playbooks against this cloud instance with no further configuration or command-line parameters required.



[link-1]: http://www.ansible.com/
[xref-1]: {{< relref "2015-05-26-bootstrap-servers-ansible.md" >}}
