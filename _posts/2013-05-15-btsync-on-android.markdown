---
layout: post
title: "Bittorrent Sync on Android"
date: 2013-05-15 17:47
comments: true
categories: [software, android]
---
In [my previous blog post](http://www.twigtechnology.com/blog/2013/05/14/i-heart-btsync/),
I raved about how much I love Bittorrent Sync (BTSync). And while it
is true that for most of my use-cases, it is superior to Dropbox,
there are a few things Dropbox does that BTSync doesn't do out of the
box. Two of these use-cases are related to mobile devices: I want to share
folders with my phone, and synchronize photos taken with the phone to my PC.

## Sharing Folders with my Phone

There are [inofficial ways](https://matt.bionicmessage.net/blog/2013/04/27/Running%20BitTorrent%20Sync%20on%20your%20(rooted)%20Android%20device)
of getting btssync to run on Android devices, but I'm trying to solve
this problem without rooting the phone, and anyhow, the bittorrent
protocol would be insane to run on my data plan. My current solution is
a hybrid: I am creating a *Phone* folder in my Dropbox that gets synced
with my phone through the Dropbox app. This obviously means that a copy
has to live in the cloud, and I lose the security benefits I just gained.
Then I add that folder on my PC's Dropbox to the folders synchronized
with BTSync to share it across all my PCs, and anything I drop into
the folder on any PC, even one that does not have Dropbox installed,
is transferred to the phone.

In my setup, I have a Desktop PC that is (almost) always on, a Laptop
that doesn't have Dropbox installed, and an Android phone runnign the
Dropbox app. This is what is going on:
{% img left /images/btsyncdb.png 594 514 %}

## Synchronizing new Photos to my PC

This is almost exactly the inverse problem, and has the same solution:
When I take a picture on my phone, the Dropbox app has an option to
automatically add that picture to a "Camera Uploads" folder. This is
of course synchronized with the dropbox servers in the cloud and my
Desktop PC, where I share the Camera Uploads folder through BTSync,
and that way, I get it on my Laptop, too.
