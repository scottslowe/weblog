---
author: slowe
categories: Information
comments: true
date: 2008-03-17T20:30:14Z
slug: old-netware-integration-notes
tags:
- Interoperability
- Novell
- SSH
title: Old NetWare Integration Notes
url: /2008/03/17/old-netware-integration-notes/
wordpress_id: 663
---

I'm posting this stuff here on the off chance that it someday might be useful to someone out there somewhere. About four years ago, I had a wild hunch to start learning Novell NetWare 6.5, and to perform some integration testing with some other technologies with which I was already familiar. Along the way, I gathered these notes. I make no warranties about the accuracy, validity, or relevance of this information; I'm just publishing it here in case it may prove useful later. (You never know.)

So, that being said, here are the notes:

* _SSH "shell" access to NetWare 6.5 server:_ The SSHD NetWare Loadable Module (NLM) had to be loaded first. Attempts to login failed; the sshd_config file had to be edited and a Novell-specific directive (eDirNameContext) had to be modified in order to add the context where the admin account was stored (in this case, OU=Users.O=Company). After the configuration file was modified and the SSHD NLM unloaded and loaded again (to reflect the changes to the configuration file), logins via SSH were successful. (Note: It appears that NetWare 6.5 does not support the Blowfish-CBC cipher.)

* _SFTP access to NetWare 6.5 server:_ After successful SSH "shell" access (see previous bullet point), SFTP access also worked correctly. Tests using Fugu (a native Mac OS X SFTP application) were successful and without any major events or problems. In fact, SFTP was used to transfer the files necessary for the VNC testing (see next bullet point) to the NetWare server.

* _VNC access to NetWare 6.5 console:_ Using SFTP, a VNC server NLM was copied to the server. After setting the VNC password (using VNCPASS.NLM) and loading the VNC server (VNCSRV.NLM), access to the NetWare servers GUI via VNC was successful. The VNC client used was Chicken of the VNC, a freeware Mac OS X VNC client. Performance was on par for LAN access to a server.

* _Native file access from Mac OS X:_ As indicated in several online sources, the AFPTCP NLM had to be unloaded and then reloaded with the CLEARTEXT option. Then the SYS volume on the server could be mounted using the Go To Server command. After an initial login, the AFPTCP NLM was unloaded and reloaded without the CLEARTEXT option, and everything continued to work just fine.

* _Rconsole access from Mac OS X:_ Using RconJ, a Java-based port of Rconsole to Mac OS X, Rconsole access was successful. The RCONAG6 NLM had to be loaded first on the server in order for this to work.

* _VNC inside SSH tunnel:_ Creating SSH tunnels (using the L switch) works in NetWare 6.5 just as it does with Linux or OpenBSD. Using the VNC NLM discussed earlier and an SSH tunnel, the VNC traffic was secured and encrypted across the wire. This worked exactly as expected.

* _Native file access from Windows XP:_ Initial attempts to access the server from a Windows XP system failed (authentication problems). The NDS user object had been created in iManager and a simple password had also been created in iManager as well (necessary before CIFS will work). However, the `cifsctxs.cfg` file (that specifies contexts) had not been updated with the correct context (OU=Users.O=Company, which is where all user objects are stored). After modifying this file and reloading CIFS, then access from Windows XP still failed (network path not found). Further tests showed that typing the UNC path from the Run command on the Start menu failed, but browsing through My Network Places or typing the UNC path including a share name worked just fine.

* _NTP on NetWare 6.5:_ XNTPD.NLM is an NTP daemon for NetWare, similar in implementation and purpose as NTP on Linux or OpenBSD. Upon editing the NTP.CONF file in SYS:\ETC, XNTPD could be loaded only after TIMESYNC.NLM was unloaded. Even then, XNTPD seemed to unload occasionally and without reason, and the NTPDATE utility had to be used to manually synchronize the time.

* _Autoloading specific NLMs on startup:_ Upon reboot, the VNC, SSH, and Rconsole NLMs weren't loaded, and so the server was inaccessible except from the console. Using the `rconag6 encrypt` command, a `LDRCONAG.NCF` file was created with an encrypted Rconsole password. Then, `AUTOEXEC.NCF` was edited to reference this file (in order to load the Rconsole agent) as well as the SSH and VNC NLMs. This would ensure that the necessary NLMs were loaded every time the server booted.

* _Universal passwords:_ After some difficulty mounting a volume from Mac OS X, setting passwords, and such, the server was rebooted and Universal Passwords were enabled for the Users.Company container. The passwords were then set for various accounts. Following that, native file access from both Mac OS X and Windows XP (with one caveat; see below) worked flawlessly. The caveat for Windows XP native file access is that browsing shares using just the server name in the UNC path does not work; at least one share name must also be included (i.e., `\vsninteg` does not work, but `\vsninteg\sys` works just fine). SSH access worked fine after enabling universal passwords. SFTP access worked fine as well, as long as the user logging in had sufficient permissions.

OK, there you go. Here's hoping it may prove useful to someone. Feel free to correct me, clarify these notes, or just tell me I'm crazy in the comments below.
