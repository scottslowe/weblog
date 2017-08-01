---
author: slowe
categories: Explanation
comments: true
date: 2015-06-08T16:20:00Z
tags:
- Ansible
- Vagrant
- Linux
- CLI
title: Using Vagrant to Help Learn Ansible
url: /2015/06/08/using-vagrant-to-learn-ansible/
---

I've been spending some time with [Ansible][link-1] recently, and I have to say that it's really growing on me. While Ansible doesn't have a steep learning curve, there _is_ still a learning curve---albeit a smaller/less steep curve---so I wanted to share here a "trick" that I found for using [Vagrant][link-2] to help with learning Ansible. (I say "trick" here because it isn't that this is complicated or undocumented, but rather that it may not be immediately obvious how to combine these two.)

Note that this is not to be confused with using Ansible from within Vagrant as a provisioner; that's something different (see [the Vagrant docs][link-3] for more information on that use case). What I'm talking about is having a setup where you can easily explore how Ansible works and iterate through your playbooks using a Vagrant-managed VM.

Here are the key components:

1. You'll need a Vagrant environment (you know, a working `Vagrantfile` and any associated support files).
2. You'll need Ansible installed on the system where you'll be running Vagrant and the appropriate back-end virtualization platform (I tested this with [VMware Fusion][link-4], but there's nothing VMware-specific here).
3. In the same directory as the `Vagrantfile`, you'll need an Ansible configuration file and an Ansible inventory file (more on both of these in just a moment).

With the right setup (and I'll go into details on the setup shortly), you'll be able to do this:

* Run `vagrant up` to turn up the VMs in the `Vagrantfile`.
* Then run Ansible commands (ad-hoc commands or playbooks) against the VMs in the Vagrant environment.
* Use `vagrant destroy` to kill the VMs and wipe the environment clean.
* Run `vagrant up` again in order to start again with a fresh configuration for additional playbook testing or other ad-hoc commands.

Maybe this doesn't seem like a big deal to you, but having this ability to _easily_ iterate over an Ansible playbook, or to test an Ansible playbook against various Linux distributions, has come in extraordinarily handy as I've been learning Ansible.

The "trick" here lies in the Ansible configuration file and the Ansible inventory. Ansible, by default, will look in the current directory for a file named `ansible.cfg` for its configuration. By placing an Ansible configuration file with this name in the same directory as the Vagrantfile, we can configure Ansible specific for _that particular Vagrant environment._

Here's a very generic Ansible configuration file I use:

```
[defaults]
inventory = ./hosts
private_key_file = /Users/slowe/.vagrant.d/insecure_private_key
remote_user = vagrant
host_key_checking = False
```

The [documentation on the Ansible configuration file][link-5] can explain these in greater detail, but here's a quick breakdown of each line:

* Look in the current directory for the inventory in a file named `hosts`.
* Use the default Vagrant insecure SSH key, which (pretty much) every Vagrant box out there has pre-installed.
* Tell Ansible to use the "vagrant" account, which---like the SSH key---every Vagrant box out there has pre-defined.
* Don't bother with checking host SSH keys.

Then, in the Ansible inventory file (named `hosts` in the same directory as the Ansible configuration file and the `Vagrantfile`), you simply place the IP addresses of each VM in the Vagrant environment. If you're assigning IP addresses in the `Vagrantfile` (recommended), then it's a cinch to put this in the Ansible inventory as well. (I'm working on a way to automate this via Ruby in the `Vagrantfile`.)

With this configuration, Ansible will use the inventory file in the same directory as the `Vagrantfile`, which is where you'd need to be to run `vagrant up` anyway. This will tell Ansible to work on the VMs defined in the Vagrant environment, using the default "vagrant" user and the default (insecure) SSH key. No special inventory variables, no command-line variables, nothing special needed...just run your Ansible commands or Ansible playbooks. Easy!

The complete workflow looks something like this:

* If you are statically assining IP addresses in the `Vagrantfile`, copy those same addresses to the file named `hosts` in the same directory.
* From that directory, run `vagrant up` to create the VMs defined in the `Vagrantfile`.
* Once the VMs are provisioned, run Ansible ad-hoc commands (or run playbooks using `ansible-playbook`). For example, I wanted to verify some of the facts that are gathered by Ansible across various Linux distributions. I can show that information with this command:

        ansible -m setup all

    No need to specify the username (found in the configuration file named `ansible.cfg` in the same directory), the authentication method (it will use the private SSH key from the configuration file), or the inventory (also supplied by the configuration file). Just run Ansible and be done.
* When you're done with the environment, just run `vagrant destroy`. If you want to start with a clean, fresh environment, then `vagrant up` again and you're off to the races.

As I said earlier, I don't think there's anything terribly unique here, but it _is_ a pretty useful configuration (for me, at least). Hopefully others will find it useful as well.


[link-1]: http://www.ansible.com/home
[link-2]: https://www.vagrantup.com
[link-3]: https://docs.vagrantup.com/v2/provisioning/ansible.html
[link-4]: http://www.vmware.com/products/fusion/
[link-5]: http://docs.ansible.com/intro_configuration.html
