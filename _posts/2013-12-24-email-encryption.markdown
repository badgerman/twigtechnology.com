---
layout: post
date: 2013-12-24 10:00:00
comments: true
published: false
categories: [howto, software, email, crypto, android, privacy]
tags: [gmail, thunderbird, k9, pgp, gnupg, apg]
title: "E-Mail Encryption"
---

Here is a depressing thought: public-key encryption was around when I was in
college in the mid-90's, and it's still not widely adopted. Instead, it's
considered "too complicated for regular users", and none of the major vendors 
of email solutions has made an attempt to integrate encryption into their
software.

As long as the NSA and Google read every one of my emails, I want to have a way
of preserving my privacy. I'm writing these instructions in the hope that some
of the people I communicate will find it easier to use encryption when 
communicating with me.

<!-- more -->

## Using GnuPG with Thunderbird 

PGP, or "Pretty Good Privacy", is a standard for email encryption developed in
1991, and remains the de-facto standard until today. The most popular 
implementation of the PGP standard is the Gnu Privacy Guard (commonly
abbreviated as GnuPG or GPG). On your computer, you will need to install a [GnuPG download](http://www.gnupg.org/) that matches your operating system.

To integrate GnuPG into Thunderbird, you will need to install the [Enigmail extension](https://www.enigmail.net/).

## Using APG on Android

On Android, PGP is implemented in the [APG App](https://play.google.com/store/apps/details?id=org.thialfihar.android.apg) that I mentioned in my previous posting.

## Inline PGP vs. PGP/MIME
