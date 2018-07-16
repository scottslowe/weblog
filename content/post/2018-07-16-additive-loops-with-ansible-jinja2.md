---
author: slowe
categories: Explanation
comments: true
date: 2018-07-16T15:00:00Z
tags:
- Ansible
- Jinja2
- Linux
- CLI
title: Additive Loops with Ansible and Jinja2
url: /2018/07/16/additive-loops-with-ansible-jinja2/
---

I don't know if "additive" is the right word, but it was the best word I could come up with to describe the sort of configuration I recently needed to address in [Ansible][link-2]. In retrospect, the solution seems pretty straightforward, but I'll include it here just in case it proves useful to someone else. If nothing else, it will at least show some interesting things that can be done with Ansible and Jinja2 templates.<!--more-->

First, allow me to explain the problem I was trying to solve. As you may know, Kubernetes 1.11 was recently released, and along with it a new version of `kubeadm`, the tool for bootstrapping Kubernetes clusters. As part of the new release, the Kubernetes community released [a new setup guide for using `kubeadm` to create a highly available cluster][link-1]. This setup guide uses new functionality in `kubeadm` to allow you to create "stacked masters" (control plane nodes running both the Kubernetes components as well as the etcd key-value store). Because of the way etcd clusters work, and because of the way you create HA control plane members, the process requires that you start with a single etcd node, then add the second node, and finally add the third node. If you start out with all three, then the cluster won't establish quorum and establishing a functional Kubernetes control plane node will fail.

As a result, this means the `kubeadm` configuration file used on the first Kubernetes control plane node must be written to boot a new single-node cluster. However, the `kubeadm` configuration file on the second control plane node specifies the first node and _adds_ the second node. Likewise, the `kubeadm` configuration file for the third control plane node has the first and second nodes and _adds_ the third node (hence my use of "additive loops" in the blog post title).

So, when I set out to automate this process using Ansible, I knew this wasn't going to be your standard "run-of-the-mill" inventory loop in a Jinja2 template---at least, that wasn't _all_ it was going to be.

I arrived at a solution with three templates (one for each of the stacked masters). In the first template, the section for configuring etcd specifies the "initial-cluster" in the following way:

``` jinja2
initial-cluster: "{%- for host in groups['masters'] -%}
{%- if loop.first -%}
{{ hostvars[host]['ansible_fqdn'] }}=https://{{ hostvars[host]['ansible_' + primary_interface]['ipv4']['address'] }}:2380{%- endif -%}
{%- endfor -%}"
```

The result, as you can probably determine, is that the first item in the loop---the first server in that inventory group---is the only item rendered in the template.

The second server was a bit more challenging, mostly because of a stupid error on my part. After accounting for my stupid error (if you must know, I was using `hostvars[inventory_hostname]` instead of `hostvars[host]`), this is where I ended:

``` jinja2
initial-cluster: "{%- for host in groups['masters'] -%}
{%- if loop.first -%}
{{ hostvars[host]['ansible_fqdn'] }}=https://{{ hostvars[host]['ansible_' + primary_interface]['ipv4']['address'] }}:2380,{%- endif -%}
{%- if (not loop.last and not loop.first) -%}
{{ hostvars[host]['ansible_fqdn'] }}=https://{{ hostvars[host]['ansible_' + primary_interface]['ipv4']['address'] }}:2380{%- endif %}
{%- endfor -%}"
```

This adds to the previous template by including the `if (not loop.last and not loop.first)` conditional, which---for a group of three---ends up meaning the second host in the group. Great; now we have a template for the first host which has only the first host, and a template for the second host which has both the first and second hosts listed.

The third and final template is, after all, a standard "run-of-the-mill" inventory loop:

``` jinja2
initial-cluster: "{%- for host in groups['masters'] -%}
{%- if loop.last -%}
{{ hostvars[host]['ansible_fqdn'] }}=https://{{ hostvars[host]['ansible_' + primary_interface]['ipv4']['address'] }}:2380
{%- else -%}
{{ hostvars[host]['ansible_fqdn'] }}=https://{{ hostvars[host]['ansible_' + primary_interface]['ipv4']['address'] }}:2380,
{%- endif -%}{%- endfor -%}"
```

This one probably requires no explanation; it simply iterates through the inventory group. The only "special" thing about it is knowing whether it should include a comma after the rendered text or not by testing for `loop.last`.

I _should_ probably explain, though, the use of the `['ansible_' + primary_interface]` syntax. I needed a way to get the IP address of an interface on the target system, but the names of interfaces change depending on platform. For example, on one platform the first Ethernet interface may be called "eth0"; on another, it may be called "ens0p0" or similar. Using the "primary_interface" variable allows me to use the same Ansible playbooks across platforms, only needing to adjust the variable when interface names change between platforms. (This was an Ansible trick I picked up from [my colleague Craig][link-3].) Ansible substitutes the value of the variable when pulling the host variable to render the template. If I specify "eth0" as the value for "primary_interface", Ansible will treat that as `ansible_eth0` and thus pull the IPv4 address for eth0 when rendering the template. Handy! (Thanks Craig!)

As I said, looking back on the solution now it seems simple and straightforward. There's probably a more elegant solution out there somewhere, but for now this will suffice. Oh, and if you're interested in seeing the full template or the Ansible playbook used to render the template, have a look at the `ansible/kubeadm-etcd-template` directory in [my GitHub "learning-tools" repository][link-4]. Enjoy!

[link-1]: https://kubernetes.io/docs/setup/independent/high-availability/
[link-2]: https://www.ansible.com/
[link-3]: https://twitter.com/craig_tracey/
[link-4]: https://github.com/scottslowe/learning-tools/
