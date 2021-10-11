---
author: slowe
categories: Explanation
comments: true
date: 2019-09-13T12:00:00Z
tags:
- CLI
- Kubernetes
- Kustomize
- YAML
title: An Introduction to Kustomize
url: /2019/09/13/an-introduction-to-kustomize/
---

[`kustomize`][link-3] is a tool designed to let users "customize raw, template-free YAML files for multiple purposes, leaving the original YAML untouched and usable as is" (wording taken directly from [the `kustomize` GitHub repository][link-8]). Users can run `kustomize` directly, or---starting with [Kubernetes][link-9] 1.14---use `kubectl -k` to access the functionality (although the standalone binary is newer than the functionality built into `kubectl` as of the Kubernetes 1.15 release). In this post, I'd like to provide an introduction to `kustomize`.<!--more-->

In its simplest form/usage, `kustomize` is simply a set of resources (these would be YAML files that define Kubernetes objects like Deployments, Services, etc.) plus a set of instructions on the changes to be made to these resources. Similar to the way `make` leverages a file named `Makefile` to define its function or the way Docker uses a `Dockerfile` to build a container, `kustomize` uses a file named `kustomization.yaml` to store the instructions on the changes the user wants made to a set of resources.

Here's a simple `kustomization.yaml` file:

```yaml
resources:
- deployment.yaml
- service.yaml
namePrefix: dev-
namespace: development
commonLabels:
  environment: development
```

This article won't attempt to explain _all_ the various fields that could be present in a `kustomization.yaml` file (that's well handled [here][link-10]), but here's a quick explanation of this particular example:

* The `resources` field specifies which things (resources) `kustomize` will modify. In this case, it will look for resources inside the `deployment.yaml` and `service.yaml` files in the same directory (full or relative paths can be specified as needed here).
* The `namePrefix` field instructs `kustomize` to prefix the name attribute of all resources defined in the `resources` field with the specified value (in this case, "dev-"). So, if the Deployment specified a name of "nginx-deployment", then `kustomize` would change the value to "dev-nginx-deployment".
* The `namespace` field instructs `kustomize` to add a namespace value to all resources. In this case, the Deployment and the Service are modified to be placed into the "development" namespace.
* Finally, the `commonLabels` field includes a set of labels that will be added to all resources. In this example, `kustomize` will label the resources with the label name "environment" and a value of "development".

When a user runs `kustomize build .` in the directory with the `kustomization.yaml` and the referenced resources (the files `deployment.yaml` and `service.yaml`), the output is the customized text with the changes found in the `kustomization.yaml` file. Users can redirect the output if they want to capture the changes:

    kustomize build . > custom-config.yaml

The output is deterministic (given the same inputs, the output will always be the same), so it may not be necessary to capture the output in a file. Instead, users could pipe the output into another command:

    kustomize build . | kubectl apply -f -

Users can also invoke `kustomize` functionality with `kubectl -k` (as of Kubernetes 1.14). However, be aware that the standalone `kustomize` binary is more recent than the functionality bundled into `kubectl` (as of the Kubernetes 1.15 release).

Readers may be thinking, "Why go through this trouble instead of just editing the files directly?" That's a fair question. In this example, users _could_ modify the `deployment.yaml` and `service.yaml` files directly, but what if the files were a fork of someone else's content? Modifying the files directly makes it difficult, if not impossible, to rebase the fork when changes are made to the origin/source. However, using `kustomize` allows users to centralize those changes in the `kustomization.yaml` file, leaving the original files untouched and thereby facilitating the ability to rebase the source files if needed.

The benefits of `kustomize` become more apparent in more complex `kustomize` use cases. In the example shown above, the `kustomization.yaml` and the resources are in the same directory. However, `kustomize` supports use cases where there is a "base configuration" and multiple "variants", also known as _overlays_. Say a user wanted to take this simple Nginx Deployment and Service I've been using as an example and create development, staging, and production versions (or variants) of those files. Using overlays with shared base resources would accomplish this.

To help illustrate the idea of overlays with base resources, let's assume the following directory structure:

```
- base
  - deployment.yaml
  - service.yaml
  - kustomization.yaml
- overlays
  - dev
    - kustomization.yaml
  - staging
    - kustomization.yaml
  - prod
    - kustomization.yaml
```

In the `base/kustomization.yaml` file, users would simply declare the resources that should be included by `kustomize` using the `resources` field.

In each of the `overlays/{dev,staging,prod}/kustomization.yaml` files, users would reference the base configuration in the `resources` field, and then specify the particular changes for _that environment_. For example, the `overlays/dev/kustomization.yaml` file might look like the example shown earlier:

```yaml
resources:
- ../../base
namePrefix: dev-
namespace: development
commonLabels:
  environment: development
```

However, the `overlays/prod/kustomization.yaml` file could look very different:

```yaml
resources:
- ../../base
namePrefix: prod-
namespace: production
commonLabels:
  environment: production
  sre-team: blue
```

When a user runs `kustomize build .` in the `overlays/dev` directory, `kustomize` will generate a development variant. However, when a user runs `kustomize build .` in the `overlays/prod` directory, a production variant is generated. All without _any_ changes to the original (base) files, and all in a declarative and deterministic way. Users can commit the base configuration and the overlay directories into source control, knowing that repeatable configurations can be generated from the files in source control.

There's a _lot_ more to `kustomize` that what I've touched upon in this post, but hopefully this gives enough of an introduction to get folks started.

## Additional Resources

There are quite a few good articles and posts written about `kustomize`; here are a few that I found helpful:

[Change base YAML config for different environments prod/test using Kustomize][link-1]

[Kustomize - The right way to do templating in Kubernetes][link-2]

[Declarative Management of Kubernetes Objects Using Kustomize][link-4]

[Customizing Upstream Helm Charts with Kustomize][link-11]

If anyone has questions or suggestions for improving this post, I'm always open to reader feedback. Feel free to [contact me via Twitter][link-12], or hit me up on [the Kubernetes Slack instance][link-13]. Have fun customizing your manifests with `kustomize`!

[link-1]: https://levelup.gitconnected.com/kubernetes-change-base-yaml-config-for-different-environments-prod-test-6224bfb6cdd6
[link-2]: https://blog.stack-labs.com/code/kustomize-101/
[link-3]: https://kustomize.io
[link-4]: https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/
[link-5]: https://tools.ietf.org/html/rfc6902
[link-6]: https://github.com/kubernetes-sigs/kustomize/tree/master/examples
[link-7]: https://kubectl.docs.kubernetes.io/pages/app_customization/introduction.html
[link-8]: https://github.com/kubernetes-sigs/kustomize
[link-9]: https://kubernetes.io/
[link-10]: https://github.com/kubernetes-sigs/kustomize/blob/master/docs/fields.md
[link-11]: https://testingclouds.wordpress.com/2018/07/20/844/
[link-12]: https://twitter.com/scott_lowe
[link-13]: https://kubernetes.slack.com/
