---
author: slowe
categories: Explanation
comments: true
date: 2024-08-30T10:00:00-06:00
tags:
- Ansible
- AWS
- Kubernetes
- Packer
title: Preloading Extra Images with Kubernetes Image Builder
url: /2024/08/30/preloading-extra-images-with-kubernetes-image-builder/
---

The Image Builder project is a set of tools aimed at automating the creation of Kubernetes disk images---such as VM templates or Amazon Machine Images (AMIs). (Interesting side note: Image Builder is the evolution of [a much older Heptio project][link-2] where I was a minor contributor.) I recently had a need to build a custom AMI with some extra container images preloaded, and in this post I'll share with you how to configure Image Builder to preload additional container images.<!--more-->

[Image Builder][link-5] isn't a single binary; it's a framework built on top of other tools such as [Packer][link-3] and [Ansible][link-4]. Although in this post I'm discussing Image Builder in the context of building an AMI, it's not limited to use with AWS. You can use Image Builder for a pretty wide collection of platforms (check [the Image Builder web site][link-1] for more details).

To have Image Builder preload additional images into your disk image, there are three changes needed. All three of these changes belong in the `images/capi/packer/config/additional_components.json` file:

1. Set `load_additional_components` to `true`. (The default value is `false`.)
2. Set `additional_registry_images` to `true`. (This also defaults to `false`.)
3. Set `additional_registry_images_list` to a comma-delimited list of fully-qualified image names. For example, if you wanted to preload the "cilium:v1.15.8" container image, you'd specify "quay.io/cilium/cilium:v1.15.8".

That's it---you don't need any other changes. (Shout-out to the Image Builder authors and contributors for making the tooling extendable and customizable.) I didn't find this until after the fact, but [the Image Builder web site][link-1] has an example of a customized `additional_components.json` file [here][link-6].

From there, you're ready to build your disk image. In my case, I needed to build an Ubuntu-based AMI, so I used this command:

```bash
make build-ami-ubuntu2204
```

The specific targets to use with the `make` command are in the `Makefile` but I will admit it's a bit hard to decipher the `Makefile`. To build an AMI, the target will take the form `build-ami-<base-os>`. Examples include `build-ami-amazon-2` and `build-ami-flatcar`, along with the usual Ubuntu suspects.

Once your disk image is complete, the `make` command will return the identifiers needed to locate or identify the finished images. For an AMI, you'll get a list of the AMI IDs for the regions where the AMI exists. (The regions are configurable in Image Builder.)

I hope this information is useful to someone. Although this information is available on the Image Builder web site, it wasn't immediately clear to me when I first started down this path. If nothing else, this post should at least make the existing information on the Image Builder site a bit easier to find.

If you have questions about this post or suggestions on how I can improve it, I'd love to hear from you. Feel free to reach out [via Twitter][link-7], [via Mastodon][link-8], via Slack (I frequent [the Kubernetes Slack instance][link-9], among others), or even via e-mail. Thanks for reading!

[link-1]: https://image-builder.sigs.k8s.io/
[link-2]: https://github.com/vmware-archive/wardroom
[link-3]: https://www.packer.io/
[link-4]: https://www.ansible.com/
[link-5]: https://github.com/kubernetes-sigs/image-builder
[link-6]: https://image-builder.sigs.k8s.io/capi/capi#loading-additional-components-using-additional_componentsjson
[link-7]: https://twitter.com/scott_lowe
[link-8]: https://fosstodon.org/@scottslowe
[link-9]: https://kubernetes.slack.com
