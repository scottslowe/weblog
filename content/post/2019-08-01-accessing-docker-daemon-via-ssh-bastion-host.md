---
author: slowe
categories: Information
comments: true
date: 2019-08-01T19:00:00Z
tags:
- Docker
- Linux
- SSH
- CLI
title: Accessing the Docker Daemon via an SSH Bastion Host
url: /2019/08/01/accessing-docker-daemon-via-ssh-bastion-host/
---

Today I came across [this article][link-1], which informed me that (as of the 18.09 release) you can use SSH to connect to a [Docker][link-2] daemon remotely. That's handy! The article uses `docker-machine` (a useful but underrated tool, I think) to demonstrate, but the first question in my mind was this: can I do this through an SSH bastion host? Read on for the answer.<!--more-->

If you're not familiar with the concept of an SSH bastion host, it is a (typically hardened) host through which you, as a user, would proxy your SSH connections to other hosts. For example, you may have a bunch of EC2 instances in an AWS VPC that do not have public IP addresses. (That's reasonable.) You could use an SSH bastion host---which _would_ require a public IP address---to enable SSH access to otherwise inaccessible hosts. I wrote a post about [using SSH bastion hosts back in 2015][xref-1]; give that post a read for more details.

The syntax for connecting to a Docker daemon via SSH looks something like this:

    docker -H ssh://user@host <command>

So, if you wanted to run `docker container ls` to list the containers running on a remote system, you'd run this command:

    docker -H ssh://slowe@10.11.12.13 container ls

Note that there's no way to pass `sudo` via this syntax, so you'll need to ensure that the user you specify on the command line has the ability to run `docker` commands without privilege escalation. (You could add them to the "docker" group on the remote system, for example.)

Now, what about doing this through an SSH bastion host? Let's say you have this configuration stanza in your `~/.ssh/config` file (the IP address shown below formerly belonged to an EC2 instance in one of my AWS accounts, but has since been terminated---_do not try to connect to this system!_):

```text
Host bastion-host
  HostName 18.191.181.211
  User ubuntu
  IdentityFile ~/.ssh/sshkey_rsa

Host 10.100.15.*
  User ubuntu
  IdentityFile ~/.ssh/sshkey_rsa
  ProxyCommand ssh bastion-host -W %h:%p
```

Assuming there was an instance at 10.100.15.25, this configuration would allow you to simply run `ssh 10.100.15.25` and it would attempt to connect as user "ubuntu", using the specified private key, through the SSH bastion host defined as "bastion-host". (This is all described in detail in a number of posts all over the Internet, as well as [here][xref-2], [here][xref-3], [here][xref-4], and [here][xref-5] on this site).

So, if you have a working SSH bastion configuration, the cool thing about accessing a remote Docker daemon over SSH is that the bastion host configuration is _totally transparent_ and _just works._

Using the working configuration described above, this command will work straight out of the box:

    docker -H ssh://10.100.15.25 container ls

SSH will connect to the remote Docker daemon via the configured bastion host, with no additional effort or configuration required on your part. (All you need is a working SSH bastion host configuration already in place.)

Nifty, eh?

[link-1]: https://medium.com/@sujaypillai/dockertips-connect-to-docker-daemon-over-ssh-using-docker-compose-f4b189dd8951
[link-2]: https://www.docker.com/
[xref-1]: {{< relref "2015-11-21-using-ssh-bastion-host.md" >}}
[xref-2]: {{< relref "2015-12-04-use-case-ssh-bastion-host.md" >}}
[xref-3]: {{< relref "2015-12-24-running-ansible-through-ssh-bastion-host.md" >}}
[xref-4]: {{< relref "2016-09-13-ssh-bastion-host-follow-up.md" >}}
[xref-5]: {{< relref "2017-05-26-bastion-hosts-custom-ssh-configs.md" >}}
