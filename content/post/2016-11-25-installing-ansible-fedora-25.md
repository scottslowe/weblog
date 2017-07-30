---
author: slowe
categories: Tutorial
comments: true
date: 2016-11-25T00:00:00Z
tags:
- Ansible
- Linux
- CLI
- Fedora
- Python
title: Installing Ansible 2.2 on Fedora 25
url: /2016/11/25/installing-ansible-fedora-25/
---

As part of my ongoing investigation of the usability of various Linux distributions and desktop environments, I've been working with [Fedora 25][link-1]. As part of the investigation I need to see how to perform certain tasks, one of which is working with [Ansible][link-2]. As a result, I needed to install Ansible 2.2 on Fedora 25, and it turns out it wasn't as simple as `pip install ansible`.

I generally prefer to run Ansible in a [Python virtualenv][link-3], but I don't believe that it will make any difference to this procedure. However, I'm happy to be corrected if someone knows otherwise.

To create a Python virtualenv, you'll first need virtualenv installed. I prefer to install virtualenv globally for all users using this command:

    sudo -H pip install virtualenv

Once virtualenv is installed, then create a virtualenv for Ansible:

    virtualenv ~/Envs ansible

Then activate the virtualenv:

    source ~/Envs/ansible/bin/activate

At this point, you can try a `pip install ansible`, but it will fail. First, you need to install some additional development libraries that are required in order to install Ansible:

    sudo dnf install libffi-devel redhat-rpm-config python-devel openssl-devel

Once those packages are installed, _then_ you're finally ready to install Ansible into the virtualenv:

    pip install ansible

(Note that `sudo` is _not_ used here because you are installing Ansible into your own virtual environment.)

That's it---you're now ready to roll. Enjoy!



[link-1]: https://getfedora.org/
[link-2]: https://www.ansible.com/
[link-3]: https://virtualenv.pypa.io/
