---
author: slowe
categories: Tutorial
comments: true
date: 2022-01-12T09:00:00
tags:
- Encryption
- CLI
- Security
title: Getting Certificate Details from HashiCorp Vault
url: /2022/01/12/getting-certificate-details-from-hashicorp-vault/
---

It seems there are lots of tutorials on setting up a PKI (public key infrastructure) using [HashiCorp Vault][link-1]. What I've found missing from most of these tutorials, however, is how to get details on certificates issued by a Vault-driven PKI _after_ the initial creation. For example, someone other than you issued a certificate, but now you need to get the details for said certificate. How is that done? In this post, I'll show you a couple ways to get details on certificates issued and stored in HashiCorp Vault.<!--more-->

For the commands and API calls I've shared below, I'm using "pki" as the name/path you (or someone else) assigned to a PKI secrets engine within Vault. If you're using a different name/path, then be sure to substitute the correct name/path as appropriate.

To use the Vault CLI to see the list of certificates issued by Vault, you can use this command:

```bash
vault list pki/certs
```

This will return a list of the serial numbers of the certificates issued by this PKI. Looking at just serial numbers isn't terribly helpful, though. To get more details, you first need to read the certificate details (note singular "cert" here versus plural "certs" in the previous command):

```bash
vault read pki/cert/<serial-num>
```

Now the output is a formatted response that includes the PEM-encoded certificate. That's a bit more useful, but still not quite all the way to seeing all the details about the certificate. Fortunately, the Vault CLI supports a `-format=json` parameter, and you can couple that with the trusty tool `jq` to parse the output (see [this post][xref-1] if you're not familiar with `jq`):

```bash
vault read -format=json pki/cert/<serial-num> | jq -r '.data.certificate'
```

Finally! One more step: pipe the output of the command through `openssl x509`, like this:

```bash
vault read -format=json pki/cert/<serial-num> | \
jq -r '.data.certificate' | \
openssl x509 -in - -noout -text
```

Bam! Now you have all the details on the certificate.

It's also possible to do this via Vault's HTTP API. Using `curl`, you can retrieve the list of certificate serial numbers with this API call:

```bash
curl -s --request LIST https://<vault-ip-or-hostname>/v1/pki/certs
```

Armed with a serial number, you can then retrieve a specific certificate:

```bash
curl -s https://<vault-ip-or-hostname>/v1/pki/cert/<serial-num>
```

And then, naturally, you can use `jq` and `openssl` to see the certificate details:

```bash
curl -s https://<vault-ip-or-hostname>/v1/pki/cert/<serial-num> | \
jq -r '.data.certificate' | \
openssl x509 -in - -noout -text
```

Per [the Vault API docs][link-2], the endpoints for retrieving the list of certificates or retrieving a specific certificate are unauthenticated endpoints, so there's no need to include the "X-Vault-Token" header with your API requests.

I hope this is useful. If you have questions---or corrections, in the event I explained something incorrectly above---feel free to reach out to [me on Twitter][link-3], or find me in any one of a number of Slack communities.

[link-1]: https://www.vaultproject.io/
[link-2]: https://www.vaultproject.io/api-docs/secret/pki
[link-3]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
