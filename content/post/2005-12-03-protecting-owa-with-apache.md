---
author: slowe
categories: Explanation
comments: false
date: 2005-12-03T09:21:54Z
slug: protecting-owa-with-apache
tags:
- Apache
- Exchange
- Security
- SSL
- Interoperability
- Messaging
title: Protecting OWA with Apache
url: /2005/12/03/protecting-owa-with-apache/
wordpress_id: 134
---

Outlook Web Access (OWA) is the web-based interface for accessing e-mail and other resources handled by [Microsoft Exchange](http://www.microsoft.com/exchange/). Unfortunately, OWA's popularity also makes it the target of numerous worms and security exploits. As a result, many organizations seek to deploy OWA behind a reverse proxy that can help shield OWA from web-based attacks and exploits. In this posting, I'm going to share information to help build a reverse proxy using [Apache 2.0](http://www.apache.org/httpd/).

Here's a skeleton of an `httpd.conf` file to support Apache as a reverse proxy in front of OWA:

    NameVirtualHost 1.2.3.4:80
    NameVirtualHost 1.2.3.4:443
    ProxyRequests Off
     
    <VirtualHost 1.2.3.4:443>
    ServerAdmin webmaster@domain.com
    ServerName webmail.domain.com
    DocumentRoot /var/www/webmail
    RequestHeader set Front-End-Https â€œOnâ€
    ProxyRequests Off
    ProxyPreserveHost On
     
    SSLEngine On
    SSLCertificateFile conf/webmail-ssl-cert.pem
     
    <Location /exchange>
    ProxyPass http://mail.domain.com/exchange
    ProxyPassReverse http://mail.domain.com/exchange
    SSLRequireSSL
    </Location>
     
    <Location /exchweb>
    ProxyPass http://mail.domain.com/exchweb
    ProxyPassReverse http://mail.domain.com/exchweb
    SSLRequireSSL
    </Location>
     
    <Location /public>
    ProxyPass http://mail.domain.com/public
    ProxyPassReverse http://mail.domain.com/public
    SSLRequireSSL
    </Location>
     
    </VirtualHost>

The key portions of this configuration are described below, along with some supporting information.

* _NameVirtualHost:_ The NameVirtualHost directive enables Apache to use name-based virtual hosts on the specified IP addresses and ports. The parameter to the NameVirtualHost directive must match one of the VirtualHost definitions, as shown in the sample configuration, or else the content will be served from the default virtual host (the first virtual host listed in the configuration). Note that if the Apache reverse proxy will not be using name-based virtual hosts (instead using IP address-based virtual hosts or running only a single server instance), then this directive is not required.

* _RequestHeader:_ This directive instructs Apache to add a header "Front-End-Https: On" to requests sent to the internal OWA server. This header is proprietary to OWA and forces OWA to build URLs using "https://" references instead of ordinary "http://" references. This directive is required in order to terminate the SSL tunnel at the reverse proxy and use clear-text HTTP between the reverse proxy and the internal OWA server. This directive requires the mod_headers module.

* _ProxyPreserveHost:_ This directive configures Apache to pass the original host header, supplied by the client, to the server to which the request is being proxied. (This is instead of the host name supplied in the ProxyPass directive.) Again, this facilitates the construction of URLs with the correct hostname when accessing resources inside OWA.

* _SSLCertificateFile:_ Apache expects the web server's SSL certificate to be in PEM format. If the certificate's key is encrypted, Apache will prompt upon startup for the passphrase to the key (this prevents any form of automated startup). It is considered a security best practice to keep the key in a separate file (using the SSLCertificateKeyFile directive) in encrypted form and supply the password upon the startup of Apache.

With this configuration in place, the following benefits are realized:

1. Name-based virtual hosts are supported. This allows other URLs to also be proxied through this same reverse proxy server.

2. SSL encryption is offloaded from the Exchange server to the reverse proxy server. Traffic from the reverse proxy server itself to the Exchange server is standard, unencrypted HTTP.

3. When used in conjunction with [mod_security](http://www.modsecurity.org/) (another Apache module), OWA is protected against a very significant majority of all web-based attacks.

Using Apache to serve as a reverse proxy for OWA is a cost-effective way to add another layer of security to an Exchange-based messaging infrastructure.
