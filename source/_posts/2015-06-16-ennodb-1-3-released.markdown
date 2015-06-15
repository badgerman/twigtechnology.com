---
layout: post
title: "EnnoDB 1.3 released"
date: 2015-06-11 22:30
comments: true
published: true
categories: [software, ennodb, fastcgi]
---

EnnoDB 1.3 fixes a critical bug and introduces several new convenience features for administrators.

* bug: when updating an existing key, the in-memory database was corrupted (aka the catbug).
* configuration through /etc/ennodb/ennodb.ini
* added a read-only mode in which no new log entries are written
* added -v option to print version number
<!-- more -->

## server migration

Due to new hardware getting delivered last week, I was forced to move the public instance at http://enno.kn-bremen.de/ to a new server. This required some downtime during which the service was unavailable. Migrations will be easier in the future, the process is now:

1. on the source host, set `readonly=1` in /etc/ennodb/ennodb.ini
2. `sudo service ennodb reload` to signal the change to the service
3. `curl http://localhost/ennodb/foo` to activate the change.
4. rsync the database to the destination host
5. on the destination host: `service ennodb start`
6. point the load balancer at the destination host IP
7. shut down the source host. 

This replaces the window of no availability with a short window of read-only access.

## The catbug

All software has bugs. Believing that there aren't any in what are currently 2002 lines of C code is hubris, and I didn't expect version 1.0 to stand up to actual use for very long. Judging by my server logs it took less than a day for some enterprising soul from the internet to find out that setting a value twice would cause corruption of unrelated keys (setting "catz" twice caused the loss of the "cat" key, which is how this bug got its name).

I took this opportunity to create a better test framework, and narrow the issue down from an integration test to a unit test that in the end was relatively easy to fix, but needed a change to my crit-bit tree library.
