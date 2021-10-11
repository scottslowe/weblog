---
author: slowe
categories: Explanation
comments: true
date: 2019-11-12T19:57:00-07:00
tags:
- CAPI
- CLI
- Kubernetes
- Kustomize
- YAML
title: Using Kustomize with Cluster API Manifests
url: /2019/11/12/using-kustomize-with-cluster-api-manifests/
---

A topic that's been in the back of my mind since writing [the Cluster API introduction post][xref-1] is how someone could use [`kustomize`][link-2] to modify the [Cluster API][link-3] manifests. Fortunately, this is reasonably straightforward. It doesn't require any "hacks" like those needed to [use `kustomize` with `kubeadm` configuration files][xref-2], but similar to modifying `kubeadm` configuration files you'll generally need to use the patching functionality of `kustomize` when working with Cluster API manifests. In this post, I'd like to take a fairly detailed look at how someone might go about using `kustomize` with Cluster API.<!--more-->

By the way, readers who are unfamiliar with `kustomize` should probably read [this introductory post][xref-3] first, and then read the post on [using `kustomize` with `kubeadm` configuration files][xref-2]. I suggest reading the latter post because it provides an overview of how to use `kustomize` to patch a specific portion of a manifest, and you'll use that functionality again when modifying Cluster API manifests.

## A Fictional Use Case

For this post, I'm going to build out a fictional use case/scenario for the use of `kustomize` and Cluster API. Here are the key points to this fictional use case:

1. Three different clusters on AWS are needed. The management cluster already exists.
2. Two of these clusters will run in the AWS "us-west-2" region, while the third will run in the "us-east-2" region.
3. One of the two "us-west-2" clusters will use larger instance types to accommodate more resource-intensive workloads.
4. All three clusters need to be highly available, with multiple control plane nodes.

With this fictional use case in place, you're now ready to set up a directory structure to support using Cluster API with `kustomize` to satisfy this use case.

## Setting up the Directory Structure

To accommodate this fictional use case, you'll need to use a directory structure that supports the use of `kustomize` overlays. Therefore, I'd propose a directory structure that looks something like this:

``` text
(parent)
 |- base
 |- overlays
     |- usw2-cluster1
     |- usw2-cluster2
     |- use2-cluster1
```

The `base` directory will store the "starting" point for the final Cluster API manifests, as well as a `kustomization.yaml` file that identifies these Cluster API manifests as resources for `kustomize` to use.

Each of the overlay subdirectories will also have a `kustomization.yaml` file and various patch files that will be applied against the base resources to produce the final manifests.

## Creating the Base Configuration

The base configuration (found in the `base` directory of the directory structure described above) will contain complete, but fairly generic, configurations for Cluster API:

* Definitions of the Cluster and AWSCluster objects
* Definitions of the Machine and AWSMachine objects for the control plane along with associated KubeadmConfig objects
* Definitions of the Machine, AWSMachine, and KubeadmConfig (or MachineDeployment, AWSMachineTemplate, and KubeadmConfigTemplate) objects for the worker nodes

To make your job easier with the `kustomize` overlays, modifying the base configurations to accommodate the majority of your deploys will mean fewer patches needed by `kustomize` later. In this fictional scenario, two of the clusters will run in "us-west-2", so you should specify "us-west-2" as the region in the base configuration. Similarly, if you were planning on using the same SSH key for all the clusters (not recommended), you could bake that setting into the base configuration.

One final piece is needed, and that is a `kustomization.yaml` file in the base directory that identifies the resources available to `kustomize`. Assuming that your files were named `cluster.yaml` (for the Cluster and AWSCluster objects), `controlplane.yaml` (for objects related to the control plane), and `workers.yaml` (for objects related to worker nodes), then your `kustomization.yaml` might look like this:

```yaml
---
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - cluster.yaml
  - controlplane.yaml
  - workers.yaml
```

With the base configuration done, you're now ready to move on to the overlays.

## Creating the Overlays

The overlays are where things start to get interesting. Each cluster will get its own directory in the `overlays` directory, where you'll provide the cluster-specific patches `kustomize` will use to generate the YAML for that particular cluster.

Let's start with the "usw2-cluster1" overlay. To understand what will be needed, you must first understand what changes need to be made to the base configuration to produce the desired configuration for this particular cluster. So what changes needed to be made?

1. The `metadata.name` for the Cluster and AWSCluster objects needs to be modified. To keep the link between the Cluster and AWSCluster objects, the `spec.infrastructureRef.name` field for the Cluster object needs to be modified to use the correct value pointing to the AWSCluster object.
2. The `spec.sshKeyName` field of the AWSCluster object needs to have the correct SSH key name specified.
3. Similarly, the `metadata.name` field for the Machine objects, the AWSMachine objects, and the KubeadmConfig objects also need to be modified to use the correct names for the control plane objects and worker node objects. Since the `metadata.name` field is being modified for the AWSMachine and KubeadmConfig objects, you'll also need to update the `spec.infrastructureRef.name` and `spec.bootstrap.configRef.name` fields of the Machine object, respectively, with the correct values.
4. If you're instead using a MachineDeployment for the worker nodes, the `metadata.name` fields of the MachineDeployment, AWSMachineTemplate, and KubeadmConfigTemplate objects needs to be updated. As with the previous bullet, the references in `spec.template.spec.bootstrap.configRef.name` and `spec.template.spec.infrastructureRef.name` fields need to be updated for the MachineDeployment object. Finally, the `spec.template.spec.sshKeyName` field needs to be updated for the AWSMachineTemplate object, so that the correct SSH key is used.
5. All labels referencing the cluster name (such as the labels assigned to any Machine objects, assigned to any MachineDeployment objects, or referenced in the template of any MachineDeployment objects) need to be updated to refer to the correct cluster name. This would also include labels in the `spec.selector.matchLabels` field of a MachineDeployment.

Now that you have an idea of what changes need to be made to a set of Cluster API manifests, let's explore _how_ we might go about making those changes with `kustomize`. I won't go over all the changes, but rather illustrate a couple of different ways these changes could be implemented.

### Using JSON Patches

One way of patching individual fields within a manifest is using JSON 6902 patches (so named because they are described in [RFC 6902][link-4]). As an example, I will explore using JSON 6902 patches to address #1 from the list of changes described above.

The first part of a JSON 6902 patch is the reference to the patch file itself that must be placed in the `kustomization.yaml` file:

```yaml
patchesJson6902:
  - target:
      group: cluster.x-k8s.io
      version: v1alpha2
      kind: Cluster
      name: capi-quickstart
    path: cluster-patch.json
```

This tells `kustomize` where the patch file is, and against which object(s) the patch file should be applied. Since I am using the manifests from the CAPI Quick Start as the base configuration, you can see the patch is specified to operate against the Cluster object named "capi-quickstart".

The second part is the patch itself, which can be formatted as either YAML or JSON. I'll use JSON in this example, but [this section of the `kubectl` book][link-5] provides an example of a YAML-formatted patch.

Here's a JSON 6902 patch encoded as JSON:

```json
[
  { "op": "replace",
    "path": "/metadata/name",
    "value": "usw2-cluster-1" },
  { "op": "replace",
    "path": "/spec/infrastructureRef/name",
    "value": "usw2-cluster-1" }
]
```

<small><em>(This example, as well as other examples in this post, are wrapped for readability; it is perfectly acceptable to have each operation formatted as a single line.)</em></small>

In this example, the patches are provided in a JSON list (denoted by the brackets), and each patch is a JSON object with three properties: `op`, `path`, and `value`. (Readers who are unfamiliar with JSON may find [this post][xref-4] helpful.) This patch makes two changes to the original manifest. First, it modifies the `metadata.name` field to use "usw2-cluster1" as the value. Second, it modifies the `spec.infrastructureRef.name` field to also use "usw2-cluster1" as the value.

This patch addresses the Cluster object, but you also need to address the AWSCluster object. For that, you'll need a separate patch file referenced by a separate section in `kustomization.yaml`.

The reference in `kustomization.yaml` would look like this:

```yaml
patchesJson6902:
  - target:
      group: infrastructure.cluster.x-k8s.io
      version: v1alpha2
      kind: AWSCluster
      name: capi-quickstart
    path: awscluster-patch.json
```

And the corresponding patch file would look like this:

```json
[
  { "op": "replace",
    "path": "/metadata/name",
    "value": "usw2-cluster-1" }
]
```

Note that I haven't mentioned that the `kustomization.yaml` file in this directory also needs to have a reference to the base configuration; I'm only discussing the patch configuration. Refer to the `kustomize` documentation for full details, or refer back to [my introductory post on `kustomize`][xref-3].

Assuming a properly-configured `kustomization.yaml` file in this overlay directory referencing these two JSON 6902 patches, running `kustomize build .` would generate a customized set of manifests where the Cluster and AWSCluster objects have values specific for this particular workload cluster.

You can replicate this approach to make some of the other changes listed above, but in some cases using a JSON 6902 patch may not be the most effective method (this is especially true when a number of different fields are being modified).

### Using Strategic Merge Patches

Instead of using a JSON 6902 patch, the other alternative is to use a strategic merge patch. This allows you to easily modify a number of different fields in a single manifest by "overriding" the values that are already present (if any).

As with a JSON 6902 patch, the first part of a strategic merge patch involves adding a reference to the overlay's `kustomization.yaml` file:

```yaml
patches:
  - target:
      group: cluster.x-k8s.io
      version: v1alpha2
      kind: Machine
      name: .*
    path: machine-labels.yaml
```

This is very much like the reference shown earlier to a JSON 6902 patch, but in this case I'll draw your attention to the fact this uses a regular expression (regex) for the `name` field. This allows you to create a patch that will apply to _multiple_ objects (as long as the objects match the group, version, and kind selectors). In this particular example, we're referencing a patch that should apply to all Machine objects.

The second part is the patch itself, which is now a YAML file that contains the values to override in the base configuration as well as any additional values that should be added to the base configuration. In this example, I'll only modify an existing value.

Here's the contents of the patch file referenced above:

```yaml
---
apiVersion: cluster.x-k8s.io/v1alpha2
kind: Machine
metadata:
  name: .*
  labels:
    cluster.x-k8s.io/cluster-name: "usw2-cluster1"
```

Here again you see the use of a regex to capture all Machine objects regardless of name, and then a value for `labels` that will overwrite (in this case) the existing value in the base configuration. If you wanted to add additional labels, you could simply specify the additional labels right here in the patch. `kustomize` would then handle replacing existing values and adding new values.

Running `kustomize build .` with these changes present would result in all Machine objects being modified to use the label specified above, which is part of change #5 listed above (note that we haven't addressed changes affecting the use of a MachineDeployment, only individual Machine objects).

This example, however, doesn't really illustrate the difference between a JSON 6902 patch and a strategic patch merge. I'll use another example that addresses the rest of change #5 by modifying a MachineDeployment's labels.

For this final example, you'd again need both a reference to the patch file in `kustomization.yaml` as well as the patch file itself. I won't repeat the entry in `kustomization.yaml` as you've seen a couple of times already; it would look a lot like the one for modifying Machine objects, but pointing to MachineDeployment objects instead.

The actual patch better illustrates how you can make multiple changes to a base manifest with a single patch file:

```yaml
---
apiVersion: MachineDeployment
kind: MachineDeployment
metadata:
  name: .*
  labels:
    cluster.x-k8s.io/cluster-name: "usw2-cluster1"
spec:
  selector:
    matchLabels:
        cluster.x-k8s.io/cluster-name: "usw2-cluster1"
  template:
    metadata:
      labels:
        cluster.x-k8s.io/cluster-name: "usw2-cluster1"
```

Here a single patch file is making _three_ separate (but related) changes to MachineDeployment resources in the base manifests. In this case, replicating this functionality with a JSON 6902 patch wouldn't be terribly difficult, but users may find the readability of this approach to make it easier to reason about what `kustomize` is doing as it generates manifests.

There are a number of other changes that would be necessary to fully implement the fictional scenario, but in the interest of (reasonable) brevity I won't include or describe all the necessary changes in this post. See the next section for information on where you can see an example of all the changes needed to implement the fictional scenario described in this post.

## Additional Resources

In the event you'd like to use the fictional scenario described here to help with your own learning, I've created this exact scenario in [my GitHub "learning-tools" repository][link-1], found in the `kubernetes/capi-kustomize` directory of the repository. There you'll find example YAML files, overlays, JSON patches, and related materials---all based on the fictional scenario described in this post---for you to use in your own experiments in combining Cluster API with `kustomize`.

If you have any questions, corrections (in the event I've made an error), or suggestions for improvement, please don't hesitate to [contact me on Twitter][link-99].

[link-1]: https://github.com/scottslowe/learning-tools
[link-2]: https://kustomize.io/
[link-3]: https://github.com/kubernetes-sigs/cluster-api
[link-4]: https://tools.ietf.org/html/rfc6902
[link-5]: https://kubectl.docs.kubernetes.io/pages/reference/kustomize.html#patchesjson6902
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
[xref-2]: {{< relref "2019-10-16-using-kustomize-with-kubeadm-configuration-files.md" >}}
[xref-3]: {{< relref "2019-09-13-an-introduction-to-kustomize.md" >}}
[xref-4]: {{< relref "2013-11-08-a-non-programmers-introduction-to-json.md" >}}
