---
author: slowe
categories: Tutorial
comments: true
date: 2017-02-09T00:00:00Z
tags:
- Linux
- CLI
- Fedora
title: Installing Sublime Text 3 on Fedora 25
url: /2017/02/09/installing-sublime-text-3-on-fedora-25/
---

[Sublime Text][link-1] is my current text editor of choice. I won't go into why I chose it over other tools; instead, I encourage you to have a look for yourself. Installing Sublime Text 3 (ST3) on [Fedora][link-2] 25, though, isn't as simple as running a `dnf install`. Fortunately, it's not a difficult process, but it is a process I wanted to document here for the sake of others.

Here's the process I followed:

1. Download the latest tarball of ST3. As of this writing, it was build 3126, so this cURL command accomplishes what you need:

        curl -LO https://download.sublimetext.com/sublime_text_3_build_3126_x64.tar.bz2

    As build numbers change, though, you'll want to verify the correct URL for the latest build. (A lot of sites I saw provide hard-coded scripts that help perform this process for you, but don't account for changes in the download URL.)

2. Extract the contents of the tarball with `tar xvjf sublime_text_3_build_3126_x64.tar.bz2`. This will create a directory called "sublime_text_3" with the contents of the tarball.

3. Install the desktop launcher for ST3 by copying over the `.desktop` file in the tarball:

        sudo cp -rf sublime_text_3/sublime_text.desktop /usr/share/applications/sublime_text.desktop

4. Edit the desktop launcher to specify the full path to the ST3 icon. You can use a GUI text editor, `vi`, `sed`, or whatever tool you'd like. The path to the icon will be `/opt/sublime_text/Icon/128x128/sublime-text.png`, and you'll want to specify this on the "Icon=" line of the desktop launcher.

5. Next, move the "sublime_text_3" directory created by extracting the tarball in step 2 to it's final location. I chose `/opt`:

        sudo mv sublime_text_3 /opt/sublime_text

    If you decide to put it somewhere else, be sure to repeat step 4 and adjust the path to the icon accordingly.

6. Create a symbolic link to make it easier to launch ST3:

        sudo ln -s /opt/sublime_text/sublime_text /usr/bin/subl

And that's it. You can now launch ST3 from the Fedora Overview screen, or by running `subl` in your terminal. (Side note: I have noted that running `subl` without first launching ST3 via the GUI creates an additional icon in the Dash. This may not be a big deal for you. If you don't like that, launch ST3 first via the GUI, then use `subl` all you like. Or, use the fix described in [this post][xref-1].)

Enjoy!



[link-1]: http://www.sublimetext.com/
[link-2]: https://getfedora.org/
[xref-1]: {{< relref "2017-02-16-fixing-double-sublime-text-icons-on-fedora-25.md" >}}
