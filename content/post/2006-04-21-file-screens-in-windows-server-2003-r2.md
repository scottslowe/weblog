---
author: slowe
categories: Explanation
comments: false
date: 2006-04-21T09:26:57Z
slug: file-screens-in-windows-server-2003-r2
tags:
- Microsoft
- Networking
- Windows
- Storage
title: File Screens in Windows Server 2003 R2
url: /2006/04/21/file-screens-in-windows-server-2003-r2/
wordpress_id: 229
---

One nice feature I've discovered in [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/default.mspx) is file screens. Now, finally, we have a way to make sure that users aren't storing the wrong types of files on our file servers.

We've all had those times when we are running low on disk space and we decide to go find out what (and who) is taking up all the available space on our file server. After searching through users' directories, we finally find that one person who has saved 6.3GB of MP3 files to the server.

With file screens, Windows administrators can now prevent that from happening. To create a file screen, you must create a file groups definition (or use an existing file groups definition). The file groups definition determines which files, according to extension, are affected by the screen. A few file groups definitions are already included with Windows Server 2003 R2. Once the file groups definition you need is ready, then you can create the file screen.

The file screen itself is applied to a specific filesystem location (like, `D:\Users`). The screen can either be active (blocks users from saving files in the selected file groups) or passive (monitors/alerts when users save files in the selected file groups). The file screen also includes functionality to notify administrators in a variety of methods (SMTP e-mail, event log, etc.) when a user attempts to save a file in the selected file groups.

While file screens are a big step forward for Windows-based file servers (administrators had to rely on third-party applications to provide this functionality), there are two key limitations I see so far:

* First, file screens are strictly extension-based. If a user changes the extension on a file, he or she can easily bypass the file screen.

* Second, file screens cannot be combined at a single filesystem location. This would be handy, for example, to block music files (active screening) but monitor video files (passive screening). At this time (as far as I am aware), this is not possible.

Still, it's a great new feature to have and can certainly help keep burgeoning storage requirements in check due to unwarranted use of server resources.
