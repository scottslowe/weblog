---
author: slowe
categories: Explanation
comments: true
date: 2016-12-21T00:00:00Z
tags:
- Linux
- CLI
- Ubuntu
- macOS
- XML
title: Opening Web Internet Location Files on Ubuntu
url: /2016/12/21/opening-webloc-files-ubuntu/
---

As part of my effort to make myself and my workflows more "cross-platform friendly," I've been revisiting certain aspects of how I do things. One of the things I'm reviewing is how I capture---and later review---posts or articles on the web. On macOS, I would run an AppleScript that generated a `.webloc` file (aka an Internet location file). This is an XML file that macOS understands. However, Linux doesn't natively understand these files, so today I came up with a solution to reading `.webloc` files with [Ubuntu][link-1] and [Firefox][link-2].

The solution to the file involves the use of `xmllint`, a tool that you can install on Ubuntu as part of the "libxml2-utils" package. Using `xmllint`, you can easily extract a single XML element from an XML file---and `.webloc` files are just XML files. For the sake of illustration, here's the contents of a `.webloc` file generated on macOS:

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>URL</key>
    <string>http://blog.fntlnz.wtf/post/systemd-nspawn/</string>
</dict>
</plist>
```

Using `xmllint`, you can extract the URL value, and then pass that value to the browser of your choice (Firefox, in my case). Here's a quick script that I concocted that will extract the URL using `xmllint`, then pass that URL to Firefox:

``` bash
#!/usr/bin/env bash

# Test to be sure user supplied a parameter; error if not
if [ $# -eq 0 ]; then
    echo "Please supply the name of a .webloc file to open"
    exit
fi

# Extract URL from webloc file
URL=$(xmllint --xpath "string(//string)" "$1")

# Open $URL in Firefox
/usr/bin/firefox $URL &
```

The trick here, apparently, is the `--xpath` parameter to `xmllint`. I don't fully understand the syntax yet, but it looks for the element named "string" (which, in the case of a `.webloc` file, is the URL of the site). If the element had been named "weburl", then the syntax would change to `--xpath "string(//weburl)"`.

Using this script, I can now easily pass in the name of a `.webloc` file created on macOS and open it in Firefox on my Ubuntu laptop. The script is also easily modified to work on macOS itself as well; just replace the line calling Firefox with the `open` command instead. (I'll leave that as an exercise for the reader.)

The next step for me is to create a `.desktop` file that references this script, then modify the Ubuntu environment so that double-clicking on a `.webloc` file invokes the script (which, in turn, invokes Firefox). Once I've figured that out, I'll post something here. Until then, enjoy!

[link-1]: http://www.ubuntu.com/
[link-2]: https://www.mozilla.org/en-US/firefox/new/
