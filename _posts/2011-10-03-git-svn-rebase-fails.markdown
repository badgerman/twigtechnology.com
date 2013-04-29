---
layout: post
title: "git svn rebase fails, is not using plink"
date: 2011-10-03 04:03
comments: true
categories: [git, software, howto]
---

I installed a new version of msysgit some days ago, and git svn rebase stopped
working. Some digging showed that it was not using plink to talk to putty anymore,
but using openssh. This despite my having told the git installer that I would
like to use plink.exe (and pageant).

<!-- more -->
What was going on? git was not to blame here - it was svn that was using plink. I
had also reinstalled msys, and with it a new subversion config file, it seems.
The fix was to edit my C:\Users\erehling.subversion\config file like this:

    [tunnels]
    ssh="C:/Program Files (x86)/PuTTY/plink.exe"

Next time this happens, I hope I'll remember that I wrote this little reminder
for myself :-)

