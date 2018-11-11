---
author: slowe
categories: Explanation
comments: true
date: 2006-08-21T15:25:01Z
slug: more-on-kerberos-authentication-against-active-directory
tags:
- ActiveDirectory
- Apache
- Interoperability
- Kerberos
- Microsoft
- Networking
- UNIX
title: More on Kerberos Authentication Against Active Directory
url: /2006/08/21/more-on-kerberos-authentication-against-active-directory/
wordpress_id: 324
---

I'll break the information down according by the article to which the information pertains. If the information pertains to all the articles equally, it is included in the "All Articles" portion. Links back to the original articles are included.

## All Articles

### User Accounts vs. Computer Accounts

One thing in common across all these articles is the need to create special accounts in Active Directory.  I originally recommended the use of computer accounts, but over the course of time additional integration tasks with Kerberos have led me to believe that the user of user accounts, _not_ computer accounts, is the best way to handle this. In a [very early Linux-AD integration article][1], I discussed the use of computer accounts instead of user accounts, and the use of the `ktpass.exe` command to extract a keytab from a computer account. At that time, it seemed reasonable to recommend computer accounts---we were only discussing a single service principal (the host itself), and using a computer account _made sense_.

However, because of the fact that additional accounts will have to be created for additional service principals (for example, if you are interested in using [Kerberos-based SSO with Apache][2]) and because you can't have multiple computer accounts with the same name, it's just as well to use a user account with a naming convention that describes the purpose of each account. For example, something like "host-fqdn" for the host/fqdn@REALM service principal (used for scenarios involving pam\_krb5 and "native" Kerberos/GSSAPI authentication), and "http-fqdn" for the HTTP/fqdn@REALM service principal (used by mod\_auth\_kerb for Apache-Kerberos integration). This provides an easy way to distinguish between these accounts, and allows for the multiple accounts that are required in these kinds of scenarios.

Finally, the use of user accounts will eliminate some spurious error messages that `ktpass.exe` generates when run against a computer account. Note that these error messages are not substantive; things will still work with a computer account. It's just that `ktpass.exe` appears to have been intended to run against a user account.

### Updated ktpass.exe Command Line

In addition, because of differences in the encryption types supported by Active Directory and many Unix/Linux implementations, I have found that using the "+DesOnly" switch with `ktpass.exe` seems to help.

An updated `ktpass.exe`, accounting for the change to a user account and the desire to use DES encryption types, would look something like this:

```text
ktpass.exe -princ host/fqdn@REALM -mapuser DOMAIN\account 
-crypto des-cbc-md5 +DesOnly -pass Password123 -ptype KRB5_NT_PRINCIPAL 
-out c:\filename.keytab
```

Please note that this command, particularly the service principal name (as specified by the "-princ" parameter) is **CASE SENSITIVE.** For example, the host SPN (i.e., "host/fqdn@REALM") that is used by pam\_krb5 and "native" Kerberos/GSSAPI authentication is lowercase. However, the HTTP SPN (used by mod\_auth\_kerb with Apache) needs to be _uppercase._ In both cases, the Kerberos realm (which should be equivalent to the Active Directory DNS domain name) must be in uppercase.

## Linux-AD Integration (Pre-R2)

The original article is found [here][3].

In addition to the switch from a computer account to a user account as described above, the only real change to this article pertains to the `/etc/ldap.conf` file that configures the nss_ldap library. Remember that in almost all of these scenarios, we are using Kerberos for authentication and LDAP for account information, i.e., storing the information that would normally be stored in `/etc/passwd`.

The original article calls for `/etc/ldap.conf` to include this line:

    nss_base_group dc=mydomain,dc=com

To help optimize LDAP traffic against Active Directory, change the `/etc/ldap.conf` as follows:

    nss_base_group dc=mydomain,dc=com?sub?
    &(objectCategory=group)(gidnumber=*)

(This should all be on a single line.) This will help reduce the query load on Active Directory by limiting the types of objects for which searches will be issued; in addition, the "objectCategory" type is indexed, which also also greatly speed operations.

Speaking of indexing, if the pam\_login\_attribute is not modified (it defaults to uid), you'll also need to index the uid attribute in order to avoid heavy CPU utilization and delays in responses from Active Directory. This was mentioned in the updated [Linux-AD R2 instructions][4]. Alternately, you can change the pam\_login_attribute in `/etc/ldap.conf` to an indexed attribute, like sAMAccountName.

Last, you may find it useful to edit `/etc/ldap.conf` and change the gecos field mapping from name to cn (i.e., use "nss\_map\_attribute gecos cn" instead of "nss\_map\_attribute gecos name").

## Linux-AD Integration (R2)

The latest version of these instructions are found [posted here][4].

Again, other than the change to a user account instead of a computer account (as described above), the changes mentioned in the previous section also apply here:

* Changing the nss\_base\_group in `/etc/ldap.conf`

* Changing the nss\_map\_attribute for gecos in `/etc/ldap.conf`

* Changing the pam\_login\_attribute to an indexed attribute in Active Directory (such as sAMAccountName) or indexing the uid attribute

Otherwise, the revised instructions are reasonably accurate and seem to work pretty well.

## Kerberos-Based SSO with Apache

This article is found [here][5], and already incorporates most of the changes mentioned in this posting. No suggested changes are needed at this time.

## Solaris 10 and AD Integration

This article can be found posted [here][6].

This one is a pretty recent article (published within the last week or so), and so already incorporates most of the changes suggested here. The information that I don't have yet, however, is exactly _how_ to incorporate some of the other suggested changes. For example, I'm not yet sure how to use the `ldapclient` command to modify the LDAP client profile (Solaris 10 doesn't use nss_ldap by default) to include the refined group query. I suspect that this command would work correctly:

    ldapclient mod -a serviceSearchDescriptor=group:dc=example,dc=com?sub
    ?&(objectCategory=group)(gidNumber=*)

Of course, the `ldapclient` command syntax would change depending upon if this was an initial setup or a change to an existing setup.

Likewise, I'm not sure which login attribute the Solaris LDAP libraries are looking for---uid, sAMAccountName, or something else entirely? If anyone with more Solaris 10 experience than me has the answers to these questions, I'd love to know.

As I continue to expand my knowledge of using Kerberos to integrate these various platforms and applications, I'll post additional information here. I know that a number of you have been posting questions in the comments for these various articles; keep the questions coming. I may not know the answer right away, but I'll sure do my best to find out!

[1]: {{< relref "2005-07-18-minor-linux-ad-hiccup-fixed-hopefully.md" >}}
[2]: {{< relref "2006-08-10-kerberos-based-sso-with-apache.md" >}}
[3]: {{< relref "2005-12-22-complete-linux-ad-authentication-details.md" >}}
[4]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
[5]: {{< relref "2006-08-10-kerberos-based-sso-with-apache.md" >}}
[6]: {{< relref "2006-08-15-solaris-10-and-active-directory-integration.md" >}}
