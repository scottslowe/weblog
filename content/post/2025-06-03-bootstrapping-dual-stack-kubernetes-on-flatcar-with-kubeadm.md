---
author: slowe
categories: Tutorial
comments: true
date: 2025-06-03T09:00:00-04:00
tags:
- CLI
- CoreOS
- IPv6
- Kubeadm
- Kubernetes
- Networking
title: Bootstrapping Dual-Stack Kubernetes on Flatcar with Kubeadm
url: /2025/06/03/bootstrapping-dual-stack-kubernetes-on-flatcar-with-kubeadm/
---

Recently I needed to be able to stand up a dual-stack (IPv4/IPv6) Kubernetes cluster on Flatcar Container Linux using `kubeadm`. At first glance, this seemed like it would be relatively straightforward, but as I dug deeper into it there were a few quirks that emerged. Given these quirks, it seemed like a worthwhile process to write up and publish here. In this post, you'll see how to use Butane and `kubeadm` to bootstrap a dual-stack IPv4/IPv6 Kubernetes cluster on AWS.<!--more-->

For those who are unfamiliar, [Flatcar Container Linux][link-1] is a container-optimized Linux distribution considered to be the spiritual successor to CoreOS. For configuring OS instances during provisioning, Flatcar uses Ignition (see [here][link-2] or [here][link-4] for more information). Ignition is intended to be machine-friendly, but not human-friendly. Users can use [Butane][link-3] to write human-friendly YAML configurations that then get transpiled into Ignition. So, when bootstrapping Kubernetes on Flatcar, users will generally use a Butane configuration that leverages `kubeadm`, as described [in the Flatcar documentation][link-5].

While the Butane configurations in the documentation are a good start for bootstrapping Kubernetes on Flatcar, they don't address the dual-stack use case. As outlined [in the Kubernetes documentation for dual-stack support with `kubeadm`][link-6], you need to tell `kubeadm` to have the Kubernetes nodes register themselves with _both_ the IPv4 and the IPv6 addresses, like this:

```yaml
nodeRegistration:
  kubeletExtraArgs:
  - name: "node-ip"
    value: "10.100.0.2,fd00:1:2:3::2"
```

You can also use this syntax:

```yaml
nodeRegistration:
  kubeletExtraArgs:
    node-ip: "10.100.0.2,fd00:1:2:3::2"
```

If you don't specifically instruct the Kubelet to use both addresses, it will register with only one of the addresses (typically the IPv4 address), and you won't have dual-stack support.

If you're in a situation where you are bootstrapping Kubernetes with `kubeadm` after the OS boots, then you're able to find out what IP addresses the instance is using and configure your `kubeadm` configuration file accordingly. In this case, however, we are running `kubeadm` immediately after boot (by supplying the Butane configuration as user data via [Terraform][link-9] with [the terraform-ct provider][link-10]). How are we going to know what addresses were picked up by the OS when it booted? How can we tell `kubeadm` which addresses to use? This is the first challenge I encountered when working out how to bootstrap a dual-stack cluster with Flatcar.

Fortunately, there is a solution: enter [Afterburn][link-7]. Afterburn dates from the CoreOS days but is supported for use on Flatcar (although some of the variable names change slightly; be sure to refer [to the Flatcar documentation][link-8] for the exact changes). Among other things, Afterburn enables users to get IP addresses in use by the system by using specific environment variables---perfect for this use case.

To use Afterburn, a few changes are needed to the Butane configuration that invokes `kubeadm`. Specifically:

1. The systemd unit for `kubeadm` needs to be dependent upon the Afterburn service (referenced as "coreos-metadata.service").
2. The `kubeadm` systemd unit needs to load the environment file created by Afterburn.
3. An additional line is needed to invoke `envsubst` to supply the dynamic values provided by Afterburn.

The first change involves modifying the `Requires=` and `After=` lines in the `[Unit]` section of the systemd unit definition, like this (this is the Butane configuration creating the `kubeadm` systemd unit):

```yaml
systemd:
  units:
    - name: kubeadm.service
      enabled: true
      contents: |
        [Unit]
        Description=Kubeadm service
        Requires=containerd.service coreos-metadata.service
        After=containerd.service coreos-metadata.service
```

The second change requires the addition of an `EnvironmentFile=` line at the beginning of the `[Service]` section. Building on the previous example, here's what that looks like:

```yaml
systemd:
  units:
    - name: kubeadm.service
      enabled: true
      contents: |
        [Unit]
        Description=Kubeadm service
        Requires=containerd.service coreos-metadata.service
        After=containerd.service coreos-metadata.service
        ConditionPathExists=!/etc/kubernetes/kubelet.conf
        [Service]
        EnvironmentFile=/run/metadata/flatcar
```

This sources the `/run/metadata/flatcar` file created by Afterburn. This simple text file contains environment variable definitions of various pieces of metadata gathered by Afterburn. Of particular interest in this case are the `COREOS_EC2_IPV4_LOCAL` and `COREOS_EC2_IPV6` variables---these contain the local IPv4 and IPv6 addresses in use. These environment variables are now available for use by any of the rest of the commands in the systemd unit.

The third and final change uses `envsubst` to replace placeholders in the `kubeadm` configuration with the dynamic values gathered by Afterburn. Before looking at that, let's examine the `kubeadm` configuration file being supplied by Butane:

```yaml
storage:
  files:
    - path: /etc/kubeadm.yml
      contents:
        inline: |
          ---
          apiVersion: kubeadm.k8s.io/v1beta3
          kind: InitConfiguration
          bootstrapTokens:
          - groups:
            - system:bootstrappers:kubeadm:default-node-token
            token: ${token}
            ttl: 24h0m0s
            usages:
            - signing
            - authentication
          certificateKey: ${certificate_key}
          nodeRegistration:
            kubeletExtraArgs:
              volume-plugin-dir: ${volume_plugin_dir}
              cloud-provider: ${cloud_provider}
              node-ip: "$COREOS_EC2_IPV4_LOCAL,$COREOS_EC2_IPV6"
          skipPhases:
          - addon/kube-proxy
          ---
          apiVersion: kubeadm.k8s.io/v1beta3
          kind: ClusterConfiguration
          apiServer:
            certSANs:
              - ${external_kube_apiserver_host}
              - ${internal_kube_apiserver_host}
            extraArgs:
              cloud-provider: ${cloud_provider}
            timeoutForControlPlane: 5m0s
          controlPlaneEndpoint: "${internal_kube_apiserver_host}:${internal_kube_apiserver_port}"
          controllerManager:
            extraArgs:
              flex-volume-plugin-dir: ${volume_plugin_dir}
              cloud-provider: ${cloud_provider}
          kubernetesVersion: ${version_bare}
          networking:
            podSubnet: ${pod_cidr}
            serviceSubnet: ${service_cidr}
          clusterName: ${cluster_name}
          ---
          apiVersion: kubelet.config.k8s.io/v1beta1
          kind: KubeletConfiguration
          cgroupDriver: systemd
```

Some values in this configuration file are being supplied by the terraform-ct provider (these are referenced as `${variable}`); they are templated in the Terraform configuration and variables are replaced. However, Terraform won't know what the IP addresses are going to be until after the resource is created, at which point it's too late. Hence, the `node-ip` line uses `$VARIABLE` syntax that will be picked up by `envsubst` and replaced with the dynamic values retrieved by Afterburn. So there are essentially _two_ templating passes that take place: one at the Terraform layer, and a second one at the systemd/Butane layer. Confused yet?

With this configuration in mind, the third change needed in the `kubeadm` systemd unit created by Butane has to invoke `envsubst`. This change looks like this:

```yaml
systemd:
  units:
    - name: kubeadm.service
      enabled: true
      contents: |
        [Unit]
        Description=Kubeadm service
        Requires=containerd.service coreos-metadata.service
        After=containerd.service coreos-metadata.service
        ConditionPathExists=!/etc/kubernetes/kubelet.conf
        [Service]
        EnvironmentFile=/run/metadata/flatcar
        Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/bin"
        ExecStartPre=/bin/bash -c 'envsubst < /etc/kubeadm.yml > /etc/kubeadm-final.yml'
```

The last line shown above is the key change. [Shell redirection and pipes aren't supported][link-11] directly in systemd unit files, so it's necessary to use `/bin/bash -c` (or `/bin/sh -c`) when invoking `envsubst` in order for it to work correctly. Making `envsubst` work properly in a systemd unit file was the second key challenge I ran into during this process.

Finally, the `kubeadm` systemd unit needs to invoke `kubeadm` itself to bootstrap the cluster. I did find at least one change from the Flatcar examples that was useful here, which is the inclusion of the `--upload-certs` flag. Here's the final systemd unit for `kubeadm` for the first control plane node:

```yaml
systemd:
  units:
    - name: kubeadm.service
      enabled: true
      contents: |
        [Unit]
        Description=Kubeadm service
        Requires=containerd.service coreos-metadata.service
        After=containerd.service coreos-metadata.service
        ConditionPathExists=!/etc/kubernetes/kubelet.conf
        [Service]
        EnvironmentFile=/run/metadata/flatcar
        Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/bin"
        ExecStartPre=/bin/bash -c 'envsubst < /etc/kubeadm.yml > /etc/kubeadm-final.yml'
        ExecStartPre=/opt/bin/kubeadm config images pull
        ExecStartPre=/opt/bin/kubeadm init --upload-certs --config /etc/kubeadm-final.yml
        ExecStartPre=/usr/bin/mkdir /home/${ssh_user}/.kube
        ExecStartPre=/usr/bin/cp /etc/kubernetes/admin.conf /home/${ssh_user}/.kube/config
        ExecStart=/usr/bin/chown -R ${ssh_user}:${ssh_user} /home/${ssh_user}/.kube
        [Install]
        WantedBy=multi-user.target
```

And here's the systemd unit for `kubeadm` for additional control plane nodes:

```yaml
systemd:
  units:
    - name: kubeadm.service
      enabled: true
      contents: |
        [Unit]
        Description=Kubeadm service
        Requires=containerd.service coreos-metadata.service
        After=containerd.service coreos-metadata.service
        ConditionPathExists=!/etc/kubernetes/kubelet.conf
        [Service]
        EnvironmentFile=/run/metadata/flatcar
        Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/bin"
        ExecStartPre=/bin/bash -c 'envsubst < /etc/kubeadm.yml > /etc/kubeadm-final.yml'
        ExecStartPre=/opt/bin/kubeadm config images pull
        ExecStartPre=sleep 10
        ExecStartPre=/opt/bin/kubeadm join --config /etc/kubeadm-final.yml
        ExecStartPre=/usr/bin/mkdir /home/${ssh_user}/.kube
        ExecStartPre=/usr/bin/cp /etc/kubernetes/admin.conf /home/${ssh_user}/.kube/config
        ExecStart=/usr/bin/chown -R ${ssh_user}:${ssh_user} /home/${ssh_user}/.kube
        [Install]
        WantedBy=multi-user.target
```

Finally, here's the systemd unit for `kubeadm` for any worker nodes joined to the cluster:

```yaml
systemd:
  units:
    - name: kubeadm.service
      enabled: true
      contents: |
        [Unit]
        Description=Kubeadm service
        Requires=containerd.service coreos-metadata.service
        After=containerd.service coreos-metadata.service
        ConditionPathExists=!/etc/kubernetes/kubelet.conf
        [Service]
        EnvironmentFile=/run/metadata/flatcar
        Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/bin"
        ExecStartPre=/bin/bash -c 'envsubst < /etc/kubeadm.yml > /etc/kubeadm-final.yml'
        ExecStartPre=/opt/bin/kubeadm config images pull
        ExecStart=/opt/bin/kubeadm join --config /etc/kubeadm-final.yml
        [Install]
        WantedBy=multi-user.target
```

The last step is supplying the full Butane configuration, including the `kubeadm` configuration file and the `kubeadm` systemd unit, as user data to the AWS instance when it boots. I did this in Terraform via [the `terraform-ct` provider][link-10], but really just about any method you want to use will work.

Whew! That was a lot---let's recap things:

1. Bootstrapping Kubernetes on Flatcar generally involves the use of a Butane configuration to perform certain tasks, create files, and define systemd unit when the system boots up. On AWS, you'll pass this Butane configuraiton to the EC2 instance(s) as user data.
2. The Butane configuration needs to define both a `kubeadm` configuration file and a `kubeadm` systemd unit.
3. The `kubeadm` configuration file needs to include placeholders for the Afterburn-defined environment variables `$COREOS_EC2_IPV4_LOCAL` and `$COREOS_EC2_IPV6` in the nodeRegistration portion of the InitConfiguration section.
4. The `kubeadm` systemd unit needs to be told to depend on Afterburn (aka "coreos-metadata.service"), needs to read in the environment file created by Afterburn, and needs to use `envsubst` (or whatever tool you choose) to replace the placeholders mentioned in step 3 with the actual values. This ensures that the Kubelet registers both an IPv4 _and_ and IPv6 address, making dual-stack support active.
5. Shell redirection doesn't work in systemd units, so you'll need to use `/bin/bash -c` or `/bin/sh -c` to run whatever tool is needed to replace the placeholders.

With all this in place, you should be able to stand up Flatcar Container Linux on EC2 instances and have them bootstrap into a Kubernetes cluster with dual-stack IPv4/IPv6 support.

I hope this proves useful to other folks out there. If you have any questions, feel free to find me either [on Mastodon][link-12] or [on Bluesky][link-13] (I don't really use Twitter/X any longer). You're also welcome to hit me up on [the Kubernetes Slack instance][link-14]; I'm in both the `#flatcar` and `#kubeadm` channels. I also welcome feedback or suggested improvements to the information provided in this post, so don't hesitate to reach out. Thanks for reading!

[link-1]: https://www.flatcar.org/
[link-2]: https://github.com/coreos/ignition
[link-3]: https://www.flatcar.org/docs/latest/provisioning/config-transpiler/
[link-4]: https://www.flatcar.org/docs/latest/provisioning/ignition/
[link-5]: https://www.flatcar.org/docs/latest/container-runtimes/getting-started-with-kubernetes/
[link-6]: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/dual-stack-support/#create-a-dual-stack-cluster
[link-7]: https://github.com/coreos/afterburn/
[link-8]: https://www.flatcar.org/docs/latest/provisioning/ignition/dynamic-data/
[link-9]: https://developer.hashicorp.com/terraform
[link-10]: https://registry.terraform.io/providers/poseidon/ct/latest
[link-11]: https://www.man7.org/linux/man-pages/man5/systemd.service.5.html#COMMAND_LINES
[link-12]: https://fosstodon.org/@scottslowe
[link-13]: https://bsky.app/profile/scottslowe.bsky.social
[link-14]: https://kubernetes.slack.com
