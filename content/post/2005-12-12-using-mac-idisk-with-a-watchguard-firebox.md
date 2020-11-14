---
author: slowe
categories: Explanation
comments: false
date: 2005-12-12T09:37:38Z
slug: using-mac-idisk-with-a-watchguard-firebox
tags:
- macOS
- Networking
- Security
- Interoperability
title: Using .Mac iDisk with a WatchGuard Firebox
url: /2005/12/12/using-mac-idisk-with-a-watchguard-firebox/
wordpress_id: 139
---

One of the strengths of the [WatchGuard](http://www.watchguard.com/) [Firebox](http://www.watchguard.com/products/appliances.asp) series of firewalls is the inclusion of application-layer proxies for protocols such as SMTP, FTP, and HTTP. Whereas ordinary packet filtering firewalls and stateful inspection firewalls only control traffic based on IP address or TCP/UDP port number, application-layer proxies like those included in the Firebox look deeper into the packet to make decisions on information such as URL or MIME content type (for an HTTP proxy, for example).

However, this advantage also becomes a disadvantage as many application-layer proxies may not understand or be compatible with protocol-specific extensions. In this particular case, it's the iDisk service provided by Apple's [.Mac](http://www.mac.com/) subscription service. The iDisk service uses [WebDAV](https://en.wikipedia.org/wiki/WebDAV), an extension of the HTTP/1.1 protocol specification. It appears that, in general, most WebDAV services operate properly through the Firebox's HTTP proxy (although some issues with Microsoft Exchange's Outlook Web Access were reported with earlier versions of the WatchGuard Firebox System software).

The iDisk service, though, apparently uses some non-RFC-compliant headers (note that this was _not_ confirmed with a packet capture). As a result, the Proxied-HTTP service (which employs, as the name suggests, an application-layer HTTP proxy) prevents these headers from being passed and therefore prevents iDisk from working.

To make iDisk work from behind a WatchGuard Firebox firewall, one of several steps must be taken:

* Use the Filtered-HTTP service, which does not employ an application-layer HTTP proxy and therefore would not interfere with iDisk or other WebDAV-based applications or services.

* Modify the default properties of the Proxied-HTTP service to allow unknown headers.

Since the Proxied-HTTP service also provides for content filtering and reporting, it is generally preferred to use the HTTP proxy instead of an ordinary HTTP packet filter. The first option is therefore not the ideal solution.

Fortunately, the behavior of the Proxied-HTTP service is somewhat configurable. In order to make iDisk work with the HTTP proxy, the "Remove unknown headers" option must be unchecked in the configuration for the HTTP proxy. By unchecking the "Remove unknown headers" option, the Firebox will allow non-RFC-compliant HTTP headers to pass through the firewall, and iDisk will work correctly.
