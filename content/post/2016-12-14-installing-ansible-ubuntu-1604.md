---
author: slowe
categories: Tutorial
comments: true
date: 2016-12-14T00:00:00Z
tags:
- Ansible
- Linux
- CLI
- Ubuntu
- Python
title: Installing Ansible 2.2 on Ubuntu 16.04
url: /2016/12/14/installing-ansible-ubuntu-1604/
---

A few weeks ago I wrote a post about [installing Ansible 2.2 on Fedora 25][xref-1]; today, I'd like to tackle what's involved in installing Ansible 2.2 on [Ubuntu][link-1] 16.04. This post, like its Fedora counterpart, stems from my ongoing evaluation of Linux distributions and desktop environments. While the information here is very similar to the information in the Fedora post, I'm putting it in its own post in the hopes of making the information easier for readers to find.

It's not really a secret that I like to run Ansible in a [Python virtualenv][link-3], but I don't believe that it will make any difference to the procedure described in this post. The errors that result when trying to install Ansible 2.2 without the necessary prerequisite packages should be the same either way (in a virtualenv or not). However, I'm happy to be corrected if someone knows otherwise.

To create a Python virtualenv, you'll first need virtualenv installed. I prefer to install virtualenv globally for all users using this command:

    sudo -H pip install virtualenv

Alternately, you could install it via a package, with `apt install virtualenv`. As far as I can tell, either approach should work just fine.

Once virtualenv is installed, then create a virtualenv for Ansible:

    virtualenv ~/Envs ansible

Then activate the virtualenv (substitute the appropriate path where you created the virtualenv, naturally):

    source ~/Envs/ansible/bin/activate

At this point, you can try a `pip install ansible`, but it will fail. To address the errors, you need to install some additional development libraries. You can install the required libraries with this command:

    sudo apt install libffi-dev python-dev libssl-dev

Once those packages are installed, _then_ you're ready to install Ansible into the virtualenv:

    pip install ansible

(Note that `sudo` is _not_ used here because you are installing Ansible into your own virtual environment.)

You should now be ready to enjoy all of Ansible's automation goodness from your Ubuntu 16.04 system. Enjoy!



[link-1]: https://www.ubuntu.com
[link-2]: https://www.ansible.com/
[link-3]: https://virtualenv.pypa.io/
[xref-1]: {{< relref "2016-11-25-installing-ansible-fedora-25.md" >}}
