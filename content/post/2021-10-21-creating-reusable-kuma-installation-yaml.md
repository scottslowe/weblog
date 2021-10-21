---
author: slowe
categories: Tutorial
comments: true
date: 2021-10-21T09:15:00-06:00
tags:
- CLI
- Kubernetes
- Kuma
- Kustomize
- YAML
title: Creating Reusable Kuma Installation YAML
url: /2021/10/21/creating-reusable-kuma-installation-yaml/
---

Using CLI tools---instead of a "wall of YAML"---to install things onto [Kubernetes][link-3] is a growing trend, it seems. [Istio][link-1] and [Cilium][link-2], for example, each have a CLI tool for installing their respective project. I get the reasons why; you can build logic into a CLI tool that you can't build into a YAML file. [Kuma][link-4], the open source service mesh maintained largely by Kong and a CNCF Sandbox project, takes a similar approach with its `kumactl` tool. In this post, however, I'd like to take a look at creating reusable YAML to install Kuma, instead of using the CLI tool every time you install.<!--more-->

You might be wondering, "Why?" That's a fair question. Currently, the `kumactl` tool, unless configured otherwise, will generate a set of TLS assets to be used by Kuma (and embeds some of those assets in the YAML regardless of the configuration). Every time you run `kumactl`, it will generate a new set of TLS assets. This means that the _command_ is not declarative, even if the _output_ is. Unfortunately, you can't reuse the output, as that would result in duplicate TLS assets across installations. That brings me to the point of this post: how can one create reusable YAML to install Kuma?

Fortunately, this is _definitely_ possible. There are two parts to this process:

1. Define replacement TLS assets using cert-manager.
2. Modify the output of `kumactl` to reference the replacement TLS assets.

## Defining TLS Assets

Instead of allowing `kumactl` to generate TLS assets every time the command is run, you need a way to be able to declaratively define what TLS assets are needed and what the properties of those assets should be. Fortunately, that's exactly what the [cert-manager][link-7] project does!

Relying on cert-manager to handle TLS assets does mean that cert-manager becomes a dependency (or a prerequisite) for Kuma---it will have to be installed before Kuma can be installed.

To define the necessary TLS assets, you'll use cert-manager to:

1. Create a self-signed ClusterIssuer.
2. Use the self-signed ClusterIssuer to issue a CA root certificate (and a corresponding Secret to store the private key).
3. Configure the root CA certificate as an Issuer.
4. Issue a TLS certificate and key that will be used by Kuma.

The root CA certificate definition could look something like this:

```yaml
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: kuma-root-ca
  namespace: kuma-system
spec:
  isCA: true
  commonName: kuma-root-ca
  secretName: kuma-root-ca
  duration: 43800h # 5 years
  renewBefore: 720h # 30d
  privateKey:
    algorithm: RSA
    encoding: PKCS1
    size: 2048
  usages:
    - digital signature
    - key encipherment
    - cert sign
  issuerRef:
    name: selfsigned-issuer # References self-signed ClusterIssuer
    kind: ClusterIssuer
    group: cert-manager.io
```

Here's an example of a cert-manager Certificate resource for the TLS certificate that Kuma would use:

```yaml
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: kuma-tls-general
  namespace: kuma-system
spec:
  commonName: kuma-tls-general
  secretName: kuma-tls-general
  duration: 8760h # 1 year
  renewBefore: 360h # 15d
  subject:
    organizations:
      - kuma
  isCA: false
  privateKey:
    algorithm: RSA
    encoding: PKCS1
    size: 2048
  usages:
    - digital signature
    - key encipherment
    - server auth
    - client auth
  dnsNames:
    - kuma-control-plane.kuma-system
    - kuma-control-plane.kuma-system.svc
  issuerRef:
    name: kuma-ca-issuer # References Issuer based on kuma-root-ca
    kind: Issuer
    group: cert-manager.io
```

Make note of the `name` and `secretName` values for both certificates; they will be needed later.

After all the TLS assets have been defined---they don't need to be actually applied against the cluster, just defined---then you're ready to modify the installation YAMl to make it reusable.

## Creating Reusable Installation YAML

Once you've defined the TLS assets, then you can make the necessary changes to the YAML output of `kumactl` to make it reusable. Keep in mind, as described in the previous section, using cert-manager to manage TLS assets means that cert-manager becomes a dependency for Kuma (in other words, you'll need to install cert-manager before you can install Kuma).

Begin by creating a starting point with `kumactl` and piping the output to a file:

    kumactl install control-plane --tls-general-secret=kuma-tls-general \
    --tls-general-ca-bundle=$(echo "blah") > kuma.yaml

For the `--tls-general-secret` parameter, you're specifying the name of the Secret created by the general TLS certificate you defined earlier with cert-manager.

The file created by this command needs four changes made to it:

1. The `caBundle` value supplied for all webhooks needs to be deleted (hence, the value you specify on the command line doesn't matter).
2. All webhooks need to be annotated for the cert-manager [CA Injector](https://cert-manager.io/docs/concepts/ca-injector/) to automatically inject the correct `caBundle` value.
3. The "kuma-control-plane" Deployment needs to be modified to mount the root CA certificate's Secret (created by cert-manager) as a volume.
4. The "kuma-control-plane" Deployment needs to be changed to pass in a different value for the `KUMA_RUNTIME_KUBERNTES_INJECTOR_CA_CERT_FILE` environment variable (it should point to the `ca.crt` file on the volume added in step 3).

You _could_ make these changes manually, but since we're going for declarative why not use something like [Kustomize][link-8]?

To make the first change---removing the `caBundle` value embedded by `kumactl`---you could use this JSON 6902 patch:

```json
[
    { "op": "remove", "path": "/webhooks/0/clientConfig/caBundle" },
    { "op": "remove", "path": "/webhooks/1/clientConfig/caBundle" },
    { "op": "remove", "path": "/webhooks/2/clientConfig/caBundle" }
]
```

To make the second change, you could use a JSON 6902 patch like this (the use of "kuma-root-ca" in the patch below refers to the name of the root CA Certificate resource defined earlier with cert-manager):

```json
[
  { "op": "add",
    "path": "/metadata/annotations", 
    "value": 
      { "cert-manager.io/inject-ca-from": "kuma-system/kuma-root-ca" }
  }
]
```

These two changes enable you to remove the Base64-encoded copy of the CA certificate---referenced by Kuma's webhooks---and instead have cert-manager's CA Injector insert the correct value instead.

This JSON 6902 patch would handle the third change:

```json
[
  {
    "op": "add",
    "path": "/spec/template/spec/volumes/0",
    "value": {
      "name": "general-ca-crt",
      "secret": {
        "secretName": "kuma-root-ca"
      }
    }
  },
  {
    "op": "add",
    "path": "/spec/template/spec/containers/0/volumeMounts/0",
    "value": {
      "name": "general-ca-crt",
      "mountPath": "/var/run/secrets/kuma.io/ca-cert",
      "readOnly": true
    }
  }
]
```

The Secret referenced in the first part of the patch above references the Secret for the root CA Certificate resource, as noted in the `secretName` field on the Certificate's manifest.

And, finally, the fourth change can be handled using this JSON 6902 patch:

```json
[
  { "op": "replace",
    "path": "/spec/template/spec/containers/0/env/11/value",
    "value": "/var/run/secrets/kuma.io/ca-cert/ca.crt" }
]
```

I won't walk through all of these changes in great detail, but I do want to take a moment to dive a bit deeper into the Secret mounted as an additional volume. Using the JSON 6902 patch above against the base YAML created using `kumactl` will result in a configuration that looks like this (focused only on volumes and volumeMounts in the Deployment, everything else is stripped away):

```yaml
spec:
  template:
    spec:
      containers:
      - volumeMounts:
        - mountPath: /var/run/secrets/kuma.io/ca-cert
          name: general-ca-crt
          readOnly: true
        - mountPath: /var/run/secrets/kuma.io/tls-cert
          name: general-tls-cert
          readOnly: true
        - mountPath: /etc/kuma.io/kuma-control-plane
          name: kuma-control-plane-config
          readOnly: true
      volumes:
      - name: general-ca-crt
        secret:
          secretName: kuma-root-ca
      - name: general-tls-cert
        secret:
          secretName: kuma-tls-general
      - configMap:
          name: kuma-control-plane-config
        name: kuma-control-plane-config
```

The "general-ca-cert" Secret and volume are what's been added by the Kustomize patch. Why? When generating the YAML output, `kumactl` combines three resources---the TLS certificate, the TLS key, and the CA certificate---into a single Secret. However, cert-manager won't create a Secret like that. So, to avoid an additional manual step, you mount the Secret created by cert-manager for CA root certificate as a separate volume. This allows you modify the environment variable passed to the control plane specifying where the CA certificate is located to the new path where the volume is mounted.

After the four changes are complete, the resulting modified YAML is completely reusable.

## Using the Reusable YAML

To actually use the reusable YAML you've just created with the above steps:

1. Apply the cert-manager TLS definitions to the cluster where you want to install Kuma. This will create all the necessary Certificate resources and Secrets. (Obviously cert-manager will need to be installed already.)
2. Apply the Kuma YAML created by the steps above. It's configured to reference the cert-manager assets created in step 1.

That's it.

## Caveats/Limitations

The process and changes outlined in this post only apply to a standalone (single zone) installation. It's absolutely possible to do this for multizone deployments, too, but I'll leave that as an exercise for the readers.

## Additional Resources

I've recently published two GitHub repositories with content that supports what's described in this post:

* The ["kuma-cert-manager" repository][link-5] outlines how to replace `kumactl`-generated TLS assets with resources from cert-manager. This supports the "Defining TLS Assets" section above. Full examples of all the related cert-manager resources are found in this repository.
* The ["kuma-declarative-install" repository][link-6] builds on the previous repository by showing the additional changes that must be made to the YAML output generated by `kumactl`. This supports the "Creating Reusable Installation YAML" section above. This includes a Kustomize configuration that will automate all the changes necessary to make the YAML reusable.

I hope this information is useful. If you have questions, feel free to find me on [the Kuma community Slack][link-9] or contact [me on Twitter][link-10] (my DMs are open).

[link-1]: https://istio.io
[link-2]: https://cilium.io
[link-3]: https://kubernetes.io
[link-4]: https://kuma.io
[link-5]: https://github.com/scottslowe/kuma-cert-manager
[link-6]: https://github.com/scottslowe/kuma-declarative-install
[link-7]: https://cert-manager.io
[link-8]: https://kustomize.io
[link-9]: https://kuma-mesh.slack.com
[link-10]: https://twitter.com/scott_lowe
