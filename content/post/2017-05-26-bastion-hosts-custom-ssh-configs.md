---
author: slowe
categories: Explanation
comments: true
date: 2017-05-26T00:00:00Z
tags:
- SSH
- Linux
- CLI
- Security
title: Bastion Hosts and Custom SSH Configurations
url: /2017/05/26/bastion-hosts-custom-ssh-configs/
---

The idea of an SSH bastion host is something I discussed [here][xref-1] about 18 months ago. For the most part, it's a pretty simple concept (yes, things can get quite complex in some situations, but I think these are largely corner cases). For the last few months, though, I've been trying to use an SSH bastion host and failing, and I could not figure out _why_ it wouldn't work. The answer, it turns out, lies in custom SSH configurations.<!--more-->

In my introduction on using SSH bastion hosts (linked above)---or in just about any tutorial out there on using SSH bastion hosts---brief mention is made of adding configuration information to SSH to use the bastion host. Borrowing from my original post, if you had an instance named "private1" that you wanted to access via a bastion named "bastion", the SSH configuration information might look like this:

    Host private1
      IdentityFile ~/.ssh/rsa_private_key
      ProxyCommand ssh user@bastion -W %h:%p

    Host bastion
      IdentityFile ~/.ssh/rsa_private_key

Normally, that information would go into `~/.ssh/config`, which is the default SSH configuration file.

In my case, I only allow public key authentication to "trusted" systems (I vaguely recall an article I read a while ago about a potential concern regarding the key exchange, though I can't find that article now). So, in my `~/.ssh/config` file, I allow public key authentication only for specific hosts but not for all hosts. I do that using a configuration that looks something like this (IP addresses and SSH keys have been changed to protect the innocent):

    Host 192.168.100.*
      PubkeyAuthentication yes
      IdentityFile ~/.ssh/path_to_rsa_private_key

    Host 127.0.0.1
      PubkeyAuthentication yes

    Host *
      PubkeyAuthentication no

In this case, SSH public key authentication is allowed to my home subnet and to localhost (this is necessary for [Vagrant][link-2] to work as expected when using [VirtualBox][link-3], by the way), but it is _not_ permitted for all other hosts. Call me paranoid.

By now you're probably thinking, "Duly noted, Scott is a bit wacko, but what does this have to do with bastion hosts?"

Fair question! I have one more thing to discuss, then I'll come to the crux of the matter. To work around my default SSH configuration, I often employ a custom SSH configuration for a project. For example, I might be working with [Terraform][link-4] and creating a bunch of instances in AWS. I won't necessarily know the IP addresses up front, and instead of cluttering up the main SSH configuration file in `~/.ssh/config` I'll create a custom SSH configuration just for that project. For example, the custom SSH configuration might look like this:

    Host *
      PubkeyAuthentication yes
      User ec2-user
      IdentityFile /path/to/project/specific/ssh/key/file

I'll then reference this specific configuration file (using the `-F <file>` parameter) when connecting to the instances, like this:

    ssh -F ssh.cfg <IP address of instance>

This works marvelously well, except in those instances when I forget to add the `-F` parameter and bang my head on the desk wondering why SSH won't connect.

OK, I'm finally getting to my point (really). Let's suppose I'm working on a project where I'm turning up instances in AWS (or Google or Azure, the cloud provider doesn't really matter) and I'd like to use a bastion host. I'll create a custom SSH configuration to enable public key authentication and define the bastion host, like this:

    Host private1
      IdentityFile /path/to/project/SSH/key
      ProxyCommand ssh bastion -W %h:%p

    Host bastion
      IdentityFile /path/to/project/SSH/key

    Host *
      PubkeyAuthentication yes

Looks good, right? I should just be able to run `ssh -F ssh.cfg private1` and be connected to my private cloud instance via the bastion host using the specified SSH key (which I injected into the cloud instance using `cloud-init` or the equivalent).

_It won't work._

Why? In retrospect, it's an obvious reason, but it took me quite a while to find it. The secret lies in the `ProxyCommand` line, which _must also reference your custom SSH configuration._ SSH is just doing what it's told---you try to connect to "private1" and it runs the command you told it to run. Unfortunately, the command you told it to run uses the system's _default_ SSH configuration, which (in my case) doesn't allow public key authentication. As a result, it fails. The fix is to modify the custom configuration to look like this instead:

    Host private1
      IdentityFile /path/to/project/SSH/key
      ProxyCommand ssh -F ssh.cfg bastion -W %h:%p

    Host bastion
      IdentityFile /path/to/project/SSH/key

    Host *
      PubkeyAuthentication yes

With this custom configuration in place (and assuming a default configuration like mine that disallows public key authentication by default), then you're able to run `ssh -F ssh.cfg private1` and you'll be connected to the private cloud instance via the bastion, authenticated using the specified key pair. (Clearly, I've glossed over details like resolving host names, so you'd need to ensure your instances have DNS entries or use `Host` directives in your custom SSH configuration).

The key takeaway: if you're using custom SSH configurations with SSH bastion hosts, then---depending on your setup---you may need to make sure your `ProxyCommand` setting _also_ references the custom configuration.

**UPDATE:** Several folks have contacted me regarding the SSH key exchange, and how the private key never actually leaves the client. I've updated the post accordingly. I'll keep looking for the original article that sparked my concern over limiting public key authentication, and if I find it I'll add the link here. Thanks for the feedback!



[link-2]: https://www.vagrantup.com/
[link-3]: https://www.virtualbox.org/
[link-4]: https://www.terraform.io/
[xref-1]: {{< relref "2015-11-21-using-ssh-bastion-host.md" >}}
