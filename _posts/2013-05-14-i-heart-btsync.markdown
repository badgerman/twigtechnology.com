---
layout: post
title: "BTSync, Dropbox Alternative"
date: 2013-05-14 08:19
comments: true
categories: [software, Raspberry Pi, htf]
---
Dropbox is a revolutionary product. It has made it simple for anyone
to share files between computers, access those files from a phone,
automatically load photos from my phone onto my computer, and make it
easy to share files publicly. Who needs USB sticks anymore?

## Why would I even want to quit Dropbox?

Dropbox comes with some striings attached that I don't like. There
is no client for my Raspberry Pi, for example. Every file I share is
also uploaded to the cloud, and while they score 4 out of 5 stars in
the [EFF's privacy rankings](https://www.eff.org/who-has-your-back-2013),
there is no technically enforced guarantee that my data isn't shared
with others. They have been in the news with
(security incidents)[http://www.zdnet.com/dropbox-gets-hacked-again-7000001928/] 
that make it clear files on their service are not encrypted. In fact,
by the nature of how their software works, they cannot be.

## Bittorrent Sync (BTSync)

[BTSync](http://labs.bittorrent.com/experiments/sync.html) is a software that sets up a private swarm of computers that
share folders between them, just like regular bittorrent clients share
the files defined in a torrent among them, except that changes in the
folder are synced, too. It's free, easy to set up, and secure by
design. There is no cloud server that keeps a copy of all my data, and
as long as at least one machine in the swarm is reachable, I can sync
my folder to a new machine. Folders are shared by exchanging a secret
key, a short alphanumeric string, and computers discover each other
either on the local LAN or over the internet.

BTSync can share multiple folders, anywhere in the file system (no
central Dropbox folder here), with differnt people. I can share the
baby pictures with Mom and Grandma, travel plans with the rest of my
group, music with the living room computer, and a public folder with
my web server, or any other combination.

### Example use: Hack The Future

For Hack The Future, I have a folder of all the installers for
software that we mentor. It's shared between the loaner laptops we
have, but it's easy to share that folder with somebody else, and
rather than downloading big installers over a museum's slow internet
connection, the new machine will pull in everything from the local
network, with the existing computers sharing the load.

### Example use: Raspberry Pi

I said earlier that my complaint about Dropbox was that there is no
client for my Raspberry Pi. BTSync has an ARM client, and I use it to
seed folders that I want to have access to at any time. The Pi is my
only "always on" machine, and takes over the role of the Dropbox cloud
servers.
