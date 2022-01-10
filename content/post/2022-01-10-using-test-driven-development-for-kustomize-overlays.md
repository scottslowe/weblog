---
author: slowe
categories: Tutorial
comments: true
date: 2022-01-10T09:00:00
tags:
- CLI
- Kubernetes
- Kustomize
- OPA
- YAML
title: Using Test-Driven Development for Kustomize Overlays
url: /2022/01/10/using-test-driven-development-for-kustomize-overlays/
---

I am by no means a developer (not by a long shot!), but I have been learning lots of development-related things over the last several years and trying to incorporate those into my workflows. One of these is the idea of _test-driven development_ (see [Wikipedia][link-1] for a definition and some additional information), in which one writes tests to validate functionality before writing the code to implement said functionality (pardon the paraphrasing). In this post, I'll discuss how to use [`conftest`][link-3] to (loosely) implement test-driven development for [Kustomize][link-2] overlays.<!--more-->

If you're unfamiliar with Kustomize, then [this introductory article I wrote][xref-1] will probably be useful.

For the discussion around using the principles of test-driven development for Kustomize overlays, I'll pull in a recent post I did on [creating reusable YAML for installing Kuma][xref-2]. In that post, I pointed out four changes that needed to be made to the output of `kumactl install control-plane` to make it reusable:

1. Remove the `caBundle` value for all webhooks.
2. Annotate all webhooks so that cert-manager will inject the correct `caBundle` value.
3. Add a volume and volume mount to the "kuma-control-plane" Deployment.
4. Change one of the environment variables for the "kuma-control-plane" Deployment to reference the volume added in step 3.

Since I know what specific changes I need to see in the original YAML, I _should_ be able to write some sort of test that tells me if that change has been made or not. I can run the test against the original YAML, knowing that it will fail, before writing the necessary Kustomize patches to make the change. Then I can test again to verify if the Kustomize patches worked as expected or anticipated.

This is where `conftest` comes in. The `conftest` command-line utility uses the Rego policy language from [Open Policy Agent][link-4], and is intended to allow users to do exactly what I'm trying to do. Specifically, it allows me to run a series of tests against YAML manifests to ensure that the test conditions (whatever those may be) are satisfied. The "test conditions" here are that the necessary changes have been made in order to make the installation YAML reusable.

I'll start with the first change (removing the `caBundle` value). Here's a Rego stanza that will test for the presence of a `caBundle` value (I'll explain the code below):

```
warn[msg] {
    input.kind == "ValidatingWebhookConfiguration"
    some i; input.webhooks[i].clientConfig.caBundle
    msg = "caBundle value not removed for a validating webhook"
}
```

Before moving on to the other tests, let's break this one down a bit:

* The `warn[msg]` header tells `conftest` to warn with the supplied message if the following conditions are true. The conditions within the curly braces act as a logical AND; all of them must be true in order to cause `conftest` to warn with the supplied message.
* The first line checks to see if `input.kind` is equal to ValidatingWebhookConfiguration. The input here is whatever YAML being fed to `conftest`, so `input.kind` refers to the top-level `kind` object in the input YAML.
* The second line is Rego's form of iteration. Rego will essentially iterate over all the webhooks in the input YAML and look for the presence of `clientConfig.caBundle` in each webhook. If it finds at least one webhook with `clientConfig.caBundle` present, then this condition is true.
* The last line in the curly braces sets the message to be displayed by `conftest` if all the conditions are true.

I can place this test into a file, which `conftest` expects to be in a subfolder named "policy", and then pipe the output of `kumactl install control-plane` through it like this:

    kumactl install control-plane | conftest test -

This will produce a warning, of course, but that's exactly what we wanted---we want to the test to indicate that the `caBundle` hasn't been removed.

The test above checked for the `caBundle` value for validating webhooks; this slightly modified version does the same thing for mutating webhooks:

```
warn[msg] {
    input.kind == "MutatingWebhookConfiguration"
    some i; input.webhooks[i].clientConfig.caBundle
    msg = "caBundle value not removed for a validating webhook"
}
```

Let's look at another change and how we can test for it. What about change #4 from above---changing an environment variable being passed to the "kuma-control-plane" Deployment? Here's a Rego test for that:

```
warn[msg] {
    input.kind == "Deployment"
    env := input.spec.template.spec.containers[0].env[_]
    env.name == "KUMA_RUNTIME_KUBERNETES_INJECTOR_CA_CERT_FILE"
    not env.value == "/var/run/secrets/kuma.io/ca-cert/ca.crt"
    msg = "KUMA_RUNTIME_KUBERNETES_INJECTOR_CA_CERT_FILE is not set correctly"
}
```

The syntax here is a bit different, so let's discuss it before moving on:

* The header and the first line inside the braces are similar/the same as what I explained above in the previous example.
* The second line in the braces creates a new variable, called `env`, and assigns it the contents of the `env` array of values under `spec.template.spec.container[0]` (i.e., the first container defined in the Deployment). The `env[_]` syntax is like another form of iteration, allowing Rego to perform the next two checks on all the contents of the array of values.
* The next two lines check for a) an entry whose `name` field is set to KUMA_RUNTIME_KUBERNETES_INJECTOR_CA_CERT_FILE _and_ b) whose `value` field is NOT set to the specified path. Recall that conditions within the braces are like a logical AND, so specifying both of these conditions allows the test to find instances where the environment variable is defined incorrectly (i.e., with a different value than what is expected/desired).
* The test closes out with setting the message `conftest` will display if all the conditions are true.

The test for change #2 is reasonably straightforward:

```
warn[msg] {
    input.apiVersion == "admissionregistration.k8s.io/v1"
    not input.metadata.annotations["cert-manager.io/inject-ca-from"]
    msg = "cert-manager CA injection annotation missing from a webhook"
}
```

The only notable syntax note here is how Rego would reference the name of the annotation, which is "cert-manager.io/inject-ca-from". If a field name contains only letters or numbers, then the `input.metadata.annotations` syntax (separating each field with a period) is sufficient. When a field contains something other than letters and numbers, then you need to move to the `input.metadata.annotations["cert-manager.io/inject-ca-from"]` syntax, where the field name is referenced in brackets and quotes.

Finally, I can write the tests for the last change: adding a volume and mounting it to a path in the container. This change is a bit more complex. In this case, the tests need to flag three different conditions:

1. If the volume isn't present
2. If the volume is present, but configured to point to the wrong entity (it's supposed to point to a specific Secret)
3. If the volume isn't mounted to the correct path in the container

As you might expect, then, I need three tests. In this case, I'm going to use a slightly different syntax that allows me to define a condition and then reference it later in a rule. First, let's set up the condition:

```
general_ca_vol_present = true {
    input.kind == "Deployment"
    vols := input.spec.template.spec.volumes[_]
    vols.name == "general-ca-crt"
}
```

By now, this syntax should look pretty familiar to you, with the exception of the header. This defines a condition---a subroutine or function, if you will---called `general_ca_vol_present`. This function checks to ensure that the volume does exist in the YAML manifest. I'll reference this in a rule in just a moment.

`<aside>`One side note here: a peculiarity of Rego is that I can't test if the volume _doesn't exist._ If you check for `not vols.name == "general-ca-crt"`, this check is true because there _are_ other volumes whose name is not "general-ca-crt". This may require to you write your tests slightly differently than you might expect at first.`</aside>`

Next, a custom function to see if the volume is configured incorrectly (it doesn't point to the correct Secret):

```
general_ca_vol_incorrect = true {
    input.kind == "Deployment"
    vols := input.spec.template.spec.volumes[_]
    vols.name == "general-ca-crt"
    not vols.secret.secretName == "kuma-root-ca"
}
```

Finally, a function to check to see if it is mounted into the container correctly:

```
general_ca_crt_volume_mounted = true {
    input.kind == "Deployment"
    mounts := input.spec.template.spec.containers[0].volumeMounts[_]
    mounts.mountPath == "/var/run/secrets/kuma.io/ca-cert"
    mounts.name == "general-ca-crt"
    mounts.readOnly == true
}
```

With the functions defined, I can now write the rules:

```
warn[msg] {
    input.kind == "Deployment"
    not general_ca_vol_present
    msg = "Additional volume not defined for CA of custom TLS certificate"
}

warn[msg] {
    input.kind == "Deployment"
    general_ca_vol_incorrect
    msg = "Additional volume for CA of custom TLS certificate not configured correctly"
}

warn[msg] {
    input.kind == "Deployment"
    not general_ca_crt_volume_mounted
    msg = "Additional volume for CA of custom TLS certificate not mounted or incorrectly mounted in container"
}
```

With all the tests in place, running `kumactl install control-plane | conftest test -` will generate warnings, as expected. From here, I can start to write Kustomize configurations that will make the necessary changes, and run them back through the tests like this (this command assumes the Kustomize configuration is in a directory named "kuma" under the current directory):

    kustomize build kuma | conftest test -

When `conftest` finally reports zero warnings, I'll know my Kustomize configuration is making the changes I coded in my tests. Bam---I've just used the principles behind test-driven development in the creation of a Kustomize configuration.

In this particular case, a sample Kustomize configuration that modifies the base YAML output by `kumactl install control-plane` is found in [this GitHub repository][link-5]. I've also added a Rego file with all the tests described in this post to the repository; it's found in the "policy" directory. You can use `conftest` to verify the sample Kustomize configuration using the Rego file in the "policy" directory.

## Additional Resources

As mentioned above, [this GitHub repository][link-5]---which was created to accompany the original [reusable Kuma installation YAML blog post][xref-2]---has both a sample Kustomize configuation _and_ a Rego policy file you can use with `conftest`. Feel free to take a closer look at either or both, or use them as the basis for something of your own.

If you have any questions, feel free to reach out to [me on Twitter][link-6]. I'd be happy to help, if I'm able. For help with Rego, OPA, or `conftest`, you can also check out [the Open Policy Agent Slack community][link-7].

[link-1]: https://en.wikipedia.org/wiki/Test-driven_development
[link-2]: https://kubernetes-sigs.github.io/kustomize/
[link-3]: https://www.conftest.dev/
[link-4]: https://www.openpolicyagent.org/
[link-5]: https://github.com/scottslowe/kuma-declarative-install
[link-6]: https://twitter.com/scott_lowe
[link-7]: https://openpolicyagent.slack.com
[xref-1]: {{< relref "2019-09-13-an-introduction-to-kustomize.md" >}}
[xref-2]: {{< relref "2021-10-21-creating-reusable-kuma-installation-yaml.md" >}}
