---
author: slowe
categories: Explanation
comments: true
date: 2015-05-26T09:35:00Z
tags:
- Ansible
- Linux
- Ubuntu
title: Bootstrapping Servers into Ansible
url: /2015/05/26/bootstrap-servers-ansible/
---

As part of a lab rebuild I've been doing over the last few weeks (funny how hardware failures can lead to a lab rebuild), I've been expanding the use of [Ansible][link-1] for configuration automation. In this post, I'm going to share the process I've created for bootstrapping newly-built servers into Ansible.

I developed this Ansible bootstrapping process to work in conjunction with the fully automated Ubuntu installation method that I described in [an earlier post][xref-1]. The idea is that I would be able to boot a new server (virtual or physical), choose a configuration from the PXE menu, and a few minutes later have a built Ubuntu system. Then, with a single command, I could "bootstrap" the server into an Ansible configuration automation system. This latter part---configuring systems to work with Ansible---is what I'll be describing here.

First, a (very) brief overview of Ansible. Ansible is a configuration automation tool that leverages standard SSH connections to remote devices in order to perform its work. Ansible is agentless, so no software has to be pre-installed on the managed servers, but this means Ansible has to authenticate against remote systems in order to establish these SSH connections. This authentication should, in ideal circumstances, be fully automated (i.e., there should not be a prompt to the user for authentication credentials). This is typically accomplished through the use of SSH keys. The challenge here is this: how do you get the systems to have the user account and corresponding SSH keys so that Ansible can fully (and without prompting for passwords) manage the systems?

There are a couple of potential ways to tackle this issue. I _could_ have modified the preseed files I was using for the fully automated Ubuntu installation to create the user account and install the corresponding SSH key. Alternately, I _could_ have written a small shell script (called something like `postinstall.sh`) that I would manually run after provisioning, and it would create the necessary user account and install the corresponding SSH key. Or I could combine these two approaches and call `postinstall.sh` at the end of the preseed file.

I knew I could specify a per-host remote user name (by including information in the Ansible inventory file), but that wasn't scalable; I'd have to put the information in the inventory file, run Ansible, then modify the inventory file again to remote the information.

As I dug into Ansible, I found something that proved to be very important: it's possible to pass variables to Ansible on the command line, and have those variables be used in the Ansible playbook you're calling. In particular, it's possible to use command-line variables to tell Ansible _the specific hosts_ against which a playbook should be executed, as well as _the remote user name_ that should be used when connecting to those hosts. That proved to be the solution I'd been seeking!

With that information in hand, I built an Ansible playbook that created an Ansible user account and installed an SSH key for that user account (this was accomplished using the "user" and "authorized_key" Ansible modules). The playbook did a couple other things as well, like handle `sudo` access for this new account. There's nothing special about the tasks in the playbook; the trick, in my opinion, is in the playbook header:

```yaml
- hosts: '{{ hosts }}'
  remote_user: '{{ user }}'
  sudo: yes
```

Every Ansible playbook starts with this header, and these lines tell it against which hosts (via the `hosts:` line) to execute the playbook, and which user account (via the `remote_user:` line) to use when connecting to those hosts. By using variables on these lines, it meant I could specify these values on the command-line---which in turn meant I had very granular control over which servers I wanted to affect and how Ansible would connect to those servers.

Let's look at an example. I knew that my preseed file always created a user account (I'll call that account "admin"). I also knew that the preseed file assigned a standard password to that account. Now, I _could_ have used this account for Ansible to connect, but I preferred to have a dedicated account for Ansible. So, using a command like this (which I've line-wrapped for your readability):

    ansible-playbook bootstrap.yml -i hosts -k -K --extra-vars \
    "hosts=newhost.domain.com user=admin"

I could execute the `bootstrap.yml` playbook (which is the playbook that set up a dedicated Ansible user account, with `sudo` access and an SSH key for authentication) against _ONLY_ "newhost.domain.com", using the "admin" user that I knew existed as a result of the configuration in the preseed file. If, for whatever reason, I wanted to bring another system into Ansible, but it wasn't built with my preseed file, then I could easily just change the value of the `user` parameter---no problem. (The "-k" and "-K" parameters just tell Ansible to prompt for password this time around.)

Once this playbook has executed though, the system now has a dedicated Ansible user account with `sudo` access and an SSH key for authentication, so no special variables or treatment is required for future playbook execution. If my main Ansible playbook is called `site.yml`, then I can execute it against all systems in my inventory with this command:

    ansible-playbook -i hosts site.yml

Ansible will connect to the hosts in the inventory file (named "hosts" in this case) and execute the playbook, using the default user name (the dedicated Ansible user account) and an SSH key for authentication. Because the bootstrap playbook has created all the necessary pieces, subsequent playbook runs require no additional user interaction whatsoever.

There are a few things to note about this process:

* Obviously, Ansible still needs a user account to use to authenticate for the initial bootstrap run. Because you can specify the user account on the command line as a variable, though, you can handle systems that may not have been built using a standardized method.
* You'll need to add the new host into the Ansible inventory before you run the bootstrap playbook. Otherwise, Ansible will complain there was no matching host.
* **Nothing** special is required in the Ansible inventory. You're welcome to use inventory variables, if you'd like, but they aren't needed at all for this process---which helps make the transition from first Ansible run to subsequent Ansible runs very seamless.

I know this may not seem like much, but I did want to share it here in the event that the use of command-line variables within a playbook---especially within the header of a playbook, as in this case---could prove useful to others. Thanks for reading!

[link-1]: http://www.ansible.com/home
[xref-1]: {{< relref "2015-05-20-fully-automated-ubuntu-install.md" >}}
