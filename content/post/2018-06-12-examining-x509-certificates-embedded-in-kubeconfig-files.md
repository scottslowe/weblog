---
author: slowe
categories: Education
comments: true
date: 2018-06-12T12:00:00Z
tags:
- Kubernetes
- Linux
- CLI
title: Examining X.509 Certificates Embedded in Kubeconfig Files
url: /2018/06/12/examining-x509-certificates-embedded-in-kubeconfig-files/
---

While exploring some of the intricacies around the use of X.509v3 certificates in [Kubernetes][link-2], I found myself wanting to be able to view the details of a certificate embedded in a kubeconfig file. (See [this page][link-1] if you're unfamiliar with what a kubeconfig file is.) In this post, I'll share with you the commands I used to accomplish this task.<!--more-->

First, you'll want to extract the certificate data from the kubeconfig file. For the purposes of this post, I'll use a kubeconfig file named `config` and found in the `.kube` subdirectory of your home directory. Assuming there's only a single certificate embedded in the file, you can use a simple `grep` statement to isolate this information:

    grep 'client-certificate-data' $HOME/.kube/config

Combine that with `awk` to isolate _only_ the certificate data:

    grep 'client-certificate-data' $HOME/.kube/config | awk '{print $2}'

This data is Base64-encoded, so we decode it (I'll wrap the command using backslashes for readability now that it has grown a bit longer):

    grep 'client-certificate-data' $HOME/.kube/config | \
    awk '{print $2}' | base64 -d

You could, at this stage, redirect the output into a file (like `certificate.crt`) if so desired; the data you have is a valid X.509v3 certificate. It lacks the private key, of course.

However, if you're only interested in viewing the properties of the certificate, as I was, there's no need to redirect the output to a file. Instead, just pipe the output into `openssl`:

    grep 'client-certificate-data' $HOME/.kube/config | \
    awk '{print $2}' | base64 -d | openssl x509 -text

The output of this command should be a decoded breakdown of the data in the X.509 certificate. Notable pieces of information in this context are the Subject (this will identify the user being authenticated to Kubernetes with this certificate):

``` text
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 8264125584782928183 (0x72b0126f24342937)
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: CN=kubernetes
        Validity
            Not Before: Jun 13 01:52:46 2018 GMT
            Not After : Jun 13 01:53:17 2019 GMT
        Subject: CN=system:kube-controller-manager
```

Also of interest is the X509v3 Extended Key Usage (which indicates the certificate is used for client authentication, i.e., "TLS Web Client Authentication"):

``` text
        X509v3 extensions:
            X509v3 Key Usage: critical
                Digital Signature, Key Encipherment
            X509v3 Extended Key Usage: 
                TLS Web Client Authentication
```

Note that the certificate is _not_ configured for encryption, meaning this certificate doesn't ensure the connection to Kubernetes is encrypted. That function is handled by a different certificate; this one is only used for client authentication.

This is probably nothing new for experienced Kubernetes folks, but I thought it might prove useful to a few people out there. Feel free [to hit me up on Twitter][link-3] with any corrections, clarifications, or questions. Have fun examining certificate data!

[link-1]: https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/
[link-2]: https://kubernetes.io/
[link-3]: https://twitter.com/scott_lowe
