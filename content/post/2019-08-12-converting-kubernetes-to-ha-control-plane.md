---
author: slowe
categories: Tutorial
comments: true
date: 2019-08-12T12:00:00Z
tags:
- Kubernetes
- CLI
- Linux
title: Converting Kubernetes to an HA Control Plane
url: /2019/08/12/converting-kubernetes-to-ha-control-plane/
---

While hanging out in the Kubernetes Slack community, one question I've seen asked multiple times involves switching a Kubernetes cluster from a non-HA control plane (single control plane node) to an HA control plane (multiple control plane nodes). As far as I am aware, this isn't documented upstream, so I thought I'd walk readers through what this process looks like.<!--more-->

I'm making the following assumptions:

* The existing single control plane node was bootstrapped using `kubeadm`. (This means we'll use `kubeadm` to add the additional control plane nodes.)
* The existing single control plane node is using a "stacked configuration," in which both etcd and the Kubernetes control plane components are running on the same nodes.

I'd also like to point out that there are a _lot_ of different configurations and variables that come into play with a process like this. It's (nearly) impossible to cover them all in a single blog post, so this post attempts to address what I believe to be the most common situations.

With those assumptions and that caveat in mind, the high-level overview of the process looks like this:

1. Create a load balancer for the control plane.
2. Update the API server's certificate.
3. Update the kubelet on the existing control plane.
4. Update other control plane components.
5. Update worker nodes.
6. Add control plane nodes.

The following sections will explore each of these steps in a bit more detail. First, though, a disclaimer.

## Disclaimer

In all reality, the _best_ way to "upgrade" a non-HA control plane to an HA control plane is to build a new HA control plane and migrate all your workloads (perhaps using something like [Velero][link-3]). This better aligns with "cluster as cattle" thinking (more appropriately known as _immutable infrastructure_). With that in mind, I'm presenting the following information more as a means of learning a bit more about how Kubernetes works. I _do not_ recommend using this procedure on any cluster that will ever be considered a production cluster.

## Create a Load Balancer for the Control Plane

The first step is to create a load balancer for the control plane (a load balancer is required when using multiple control plane nodes). Since the specifics of how to set up and configure a load balancer will vary from solution to solution, I won't try to include the details here other than to mention two high-level requirements:

* You should be using a Layer 4 load balancer (TCP instead of HTTP/HTTPS).
* Health checks should be configured as SSL health checks instead of TCP health checks (this will weed out spurious "TLS handshake errors" in the API server's logs).

It is also a good idea at this time to create a DNS CNAME entry to point to your load balancer (highly recommended). This gives you some additional flexibility in the event you need to swap out or reconfigure your load balancing solution, as the DNS CNAME remains constant even if the names to which it resolves change behind the scenes. (The name remaining constant is important, as you'll see in a moment.)

Whether or not you create a DNS CNAME, make a note of the IP address(es) and DNS names you'll use to connect to the cluster through the load balancer, as you'll need those in the next step.

Because everything in the cluster still points directly to the single control plane node, adding in the load balancer won't affect anything _unless_ the load balancer is inline (requiring traffic to go through the load balancer to reach the single control plane node). At this point, I'd recommend sticking with a solution that still allows the rest of the cluster to reach the single control plane node directly.

## Update the API Server's Certificate

The next step is to update the API server's TLS certificate to account for the IP address(es) and/or DNS names that will be used to reach the control plane through the load balancer (see, I said you'd need them in the next step!). The API server (one of the three Kubernetes control plane components, the other two being the controller manager and the scheduler) uses a TLS certificate to both provide authentication as well as to encrypt control plane traffic. This certificate needs to have a proper Subject Alternative Name (SAN) that matches whatever IP address or DNS name is being used to communicate with the API server. Otherwise, communications with the API server will result in an error, and that (ultimately) break your cluster.

To address this, you'll need to add one or more new SANs to the API server's certificate. I wrote about this process in [this blog post][xref-1], so follow the instructions in that post to update your API server certificate.

Once you've completed this process, I recommend you update your Kubeconfig file (as outlined in the "Verifying the Change" section of the blog post for updating your certificate) to point to an IP address or DNS name that will direct the traffic through the load balancer, and then test `kubectl` access to your cluster. Even though the rest of the cluster is still pointing directly to the single control plane node, using `kubectl` through the load balancer should still work as expected. If it doesn't work, stop and troubleshoot the issue **before proceeding.** You'll want to be sure that access to the API server through the load balancer is working before you continue.

## Update the Kubelet on the Control Plane Node

The Kubelet on the existing control plane node communicates with the API server, as do all the other components of the cluster. Once you've verified that access to the API server through the load balancer works, the next step is to update the Kubelet to access the API server through the load balancer as well.

Much like a user does, the Kubelet uses a Kubeconfig file to know how to find the control plane and authenticate to it. This file is found in `/etc/kubernetes/kubelet.conf`, which you can verify by looking at the systemd drop-in added to the Kubelet service by `kubeadm` (the file is `/etc/systemd/system/kubelet.service.d/10-kubeadm.conf`).

In order to have the Kubelet communicate with the API server through the load balancer, you'll need to edit this Kubeconfig file for the Kubelet (again, the file in question is `/etc/kubernetes/kubelet.conf`) and change the `server:` line to point to an IP address or DNS name for the load balancer (and for which there is a corresponding SAN on the API server's certificate). Note you can also use `export KUBECONFIG=/etc/kubernetes/kubelet.conf` and `kubectl config set clusters.default-cluster.server` to make this change. (Personally, I find editing the file easier.)

Once you made this change, restart the Kubelet with `systemctl restart kubelet`, and then check the logs for the Kubelet to be sure it is working as expected.

## Update Control Plane Components

The fourth step is to update the other control plane components to communicate with the API server through the load balancer. Like the Kubelet, both the controller manager and the scheduler (two other components of the Kubernetes control plane along with the API server) use Kubeconfig files to communicate with and authenticate to the API server. Just as you updated the Kubeconfig file (by modifying the `server:` line for the cluster being modified to point to the load balancer) used by the Kubelet, you'll also need to update the Kubeconfig files that the controller manager and scheduler use to connect to the API server.

The files that need to be modified are:

    /etc/kubernetes/controller-manager.conf
    /etc/kubernetes/scheduler.conf

These files are standard Kubeconfig files. The only line that needs to be changed is the `server:` line that specifies the API endpoint (this is currently probably pointing to the IP address or hostname of the single control plane node). Edit each of these files to point to an IP address or DNS name for the load balancer (and for which a SAN exists on the API server certificate).

For these components to pick up the change, you'll need to restart them. Assuming that Docker is the container runtime in use, these commands will kill the container for each component:

    docker kill $(docker ps | grep kube-controller-manager | \
    grep -v pause | cut -d' ' -f1)
    docker kill $(docker ps | grep kube-scheduler | grep -v pause | \
    cut -d' ' -f1)

If you're using containerd as your container runtime, the commands will look a bit different. Use `crictl pods | grep kube-controller-manager | cut -d' ' -f1` to get the Pod ID. Then use `crictl stopp <pod-id>` to stop the Pod, and `crictl rmp <pod-id>` to remove the Pod. Repeat as needed for other control plane components.

The Kubelet will then restart them automatically, and they'll pull in the changes to their respective Kubeconfig files. Once the controller manager and scheduler have restarted, check the logs for each (using either `docker logs <containerID>` or `kubectl logs -n kube-system <podName>`) to make sure that they are operating correctly.

Before moving on to the worker nodes, there's one more component on the control plane node that needs to be updated: `kube-proxy`.

## Update Kube-Proxy on the Control Plane Node

With most CNI plugins, `kube-proxy` is responsible for implementing the necessary mechanisms to support Services and Network Policies. Like the Kubelet and the other control plane components, `kube-proxy` uses a Kubeconfig file to specify how to connect to the Kubernetes API. In this case, however, the config file is provided to `kube-proxy` via a ConfigMap.

To update this ConfigMap, use `kubectl -n kube-system edit cm kube-proxy` and look for the `server:` line that specifies how to connect to the Kubernetes API. It probably refers to the original control plane node's IP address, or possibly the control plane node's DNS name. You'll need to edit it to point to the load balancer you created for the control plane. Then restart the `kube-proxy` Pod on the control plane node using the instructions provided above.

At this point, you've created and configured a load balancer for the control plane, updated the API server's certificate to account for the load balancer, updated the Kubelet to point to the load balancer, updated the controller manager and scheduler to point to the load balancer, and updated `kube-proxy` to point to the load balancer. You're now ready to move on to the worker nodes.

## Update Worker Nodes

The only component running on the worker nodes that needs to be updated is the Kubelet configuration. Follow the instructions in the section "Update the Kubelet on the Control Plane Node" to update the Kubelet on the worker nodes as well.

Next, restart the `kube-proxy` Pod running on the node so that it picks up the updated ConfigMap you edited in the previous section. Use the `docker` or `crictl` commands provided earlier.

## Update the In-Cluster Configuration

As pointed out in [this post][xref-1], `kubeadm` stores some configuration in a ConfigMap named "kubeadm-config" in the "kube-system" namespace. `kubeadm` uses this information when performing cluster upgrades, or when joining nodes to the cluster. Since we've now added a load balancer in front of the control plane, we need to update this ConfigMap with the correct information. (You'll be adding control plane nodes to the cluster shortly, so having the correct information in this ConfigMap is important.)

First, pull the current configuration from the ConfigMap with this command:

    kubectl -n kube-system get configmap kubeadm-config -o jsonpath='{.data.ClusterConfiguration}' > kubeadm.yaml

Currently, the `controlPlaneEndpoint` is most likely empty (just a pair of double quotes). Edit this value to point to the DNS CNAME of the load balancer you created and configured for the control plane. (As I mentioned earlier, the use of a DNS CNAME for the control plane load balancer is recommended.)

You should also be able to see the `certSANs` section that was added as part of adding a SAN to the API server's certificate (assuming you followed the directions to update the ConfigMap when updating the API server certificate).

Once you've edited the file, upload it back to the cluster with this command:

    kubeadm config upload from-file --config kubeadm.yaml

You also need to update the "cluster-info" ConfigMap in the "kube-public" namespace, which contains a Kubeconfig file with a `server:` line that points to the single control plane node. It's easist to just use `kubectl -n kube-public edit cm cluster-info` and update the `server:` line to point to the load balancer for the control plane. _(Thanks to Fabrizio Pandini for pointing this out!)_

Only one step remains now: actually adding more control plane nodes to make the control plane highly available.

## Add Control Plane Nodes

To add additional control plane nodes---and you should be adding two additional nodes, for a total of three, which allows etcd to reach quorum---you can follow the instructions from the Kubernetes web site (see [here][link-1] for version 1.15 or [here][link-2] for version 1.14). In particular, see the "Steps for the rest of the control plane nodes" under the "Stacked control plane and etcd nodes" section.

There are a few caveats/considerations to keep in mind:

* These instructions assume you are joining the additional control plane nodes immediately or nearly immediately after creating the control plane. In this case, however, the control plane may have been up for quite a while. Therefore, you're going to need to upload the certificates again (and generate a new certificate key) using `kubeadm init phase upload-certs --upload-certs` (for 1.15) or `kubeadm init phase upload-certs --experimental-upload-certs` (for 1.14). This will generate a new certificate key, which you'll need (it's only good for 2 hours).
* For the same reason as above, you'll probably also need to generate a new bootstrap token (the default lifetime of a token is 24 hours). You can do this with `kubeadm token create`.
* Finally, you may not know the SHA256 hash of the CA certificate. Fortunately, I have this covered for you as well; see [this post][xref-2] for instructions.

Once you have a valid certificate key, a valid bootstrap token, and the correct SHA256 hash of the CA certificate, you can join a new control plane node with this command:

    kubeadm join <DNS CNAME of load balancer>:6443 \
    --token <bootstrap-token> \
    --discovery-token-ca-cert-hash sha256:<CA certificate hash> \
    --control-plane --certificate-key <certificate-key>

(If you're using 1.14, replace `--control-plane` with `--experimental-control-plane`.)

Once you've added two more control plane nodes (for a total of three), you now have a highly available Kubernetes control plane. Go pat yourself on the back.

**Reminder:** I _do not_ recommend using this procedure for any sort of cluster that is or ever will be considered "production." See the "Disclaimer" section above.

If you have any questions, comments about the post, or corrections/suggestions, please don't hesitate to [contact me on Twitter][link-4]. Or, you can find me on the Kubernetes Slack community (I hang out a lot in the "#kubeadm" channel). I hope this information is useful to you!

**UPDATE 2020-02-20:** I added notes about updating the `kube-proxy` configuration, and added commands for restarting Pods on containerd-based nodes.

[link-1]: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/
[link-2]: https://v1-14.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/
[link-3]: https://velero.io/
[link-4]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-07-30-adding-a-name-to-kubernetes-api-server-certificate.md" >}}
[xref-2]: {{< relref "2019-07-12-calculating-ca-certificate-hash-for-kubeadm.md" >}}
