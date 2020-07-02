---
author: slowe
categories: Explanation
comments: true
date: 2019-10-29T07:30:00-07:00
tags:
- Kubernetes
- JavaScript
- TypeScript
- CLI
- YAML
title: Programmatically Creating Kubernetes Manifests
url: /2019/10/29/programmatically-creating-kubernetes-manifests/
---

A while ago I came across [a utility named `jk`][link-5], which purported to be able to create structured text files---in JSON, YAML, or HCL---using JavaScript (or TypeScript that has been transpiled into JavaScript). One of the use cases was creating [Kubernetes][link-1] manifests. The [GitHub repository for `jk`][link-4] describes it as "a data templating tool", and that's accurate for simple use cases. In more complex use cases, the use of a general-purpose programming language like JavaScript in `jk` reveals that the tool has the potential to be much more than just a data templating tool---if you have the JavaScript expertise to unlock that potential.<!--more-->

The basic idea behind `jk` is that you could write some relatively simple JavaScript, and `jk` will take that JavaScript and use it to create some type of structured data output. I'll focus on Kubernetes manifests here, but as you read keep in mind you could use this for other purposes as well. (I explore a couple other use cases at the end of this post.)

Here's a very simple example:

```javascript
const service = new api.core.v1.Service('appService', {
    metadata: {
        namespace: 'appName',
        labels: {
            app: 'appName',
            team: 'blue',
        },
    },
    spec: {
        selector: {
            app: 'appName',
        },
        ports: [{
            port: 80,
        }],
    },
});
```

As you might guess, this would generate a manifest for a Kubernetes Service object. In this example, you can see why `jk` is described as a data templating tool---the user essentially has to recreate a Service object as a JavaScript object, and then `jk` just does the rendering into YAML. In my opinion, using `jk` in this way doesn't really offer a great deal of value. Developers familiar with JavaScript may prefer this format as opposed to YAML, and teams could leverage a test framework to test the code. In this instance, one could make a comparison to [Pulumi][link-2]; the same thing could be accomplished using a domain-specific language, but developers may prefer and be more productive with a general purpose programming language like JavaScript.

Given that `jk` can product multiple artifacts from the same JavaScript code (if the code is written correctly---the example above is _not_ written to support multiple outputs), a more useful way to do leverage `jk` would be to combine all (or as much as possible) of the information for an application and then generate multiple outputs. I won't post all the code here, but instead point you to [the micro-service example in the `jk` repository][link-6] as an example of this approach. (This example is also described in [this blog post introducing `jk`][link-3].)

The basic gist of the micro-service example from the `jk` repository is this:

* One JavaScript module (`kubernetes.js`) has a series of functions that generate JavaScript objects that correspond to the major Kubernetes objects (Namespace, Service, Deployment, Ingress, and ConfigMap). This module also imports a couple other JavaScript modules that create Prometheus rules and Grafana dashboards.
* Other JavaScript modules (like `billing.js`) import the module with the function definitions and use data from a generic JavaScript object to call the functions and generate multiple YAML manifests. The generic JavaScript object contains all the information needed by all the functions, although each individual function will generally only use a subset of the information in the generic object.

The end result is that a developer can combine all the information needed to deploy an application on Kubernetes in one place, and then programmatically generate the YAML manifests for the Namespace, Deployment, Service, and Ingress from that _one source._ Need to change a value? Change it in one place, re-generate the manifests, and the value will be changed in all places where it was referenced. This, in my opinion, is a far more useful example of how `jk` could be leveraged. It does require JavaScript knowledge in order to unlock this functionality, as you have to write actual JavaScript code to define functions, import modules, etc., as opposed to just creating a JavaScript object whose structure mirrors that of the desired output.

I could also envision use cases for `jk` involving creating both Kubernetes manifests and Terraform HCL (maybe you want to create a Route 53 entry that corresponds to an Ingress object, or maybe you need to update a CDN when creating a new Ingress object). The Quick Start in the project's documentation briefly discusses using `jk` to generate Terraform for the GitHub provider as well as generating JSON output, so there's another example of creating multiple outputs from the same code. I won't say the possibilities are endless, but hopefully you can see there is quite a bit of flexibility here. The tradeoff, as I mentioned earlier, is that you'll need JavaScript expertise in order to really leverage this flexibility.

Are you using `jk`? What sort of use cases can you envision for a tool that enables you to programmatically create configuration files? I'd love to hear from you, so [hit me up on Twitter][link-99] and let me know what you think!

[link-1]: https://kubernetes.io/
[link-2]: https://www.pulumi.com/
[link-3]: https://damien.lespiau.name/posts/2019-06-12-jk-configuration-as-code/
[link-4]: https://github.com/jkcfg/jk
[link-5]: https://jkcfg.github.io/#/
[link-6]: https://github.com/jkcfg/jk/tree/master/examples/kubernetes/micro-service
[link-7]: https://jkcfg.github.io/#/documentation/quick-start
[link-99]: https://twitter.com/scott_lowe
