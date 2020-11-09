---
author: slowe
categories: Tutorial
comments: true
date: 2016-04-30T00:00:00Z
tags:
- Ansible
- CLI
- macOS
- Python
title: Installing Ansible 2.x in a Python Virtualenv on OS X
url: /2016/04/30/installing-ansible-in-virtualenv/
---

In this post, I'm going to walk you through the steps to install [Ansible][link-1] 2.x into a [Python][link-3] virtual environment (virtualenv) on [OS X][link-4]. There's nothing terribly hard or unusual about this process, but I wanted to document it here for folks who might be new to the process (or who might be interested in _why_ using this approach could be beneficial).

I'm stumbled into this process because I had been using Ansible 1.9.x and wanted to upgrade to Ansible 2.x so that I could use some of the new [OpenStack][link-2]-related modules. (These are modules that allow you to manipulate OpenStack-based resources, like instances or networks, using Ansible playbooks, and they were introduced with the release of Ansible 2.x.) The new modules had some additional Python dependencies, and installing these Python dependencies on OS X can be challenging at times (due to [System Integrity Protection][link-5] [SIP]). For example, installing the `shade` module on my OS X El Capitan system ran afoul of SIP.

The answer is to use Python virtual environments (aka "virtualenvs"). Virtualenvs provide a mechanism whereby you can isolate Python dependencies between different Python-based projects. You create a Python virtualenv, then you install the Python module _into_ that virtualenv. All other environments---including other virtualenvs and the system environment---are left untouched. Handy, right? Ansible, as a Python-based project, can be run inside a virtualenv so that you can isolate Ansible's dependencies away from the rest of the system.

Here's what the process looks like:

1. First, you'll need to install support for Python virtualenvs. Although some folks have said **not** to use `sudo` when installing via Pip, I found it necessary:

        sudo -H pip install virtualenv

    Note that you might need to install Pip first, which---fortunately---is pretty straightforward (`easy_install pip`).

2. Once you have virtualenv support installed, then create a virtualenv into which you'll install the Ansible dependencies:

        virtualenv --system-site-packages ansible

    This will create a virtualenv called "ansible" in a directory named "ansible" in the current working directory. I used the `--system-site-packages` flag to allow this virtualenv to have access to the system site packages. I saw various recommendations both for and against this approach; my line of thinking was that this would help prevent some duplication of modules.

3. Activate the virtualenv by running `source ansible/bin/activate` (if you used a name other than "ansible", substitute that in the command). This will modify your prompt so that the name of the virtualenv is shown in parentheses before the prompt.

4. Now install Ansible using `pip install ansible`, along with any other necessary dependencies. In my case, I wanted to use the OpenStack modules, so I also ran `pip install shade`.

5. When you're done with the virtualenv, run `deactivate`. You'll see your prompt revert to normal.

Now, anytime you want to run Ansible, activate the "ansible" virtualenv (see step #3). This will put you into the appropriate virtualenv with all the necessary modules and dependencies. When you're done, simply deactivate the virtualenv (see step #5) and go on about your day. Easy as cake!

[link-1]: https://www.ansible.com/
[link-2]: http://www.openstack.org/
[link-3]: https://www.python.org/
[link-4]: http://www.apple.com/osx/
[link-5]: https://en.wikipedia.org/wiki/System_Integrity_Protection