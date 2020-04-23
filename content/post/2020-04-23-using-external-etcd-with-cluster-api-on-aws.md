---
author: slowe
categories: Explanation
comments: true
date: 2020-04-23T10:25:00-07:00
tags:
- Kubernetes
- CAPI
- etcd
- AWS
- Pulumi
title: Using External Etcd with Cluster API on AWS
url: /2020/04/23/using-external-etcd-with-cluster-api-on-aws/
---

If you've used [Cluster API (CAPI)][link-8], you may have noticed that workload clusters created by CAPI use, by default, a "stacked master" configuration---that is, the etcd cluster is running co-located on the control plane node(s) alongside the Kubernetes control plane components. This is a very common configuration and is well-suited for most deployments, so it makes perfect sense that this is the default. There may be cases, however, where you'll want to use a dedicated, external etcd cluster for your Kubernetes clusters. In this post, I'll show you how to use an external etcd cluster with CAPI on AWS.<!--more-->

The information in this blog post is based on [this upstream document][link-9]. I'll be adding a little bit of AWS-specific information, since I primarily use the AWS provider for CAPI. This post is written with CAPI v1alpha3 in mind.

The key to this solution is building upon the fact that CAPI leverages `kubeadm` for bootstrapping cluster nodes. This puts the full power of the `kubeadm` API at your fingertips---which in turn means you have a great deal of flexibility. This is the mechanism whereby you can tell CAPI to use an external etcd cluster instead of creating a co-located etcd cluster.

I should note that I am referring to using CAPI to create a _workload cluster_ with an external etcd environment. If you're not familiar with some of the CAPI terminology, check out [my introductory post][xref-1].

At a high level, the steps involved are:

1. Create the required Secrets in the management cluster.
2. Modify the CAPI manifests to reference the external etcd cluster.
3. Profit!

Let's take a look at these two steps in more detail. (I'll omit step 3.) Note that I'm not going to cover the process of establishing the etcd cluster, as that's something I've covered sufficiently elsewhere (like [here][xref-2], [here][xref-3], or [here][xref-4]).

## Creating the Required Secrets

When bootstrapping a typical workload cluster with a stacked master configuration, `kubeadm` typically generates all the necessary public key infrastructure (certificate authority and associated certificates). In the case of an external etcd cluster, though, some of these certificates already exist, and you need a mechanism to provide those to CAPI.

[This page][link-1] gave me the first hint on how it should be handled, and [this upstream document][link-9] finishes it out. Using Kubernetes Secrets, you can provide the certificates necessary to CAPI.

To create the required Secrets, you'll need four files from the etcd cluster:

1. The etcd certificate authority (CA) certificate
2. The etcd CA's private key
3. The API server etcd client certificate
4. The API server etcd client private key

Files #1 and #2 are, in the event of a typical `kubeadm`-bootstrapped etcd cluster, found at `/etc/kubernetes/pki/etcd/ca.{crt,key}`. Files #3 and #4 are typically found at `/etc/kubernetes/pki/apiserver-etcd-client.{crt,key}`. (Keep in mind these paths may change depending on how the etcd cluster was created.)

Once you have this files, create the first Secret using this command:

    kubectl create secret tls <cluster-name>-apiserver-etcd-client \
    --cert /path/to/apiserver-etcd-client.crt \
    --key /path/to/apiserver-etcd-client.key

Next, create the second Secret with this command:

    kubectl create secret tls <cluster-name>-etcd \
    --cert /path/to/etcd/ca.crt --key /path/to/etcd/ca.key

In both of these commands, you'll need to replace `<cluster-name>` with the name of the workload cluster you're going to create with CAPI.

If you are going to create your workload cluster in a specific namespace on the management cluster, you'll want to be sure you create these Secrets in the same namespace.

Once the Secrets are in place, the next step is to modify the CAPI manifests.

## Updating the CAPI Manifests

The first change is configuring the CAPI manifests to use an external etcd cluster. CAPI v1alpha3 introduces the KubeadmControlPlane object, and the KubeadmControlPlane object has a KubeadmConfigSpec that contains the equivalent of a valid `kubeadm` configuration file. This is the section you'll need to modify to instruct CAPI to create a workload cluster with an external etcd cluster.

Here's a snippet (unrelated entries have been removed for the sake of brevity) of YAML to add to the KubeadmControlPlane object in order to use an external etcd cluster:

```yaml
spec:
  kubeadmConfigSpec:
    clusterConfiguration:
      etcd:
        external:
          endpoints:
            - https://ip-10-11-12-13.us-west-2.compute.internal:2379
            - https://ip-10-11-14-15.us-west-2.compute.internal:2379
            - https://ip-10-11-16-17.us-west-2.compute.internal:2379
          caFile: /etc/kubernetes/pki/etcd/ca.crt
          certFile: /etc/kubernetes/pki/apiserver-etcd-client.crt
          keyFile: /etc/kubernetes/pki/apiserver-etcd-client.key
```

Obviously, you'd want to use the correct hostnames for the nodes in the external etcd cluster (the names above have been randomized). The certificate files referenced here will be created by CAPI using the Secrets created in the previous section.

The second change is to instruct CAPI to use the existing VPC where the etcd cluster resides. (Technically, this isn't required, but this sidesteps the need for VPC peering and similar configurations). For that, you can refer to [the upstream documentation for using existing AWS infrastructure][link-3], which indicates you need to add the following to your CAPI workload manifest (this is in the spec field for the AWSCluster object):

```yaml
spec:
  networkSpec:
    vpc:
      id: vpc-0123456789abcdef0
```

You will want to ensure that your existing AWS infrastructure is appropriately configured for use with CAPI; again, refer to [the upstream documentation][link-3] for full details.

The third and final step is to ensure the new CAPI workload cluster is able to communicate with the existing external etcd cluster. On AWS, that means configuring security groups appropriately to ensure access to the etcd cluster. As I explained in [this post on using existing AWS security groups with CAPI][xref-5], you'll want to configure your CAPI manifest to reference the existing AWS security group that permits access to your etcd cluster. That would look something like this in the AWSMachineTemplate that's referenced by the KubeadmControlPlane object:

```yaml
spec:
  template:
    spec:
      additionalSecurityGroups:
        - id: <etcd_security_group_id>
        - id: <any_other_needed_security_group_id>
```

Once you've modified the manifests for the CAPI workload cluster---adding the etcd section to the KubeadmConfigSpec, specifying the existing VPC, and adding any necessary security group IDs in the AWSMachineTemplate for the KubeadmControlPlane object---then you can create the CAPI cluster by applying the modified manifest with `kubectl apply -f manifest.yaml` when your `kubectl` context is set to your CAPI management cluster.

Everything _should_ work fine, but in the event you run into issues be sure to check the CAPI and CAPI provider logs (using `kubectl logs`) in the management cluster for entries that will point you in the right direction to help resolve the problem.

## Caveats/Disclaimer

Although this is a supported configuration with CAPI, you should be aware that you are giving up etcd management/upgrades via CAPI with this arrangement. It will fall on you, the cluster operator, to manage etcd upgrades and the lifecycle of the etcd cluster. Because you're also leveraging existing AWS infrastructure, you're also giving up CAPI management of that infrastructure. These may be acceptable trade-offs for your specific use case, but be sure to take that into consideration.

## How I Tested

To test this procedure, I used [Pulumi][link-4] to create some existing AWS infrastructure, in according with [the upstream project's guidelines for using existing AWS infrastructure][link-3]. In that existing infrastructure, the Pulumi code created three instances that I then set up as an etcd cluster using `kubeadm` (using a variation of [these instructions][link-5]). Once the external etcd cluster had been established, then I proceeded with testing the configuration and procedure outlined above.

Please don't hesitate to contact me if you have any questions, or if you spot an error in this post. All constructive feedback is welcome! You can contact [me on Twitter][link-6], or feel free to contact me via [the Kubernetes Slack instance][link-7].

[link-1]: https://cluster-api.sigs.k8s.io/tasks/certs/using-custom-certificates.html
[link-3]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/master/docs/existing-aws-infrastructure.md
[link-4]: https://www.pulumi.com/
[link-5]: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/setup-ha-etcd-with-kubeadm/
[link-6]: https://twitter.com/scott_lowe
[link-7]: https://kubernetes.slack.com
[link-8]: https://cluster-api.sigs.k8s.io/
[link-9]: https://github.com/kubernetes-sigs/cluster-api/blob/master/bootstrap/kubeadm/docs/external-etcd.md
[xref-1]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
[xref-2]: {{< relref "2018-08-21-bootstrapping-etcd-cluster-with-tls-using-kubeadm.md" >}}
[xref-3]: {{< relref "2018-10-29-more-on-setting-up-etcd-with-kubeadm.md" >}}
[xref-4]: {{< relref "2020-04-02-setting-up-etcd-with-kubeadm-containerd-edition.md" >}}
[xref-5]: {{< relref "2020-04-22-using-existing-aws-security-groups-with-cluster-api.md" >}}
