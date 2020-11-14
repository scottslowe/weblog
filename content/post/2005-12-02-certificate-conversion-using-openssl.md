---
author: slowe
categories: Tutorial
comments: false
date: 2005-12-02T16:08:35Z
slug: certificate-conversion-using-openssl
tags:
- Interoperability
- Encryption
- Linux
- macOS
- Security
- SSL
- Windows
title: Certificate Conversion Using OpenSSL
url: /2005/12/02/certificate-conversion-using-openssl/
wordpress_id: 133
---

In my various adventures with replacing Windows-based servers with Linux servers, I have had need on several occasions to take an SSL certificate (generated using Microsoft Certificate Services) and make it available to a Linux-based server. In some cases, it was to be used by the SSL wrapper [Stunnel](http://stunnel.mirt.net/index.html); in others, it was for [Apache](http://www.apache.org/httpd/).

In either case, the certificate needed to be in the PEM (originally Privacy Enhanced Mail, now a standard for encoding certificates) format. Windows, on the other hand, typically uses the PFX (PKCS #12) format for storing certificates. To prepare certificates for use on Linux, the certificates must be converted to PEM format.

In the hopes that this information might be useful to others, here's a process for converting from PFX to PEM using [OpenSSL](http://www.openssl.org/). With the exception of only a few items, the process is identical on Linux, [Mac OS X](http://www.apple.com/macosx/), and [Windows 2000/2003](http://www.microsoft.com/windowsserver2003/default.mspx). These steps were confirmed using OpenSSL 0.9.7a on Red Hat Linux 9.0, using OpenSSL 0.9.6i on Mac OS X 10.2.8, and OpenSSL 0.9.7c on Windows Server 2003.

1. If the certificate is not already available in PFX format, use the Certificates MMC snap-in to export the certificate and the corresponding private key to a PFX file. Be sure to note the passphrase used to protect the PFX file; it will be needed later. If the certificate (and its corresponding private key) is already available in PFX format and the passphrase for the PFX file is known, proceed to step 2.

2. At a terminal prompt, type `openssl pkcs12 -in pfxfilename.pfx -out tempfile.pem`.  This will convert the PFX file to a PEM file. The OpenSSL toolkit will prompt for the import passphrase; this will be the passphrase specified in step 1. A PEM passphrase will also be needed; make note of the passphrase used here (it will be needed later).

3. Using a text editor (such as vi on a Linux system, or Notepad on a Windows system), split the encrypted RSA private key and the certificate into two separate files. Remove all extra text, leaving only the text between the lines with the dashes. Make note of the filenames; they will be needed later.

4. The RSA key is currently encrypted; this will prevent an automated launch for the application using the certificate. It will be necessary to decrypt the RSA private key. To decrypt the RSA private key, use the command `openssl rsa -in encryptedkey -out decryptedkey` (where _encryptedkey_ is the file containing the RSA private key, as separated in step 3, and _decryptedkey_ is the file that will contain the decrypted RSA private key). The OpenSSL toolkit will prompt for the RSA key passphrase; this will be the passphrase specified in step 2.

5. On Linux or Mac OS X: To concatenate the decrypted RSA private key and the certificate into a single file, use the command `cat decryptedkey certificatefile > _finalfile.pem_` (where _decryptedkey_ is the decrypted RSA key produced in step 4 and _certificatefile_ is the file containing only the certificate, as produced in step 3).

6. On Windows: To combine the decrypted RSA private key and the certificate into a single file, use the command `copy /b decryptedkey+certificatefile finalfile.pem` (where _decryptedkey_ is the decrypted RSA key produced in step 4 and _certificatefile_ is the file containing only the certificate, as produced in step 3).

7. Edit the concatenated file (`finalfile.pem` from step 5/6) with a text editor to add a blank line between the decrypted RSA private key and the certificate, and a blank line after the end of the certificate.

At this point, the certificate is ready for use. **_Be sure_** to guard the unencrypted private key with extreme caution; loss of this file can result in the compromise of your system. Note also that most applications require that the certificates be owned by root and readable only by root (`chown root:root` to make them owned by root and `chmod 0600` to make them readable only by root).
