---
author: slowe
categories: Tutorial
comments: true
date: 2006-08-10T09:38:37Z
slug: kerberos-based-sso-with-apache
tags:
- ActiveDirectory
- Apache
- CentOS
- Interoperability
- Kerberos
- Linux
- Web
title: Kerberos-Based SSO with Apache
url: /2006/08/10/kerberos-based-sso-with-apache/
wordpress_id: 316
---

The key to the magic here is the [mod_auth_kerb module](http://modauthkerb.sourceforge.net/), which adds Kerberos authentication to [Apache](http://httpd.apache.org/). This module not only allows Apache to use Kerberos on the "back-end," so to speak, but also supports the SPNEGO and GSS-API stuff on the "front-end" that allow it to transparently authenticate users connecting with supported browsers, without ever prompting for a password.

### Preparing Active Directory (Each Apache Server)

These steps need to be repeated for each Apache server that will authenticating via Kerberos to Active Directory.

1. First, create a user account (not a computer account) for each Apache server. I highly suggest using a naming convention that supports a) the service principal(s) involved; and b) the name of the server. Since Apache will use the HTTP service principal, a name like `HTTP-lnxservername` would be good. The password doesn't matter, but do be sure to check the "Password never expires" check box, and after the account is created specify a good description so that you'll remember what this account is for in 6 months.

2. For each account that was created, run the `ktpass.exe` command to generate a unique keytab for each account. The command will look something like this (substitute the appropriate values where necessary):  

``` text
ktpass -princ HTTP/fqdn@REALM -mapuser DOMAIN\account  
-crypto DES-CBC-MD5 +DesOnly -pass password -ptype KRB5_NT_PRINCIPAL  
-out filename
```

Be sure to specify a unique output filename (so that you don't overwrite files; each server/account will needs its own unique file). I suggest using the server's name as the filename, i.e., something like `lnxservername.keytab`.

It would be ideal if we could leverage the existing computer account that may exist for that Linux server for host authentication (I'm assuming you followed my instructions for integrating host authentication into Active Directory, yes?), but for some reason it doesn't work. We can use the SetSPN utility to add the appropriate SPN to the computer account, but authentication still doesn't work. If any Kerberos/Active Directory gurus out there have some insight on this, please let me know. (By the way, this may be one reason for using user accounts for all the various SPNs---HOST/fqdn@REALM, HTTP/fqdn@REALM, etc.---as some of the online guides for integrating Linux and Active Directory have suggested.)

Now we're ready to move on to configuring the Apache servers.

### Configuring Apache (Each Server)

Repeat these steps for each Apache server. In case I haven't already mentioned this, I'm assuming you're running Apache 2.0 on Linux, and not on some flavor of Windows.

1. Download and install the mod\_auth\_kerb Apache module.

2. Add the following directives to the Apache configuration, either in `httpd.conf` or in the `conf.d` directory in its own file (my installation of mod_auth_kerb created an `auth_kerb.conf` in `conf.d`). You'll need to substitute the correct values for the KrbAuthRealms directive (which should be your Active Directory domain name in UPPERCASE) and the location and name of your keytab (which you'll copy over in the next step):

    ``` apache
    LoadModule auth_kerb_module modules/mod_auth_kerb.so  

    <Location /secured>  
      AuthType Kerberos  
      AuthName "Kerberos Login"  
      KrbMethodNegotiate On  
      KrbMethodK5Passwd On  
      KrbAuthRealms EXAMPLE.COM  
      Krb5KeyTab /etc/httpd/conf/httpd.keytab  
      require valid-user  
    </Location>
    ```

3. Securely copy over the keytab for this server from the Windows server where it was generated using `ktpass.exe` earlier. SFTP or SCP are good candidates. Once the file has been copied over, rename it and place it in the right location, as specified in the configuration entered above.

4. Change the owner of the keytab to the Apache user (typically "apache" or "web"), and set the permissions to 400 (readable only by the Apache user).

5. Restart the Apache HTTP daemon for the configuration changes to be read and applied.

Assuming that your Apache server is accessible as `web.example.com`, you should now be able to fire up a recent version of Internet Explorer (one that supports Integrated Windows Authentication) and navigate to the "http://web.example.com/secured" URL and gain access, _without getting prompted for authentication._ A quick review of the access logs (typically `/var/log/httpd/access_log`) shows that you are being authenticated as the user that is currently logged on to Windows. (If the browser you are using doesn't support the transparent authentication, you'll get prompted for a username and password, in which case you can enter your Active Directory username and password and gain access to the site.)

If this doesn't work, go back and double-check your `ktpass.exe` command (noting that the case of the Kerberos principal specified by the "-princ" option is important, as it is case-sensitive). Also check the permissions on the keytab after it has been copied over to the Linux server; it must be readable by the Apache user (and should not be readable by any other users or groups). Finally, try unchecking the "Enable Integrated Windows Authentication" option in Internet Explorer, restarting IE, re-checking that box, and then restarting IE again. (Don't ask why, but it does seem to help in some instances.)

Finally, note that a few other browsers also support the transparent authentication. I personally tested [Safari](http://www.apple.com/macosx/features/safari/) and [Shiira](http://hmdt-web.net/shiira/en) on Mac OS X, and both worked fine (after I had obtained a Kerberos ticket, either using the Kerberos application or `kinit` from a shell prompt). [Camino](http://www.caminobrowser.org/) didn't work, which is a bummer. I haven't tested [Firefox](http://www.mozilla.com/firefox/) yet, but I'm told that Firefox also works, although an extension may be required.

Extensive credit goes to Achim Grolms for his [walk-through of using mod_auth_kerb with a Windows KDC](http://www.grolmsnet.de/kerbtut/).
