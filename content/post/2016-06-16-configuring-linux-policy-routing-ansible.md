---
author: slowe
categories: Explanation
comments: true
date: 2016-06-16T00:00:00Z
tags:
- Linux
- Ansible
- CLI
- Networking
title: Configuring Linux Policy Routing using Ansible
url: /2016/06/16/configuring-linux-policy-routing-ansible/
---

In this post, I'm going to talk about using [Ansible][link-1] to configure policy routing on Linux. If you're not familiar with Linux policy routing, have a look at [this post][xref-1], and also review [this post][xref-2] for one potential use case (I'm sure there are a number of other quite valuable use cases).

As you may recall from the policy routing introductory post, there are three steps involved in configuring policy routing:

1. You must define the new routing table in `/etc/iproute2/rt_tables`
2. You must add routes to the new routing tables
3. You must define rules for when the new routing table is consulted

All three of these tasks can be handled via Ansible.

To address step #1, you can use Ansible's "lineinfile" module to add a reference to the new routing table in `/etc/iproute2/rt_tables`. For example, consider this Ansible task:

```
- lineinfile: dest=/etc/iproute2/rt_tables line="200 eth1"
```

This snippet of Ansible code would add the line "200 eth1" to the end of the `etc/iproute2/rt_tables` file (if the line does not already exist). This takes care of task #1.

For tasks #2 and #3, you can use a Jinja2 template. Because the creation of the policy routing rule and the routing table entries can be performed via the interface configuration file, we can write a Jinja2 template that will generate an interface configuration file with the appropriate `ip rule` and `ip route` statements.

Consider this Jinja2 template:

``` liquid
{% raw %}
{% if item.bootproto == 'static' %}
auto {{ item.device }}
iface {{ item.device }} inet static
{% if item.address is defined %}
address {{ item.address }}
{% endif %}
{% if item.netmask is defined %}
netmask {{ item.netmask }}
{% endif %}
{% if item.gateway is defined %}
gateway {{ item.gateway }}
{% else %}
{% if item.iface_gw is defined %}
up ip route add default via {{ item.iface_gw }} dev {{ item.device }} table {{ item.device }}
up ip rule add from {{ item.address }} table {{ item.device }}
down ip route del default table {{ item.device }}
{% endif %}
{% endif %}
{% endif %}

{% if item.bootproto == 'dhcp' %}
auto {{ item.device }}
iface {{ item.device }} inet dhcp
{% endif %}

{% if item.bootproto == 'manual' %}
auto {{ item.device }}
iface {{ item.device }} inet manual
pre-up ip link set {{ item.device }} up
post-down ip link set {{ item.device }} down
{% endif %}
{% endraw %}
```

To render this template into an actual interface configuration file, use this snippet (or something like it) in an Ansible playbook:

``` yaml
- name: Use Jinja2 template to create interface config file
  template:
    src: "ifcfg.j2"
    dest: "/etc/network/interfaces.d/ifcfg-{{ item.device }}"
  with_items: network_interfaces
  when: network_interfaces is defined
```

This snippet only runs when the variable `network_interfaces` is defined, which you can handle with this bit of YAML in an Ansible variables file (either as a host variable, a group variable, or a role variable):

``` yaml
network_interfaces:
  - device: eth1
    bootproto: static
    address: 192.168.100.101
    netmask: 255.255.255.0
    iface_gw: 192.168.100.254
```

With this setup in place---the Jinja2 template, the `template` task in an Ansible playbook, and the `network_interfaces` variable defined with the appropriate values---then it all comes together to dynamically build an interface configuration file that looks something like this (this is the file that would be created using the information supplied in this article):

``` text
auto eth1
address 192.168.100.101
netmask 255.255.255.0
up ip route add default via 192.168.100.254 dev eth1 table eth1
up ip rule add from 192.168.100.101 table eth1
down ip route del default table eth1
``` 

Obviously, the beauty of this solution is that it's automated: you simply edit the variables file to contain the information you wish, and Ansible will take care of the rest. There's a lot of stuff I didn't list here, like flushing routes and such, that you'd want to add, but this should give you a pretty good starting point.


[link-1]: http://www.ansible.com/

[xref-1]: {{< relref "2013-05-29-a-quick-introduction-to-linux-policy-routing.md" >}}
[xref-2]: {{< relref "2013-05-30-a-use-case-for-policy-routing-with-kvm-and-open-vswitch.md" >}}
