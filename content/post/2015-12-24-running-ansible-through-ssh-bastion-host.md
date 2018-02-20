---
author: slowe
categories: Education
comments: true
date: 2015-12-24T00:00:00Z
tags:
- Ansible
- SSH
- Linux
- CLI
title: Running Ansible Through an SSH Bastion Host
url: /2015/12/24/running-ansible-through-ssh-bastion-host/
---

This post will expand on some previous posts---one showing you [how to set up and use an SSH bastion host][xref-1] and a second [describing one use case for an SSH bastion host][xref-2]---to show how the popular configuration management tool [Ansible][link-3] can be used through an SSH bastion host.

The configuration/setup required to run Ansible through an SSH bastion host is actually reasonably straightforward, but I saw a _lot_ of incomplete articles out there as I was working through this myself. My hope is to supplement the existing articles, as well as the Ansible documentation, to make this sort of configuration easier for others to embrace and understand.

## Prerequisites

There are two key concepts involved here that you'll want to be sure you understand before you proceed:

1. You'll want to make sure you're comfortable with [using an SSH bastion host][xref-1]. If you don't understand how this works or how to set it up, I recommend you spend some time on this topic first, as it's crucial to how Ansible will behave/function. [This article][link-2] by Grant Taylor has some good information.
2. Spend some time making sure you know how to [use SSH multiplexing][xref-3]. This is useful for Ansible in general, but it's even more critical in situations where you're using a bastion host.

Once you're comfortable with these areas, you're ready to proceed with setting up Ansible to work through an SSH bastion host.

## Configuration

There are two primary pieces involved:

1. An Ansible-specific SSH configuration file that defines the SSH bastion host and other parameters. Using an Ansible-specific SSH configuration file keeps your own SSH configuration file (typically found in `~/.ssh/config`) "clean," and facilitates better source code control.
2. Entries in the Ansible configuration file that pull in the custom SSH configuration file.

Let's look at each of these independently.

### Custom SSH Configuration File

This is pretty straightforward, and something that I've already discussed in my earlier posts on using an SSH bastion host and using SSH multiplexing. What you'll need to do here is a) define the hosts that should be accessed via the bastion host; and b) define how SSH will connect to the bastion host.

Here's a sample SSH configuration file. I'll break down some of the various settings found here below.

```
Host 10.10.10.*
  ProxyCommand ssh -W %h:%p bastion.example.com
  IdentityFile ~/.ssh/private_key.pem

Host bastion.example.com
  Hostname bastion.example.com
  User ubuntu
  IdentityFile ~/.ssh/private_key.pem
  ControlMaster auto
  ControlPath ~/.ssh/ansible-%r@%h:%p
  ControlPersist 5m
```

What does this configuration file do? Here's a quick rundown:

* The `Host 10.10.10.*` line indicates that _all_ hosts in that subnet will use the settings defined in that block; specifically, all hosts will be accessed using the `ProxyCommand` setting and connect via `bastion.example.com` using the specified private key.
* The `Host bastion.example.com` combines settings for acting as an SSH bastion host with settings for using SSH multiplexing (the `ControlMaster`, `ControlPath`, and `ControlPersist` settings).

You can store this file wherever you like, but make note of where the file is stored as you'll need that for the next step: configuring Ansible to use the custom SSH settings.

### Ansible Configuration

This custom SSH configuration file is useless without explicitly telling Ansible to use these settings when connecting to Ansible-managed hosts. This is accomplished by creating (or modifying) `ansible.cfg` and adding the following setings:

```
[ssh_connection]
ssh_args = -F ./ssh.cfg -o ControlMaster=auto -o ControlPersist=30m
control_path = ~/.ssh/ansible-%%r@%%h:%%p
```

This is an area where I found a lot of incorrect and/or incomplete information. A number of articles failed to note that these settings need to be in the `[ssh_connection]` portion of `ansible.cfg`; based on my testing, it won't work properly unless the settings are in the right section. 

The `-F` parameter is where you'll provide the path (full/absolute or relative) to the custom SSH configuration file you created earlier. To simplify being able to keep all these files in source control, I prefer to place `ansible.cfg` in the current directory with my playbooks, inventory, roles, and host/group variables. I also prefer to keep the custom SSH configuration file here, which is why you see `-F ./ssh.cfg` in the example text above. Obviously, you'll want to tailor this to your specific environment.

Once you have these two pieces of configuration in place, you'll be able to run Ansible ad-hoc commands and/or Ansible playbooks against remote hosts via the SSH bastion host. Note that you'll still need entries in the Ansible inventory file for these remote hosts; the use of an SSH bastion host does not change that requirement.

For example, let's say I wanted to run the playbook `hypervisors.yml` against one of the remote servers using the sample values/configuration I've shown in this article. I could run something like this:

    ansible-playbook hypervisors.yml --limit 10.10.10.23

This would run the `hypervisors.yml` playbook against _only_ the host 10.10.10.23, which would need to have an entry in the inventory. (Note that I didn't specify an inventory file; this is because I typically use `ansible.cfg` to tell Ansible to look in the current directory for the inventory file.) The connection to 10.10.10.23 would occur via the SSH bastion host named "bastion.example.com" using the specified private key and specified user account, and would leverage SSH multiplexing to speed up the SSH connections and Ansible transactions occurring over those connections.

To again remind you why this might be useful, consider the ability to use Ansible as a configuration management tool against cloud instances that are not externally accessible (perhaps they exist inside an OpenStack Neutron tenant network or a private AWS VPC without Internet access). In this case, being able to use a bastion host to run Ansible against such hosts prevents you from needing to burn public/floating IP addresses or exposing hosts to external traffic when it really isn't absolutely necessary.

## Other Resources

[This article][link-4] has information on setting up Ansible Tower for the use of an SSH bastion host. Also, [this GitHub gist][link-1] has much of the same information as this article, so you may find it helpful in combination with this article or others.

If you have any questions or want more information, [hit me up on Twitter][link-5] and I'll do my best to help you out.

**UPDATE 14 Sep 2016:** I've removed the information on SSH agent forwarding from this article, as it is not required when using an SSH bastion host.

**UPDATE 17 Aug 2017:** In Ansible 2, you're able to do this "natively" in Ansible; see [this entry in the FAQ][link-6].



[link-1]: https://gist.github.com/seansawyer/8fe009e67f7e01344328
[link-2]: http://dotfiles.tnetconsulting.net/articles/2015/0506/empowering-openssh.html
[link-3]: http://www.ansible.com/
[link-4]: http://blog.dualspark.com/ansible/configuration-management/aws/ssh/2014/12/19/ansible-tower-ssh-agent-forwarding.html
[link-5]: https://twitter.com/scott_lowe
[link-6]: https://docs.ansible.com/ansible/latest/faq.html#how-do-i-configure-a-jump-host-to-access-servers-that-i-have-no-direct-access-to
[xref-1]: {{< relref "2015-11-21-using-ssh-bastion-host.md" >}}
[xref-2]: {{< relref "2015-12-04-use-case-ssh-bastion-host.md" >}}
[xref-3]: {{< relref "2015-12-11-using-ssh-multiplexing.md" >}}