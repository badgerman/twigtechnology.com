---
layout: post
title: "Announcing EnnoDB"
date: 2015-06-11 22:30
comments: true
published: true
categories: [c, software, ennodb, fastcgi]
---

Today I am announcing the first official version of EnnoDB, a new
entry in the busy field of NoSQL databases. It's goal is not to be a
tool for Big Data problems, but more for the Medium to Small Data that
fits into memory.

I originally wrote this software as an exercise, but it got out of
hand and became actually useful. In other words, you may want to think
twice before using this with mission-critical data. I wouldn't, and I
wrote the thing!

Source code is available on
[github](https://github.com/badgerman/ennodb) under a BSD style
license.

## Core Features 

* fast, in-memory database
* fastcgi interface
* accessible through HTTP API
* journaled database

EnnoDB is fast because it keeps the entire database in memory. It uses
djb's [critbit trees](http://cr.yp.to/critbit.html) as its internal
data structure, which means that even though it only handles a single
request at a time, each request is extremely short.

## Example Usage

I set up a quick JavaScript example that reads and writes values from
the database running on my home server, at
[http://enno.kn-bremen.de/keyval.html](http://enno.kn-bremen.de/keyval.html).

Much like the now defunct openkeyval.org, this means all keys in this
example database are world writable. Though you could use this
instance of EnnoDB for your own web projects, I do not recommend that
anyone store any data here that they rely on.

## Acknowledgements

This project would not have happened without the inspiration from
[openkeyval](https://github.com/shinypb/openkeyval). 

[Daniel J Bernstein](http://cr.yp.to/djb.html) published the paper
about the critbit tree data structure that makes all this happen so
quickly. 

[Katelyn Gadd](http://luminance.org/) kept nagging me to finish this,
when it was still just a pipe dream.

The [Raspberry Pi](https://www.raspberrypi.org/) that I used for
developing and running the server is a fantastic little machine, and
its specs encouraged the compact, memory-efficent design.

## History

First of all, yes, the name is a pun. We were converting database
tables from myISAM to InnoDB at IMVU one day, and I kept hearing my
name come up in conversations around me, only to realize that it was
not a mispronunciation of Enno (which I am used to), but the name of a
MySQL table format. And so I made the joke that, if I were to one day
create my own database, it would have to be called EnnoDB, as payback.

Some time later, I was reading djb's paper on critbit tries, and
decided to implement [my own C
version](https://github.com/badgerman/critbit) of his idea for fun.
This worked out well, and in doing so I realized that it could not
just be used for fast prefix searches, but fast string lookups in
general. Anyone who ever wrote a std::map<std::string, int> and
cringed when they thought of what was really going on when they looked
up a customer id by name will understand why that was an exciting
prospect.

These two ideas together, that I should some day write my own
database, and that I knew how to do fast key-value lookups, combined
when I learned about FastCGI. During a period of low-intensity work I
sat down to teach myself libfcgi so I could put it all together, and
here we are.
