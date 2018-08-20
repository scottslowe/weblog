---
author: slowe
categories: Education
comments: true
date: 2018-08-20T12:00:00Z
tags:
- CLI
- Linux
- SSL
title: Troubleshooting TLS Certificates
url: /2018/08/20/troubleshooting-tls-certificates/
---

I was recently working on a blog post involving the use of TLS certificates for encryption and authentication, and was running into errors. I'd checked all the "usual suspects"---AWS security groups, host-level firewall rules (via `iptables`), and the application configuration itself---but still couldn't get it to work. When I did finally find the error, I figured it was probably worth sharing the commands I used in the event others might find it helpful.<!--more-->

The error was manifesting itself in that I was able to successfully connect to the application (with TLS) on the loopback address, but not the IP address assigned to the network adapter. Using `ss -lnt`, I verified that the application was listening on all IP addresses (not just loopback), and as I mentioned earlier I had also verified that AWS security groups and host-level firewall weren't in play. This lead me to believe that there was something wrong with my TLS configuration.

Since the application's error message was extremely vague (and not even remotely TLS-related), I decided to try using `curl` to verify that TLS was working correctly. First I ran this command:

    curl --cacert /path/to/CA/certificate https://127.0.0.1 -v

After some output, `curl` reported this error:

    curl: (35) gnutls_handshake() failed: certificate is bad

At first, I took this to mean that something was actually wrong with the certificate, but I quickly realized this was an expected error---TLS authentication was enabled, and I hadn't given `curl` the correct parameters. So I tried it again, this time with the correct parameters:

    curl --cacert /path/to/CA/certificate \
    --cert /path/to/client/certificate \
    --key /path/to/client/certificate/key \
    https://127.0.0.1 -v

This time the connection succeeded, and the output of the `curl` command showed that TLS encryption and authentication were in place and successful. The next step was to try it against the IP address assigned to the network adapter (where it had been failing):

    curl --cacert /path/to/CA/certificate \
    --cert /path/to/client/certificate \
    --key /path/to/client/certificate/key \
    https://10.200.10.1 -v

And the output tells me, clearly, what's wrong:

    curl: (51) SSL: certificate subject name (ip-10-200-10-1) does not match target host name '10.200.10.1'

Ah, now that's much more informative, but unexpected---was the certificate, in fact, not configured correctly? Fortunately, it was relatively easy to double-check:

    openssl x509 -in /path/to/server/certificate -text

Clearly noted in the output under the "X509v3 Extensions" section is the answer:

    X509v3 Subject Alternative Name:
        DNS:ip-10-200-10-1, DNS:localhost, IP Address:127.0.0.1, IP Address:0:0:0:0:0:0:0:1

Sure enough, the certificate was missing a Subject Alternative Name (SAN) for the network adapter's IP address, and that was enough to cause the application (rightfully so) to fail.

So what are the takeaways? Well, they might be simplistic, but here's what I learned:

* Use `curl -v` to get more verbose information on SSL/TLS connections, especially when the application's error messages aren't very informative.
* Use `openssl x509` to view certificate details so that you can verify that the certificate is configured correctly and has the right properties (not just SANs but also Extended Key Usage would be another area---is the certificate actually enabled for client authentication, for example).

These takeaways may be "common knowledge" for a fair number of folks, but I suspect there's a few readers that will probably find this information handy.
