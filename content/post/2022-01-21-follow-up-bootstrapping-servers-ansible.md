---
author: slowe
categories: Information
comments: true
date: 2022-01-21T12:00:00-07:00
tags:
- Ansible
- Linux
- Ubuntu
title: "Follow Up: Bootstrapping Servers into Ansible"
url: /2022/01/21/follow-up-bootstrapping-servers-ansible/
---

Seven years ago, I wrote a quick post on [bootstrapping servers into Ansible][xref-1]. The basic gist of the post was that you can use variables on the [Ansible][link-1] command-line to specify hosts that aren't part of your inventory or log in via a different user (useful if the host doesn't yet have a dedicated Ansible user account because you want to use Ansible to create that account). Recently, though, I encountered a situation where this approach doesn't work, and in this post I'll describe the workaround.<!--more-->

In one of the Slack communities I frequent, someone asked about using the approach described in the original blog post. However, they were having issues connecting. Specifically, this error was cropping up in the Ansible output (names have been changed to protect the innocent):

```text
fatal: [new-server.int.domain.test]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: ansible@new-server.int.domain.test: Permission denied (publickey,password).", "unreachable": true}
```

Now, this is odd, because the Ansible command-line being executed included the parameters I mentioned in the original blog post:

```shell
ansible-playbook bootstrap.yml -i inventory/hosts -K \
--extra-vars "hosts=new-server.int.domain.test user=john"
```

For some reason, though, it was ignoring that parameter and attempting to connect as `ansible`. At first, I thought this was just an SSH error, but the person assured me that they were definitely able to SSH into the specified host as the specified user.

That was when we found the difference between this environment and the environment I'd used: in this environment, the `ansible_user` variable was defined in the specified inventory file. I hadn't defined `ansible_user`. Could that be it?

I asked the person to try specifying `ansible_user` instead of `user` on the command line, like so:

```shell
ansible-playbook bootstrap.yml -i inventory/hosts -K \
--extra-vars "hosts=new-server.int.domain.test ansible_user=john"
```

It worked!

So, the key takeaway is: if you're thinking of using this bootstrapping technique with your Ansible environment, be sure to specify `ansible_user` on the Ansible command-line _if_ you've defined `ansible_user` in your inventory. Otherwise, you should be able to just use `user`.

Credit for finding this issue with the approach I described in the original blog post, and for testing the workaround, goes to James Cox. Thanks James! As always, feel free to reach out to [me on Twitter][link-2]---or find me in any of the Slack communities I regularly visit---and ask questions, provide feedback, or just say hello. I'd love to hear from you.

[link-1]: https://www.ansible.com/
[link-2]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2015-05-26-bootstrap-servers-ansible.md" >}}
