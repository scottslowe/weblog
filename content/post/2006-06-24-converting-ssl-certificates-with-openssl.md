---
author: slowe
categories: Tutorial
comments: false
date: 2006-06-24T10:17:40Z
slug: converting-ssl-certificates-with-openssl
tags:
- CLI
- Encryption
- Interoperability
- Security
- SSL
title: Converting SSL Certificates with OpenSSL
url: /2006/06/24/converting-ssl-certificates-with-openssl/
wordpress_id: 276
---

The [OpenSSL](http://www.openssl.org/) toolkit is a veritable Swiss Army knife of SSL functionality. Among the many, many things that can be done using OpenSSL is converting SSL certificates between formats. This is particularly helpful in a heterogeneous environment where different platforms may require SSL certificates to be in different formats.

A mixed Windows-Linux shop is one excellent example. Windows typically requires certificates in PFX format; Linux, on the other hand, typically needs PEM format. (See this [X.509 article](http://en.wikipedia.org/wiki/X.509) for more information on the PFX and PEM formats.) Using the OpenSSL toolkit, we can pretty easily convert certificates from PFX to PEM. Here's how.

Before we begin, we'll need to make sure we have the certificate in PFX format with the private key. In organizations that use the Windows Certificate Services as a CA, we can use the Certificates MMC snap-in to export the certificate and the corresponding private key to a PFX file. During this process, we'll be prompted for a passphrase; make note of it, as we'll need it later in the process.

With our PFX file in hand, we start the conversion process:

1. At a command-line prompt, type `openssl pkcs12 -in _pfxfilename.pfx_ -out _tempfile.pem_`. This will convert the PFX file to a PEM file. The OpenSSL toolkit will prompt for the import passphrase; this will be the passphrase for the PFX file when the certificate and private key were exported (as mentioned above). OpenSSL will prompt for a new PEM passphrase; be sure to make note of this information as well.

2. Using a text editor, split the PEM file into two separate files, one containing the certificate and one containing the encrypted private key. Remove all extra text from these files outside the lines with the dashes.

3. Because many Linux-based applications will need the private key decrypted (or they will prompt for the passphrase during service start), we'll decrypt the private key. To decrypt the private key, use the command `openssl rsa -in _encryptedkey_ -out _decryptedkey_` (where _encryptedkey_ is the file containing the RSA private key, as separated above, and _decryptedkey_ is the file that will contain the decrypted RSA private key). The OpenSSL toolkit will prompt for the RSA key passphrase; this will be the PEM passphrase we specified when we first converted the certificate to PEM format above.

4. If the application can use the certificate and the key in separate files, then we're finished. If we need to put them back into the same file, then use the command `cat _decryptedkey_ _certificatefile_ > _finalfile.pem_` (on [Mac OS X](http://www.apple.com/macosx/) or Linux) or the command `copy /b _decryptedkey_+_certificatefile_ _finalfile.pem_` (on Windows). This will combine the certificate and the decrypted private key into a single file. Using a text editor, add a blank line between the decrypted RSA private key and the certificate, and a blank line after the end of the certificate.

The final file is now ready for use with any number of Linux-based applications, such as [Stunnel](http://stunnel.mirt.net/index.html), [Apache](http://httpd.apache.org/), [Postfix](http://www.postfix.org/), or others.

**UPDATE:** It turns out this is a duplicate post, originally covered earlier [here][1]. Sorry!

[1]: {{< relref "2005-12-02-certificate-conversion-using-openssl.md" >}}
