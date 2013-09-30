---
layout: post
title: "Solved: Irfanview icons.dll on Windows 7"
date: 2013-06-30 06:47
comments: true
categories: [software, windows]
---
I have used [Irfanview](http://www.irfanview.com) as my image viewer
for many, many years, and one thing that it does well is customize the
windows explorer icons for supported file types through an icons.dll
file. There is a good explanation of how to make this work [at
emiliolab](http://emilio.fobby.net/features.php?g=1).

## Windows 7

There are two problems that I had with this explanation on Windows:

1. you cannot write to anywhere inside C:\Program Files (x86)
without administrator permissions, so 7-zip fails to extract the
icons.dll to the Plugins directory. Extract the file to the desktop
instead and copy it manually.

2. Irfanview needs administrator permissions to set the icon for a
file type (it has to write to the registry). So in order to change the
preference, start Irfanview by right-clicking it, and select "Run as
Administrator".
