---
layout: post
title: "Using Gmail with Thunderbird"
date: 2013-12-23 13:00:00
comments: true
categories: [howto, gmail, software, email, thunderbird]
published: true
---
I have had it with Gmail. The web interface is becoming more and more
antagonistic every day, and it is my firmly help belief that Gmail is not email.
Back in the days, Gmail was a standards-compliant service built on IMAP and SMTP,
with a fancy web interface. A web interface that eventually became so usable, it
convinced me that I did not actually need a desktop email application any longer.
Those days are over. The new Compose, the inability to use it with a PGP browser
extension, the G+ integration, the hopeless top-posting and that new thing where
every attachment gets stored into Drive are all making me harken back to a
simpler time.

<!-- more -->
## Why stick with Gmail at all?

It is tempting to make a radical departure and just set up my own mail service
on my own domain. And one day, that's what I intend to do, but not everyone can
do that, and I believe in taking incremental steps in the right direction. So
the first thing I'm doing is getting rid of Gmail the web application, but I am
keeping Gmail the IMAP/SMTP service. I have had this email address for the last
10 years (I signed up November 11, 2004), and it's become impossible to just
stop using it overnight.

## Thunderbird is still the best.

Unlike Microsoft and Google, the Mozilla Foundation seems to truly believe in
Internet standards and extensibility, and Thunderbird is still the best desktop
email software. It also exists on every major platform, so if I ever decide to
abandon Windows, it will still be wherever I go.

There is at this time no Thunderbird for Android or iOS, and I doubt that this
will happen anytime soon. However, other email software exists, and the Android
Gmail app is not as awful as the web application, so the mental trauma is not
quite as high.

## Thunderbird Setup

Modern versions of Thunderbird (I am running 24.2) recognize a Gmail address in
their account wizard and they will configure the correct IMAP and SMTP server
for you, with the correct SSL/TLS settings. If you are using two-factor
authentication (you should!), then this is one of the cases where you need to
use an
[application-specific password](https://support.google.com/accounts/answer/185833?hl=en) to access
your Google account. Tell Thunderbird to remember it. You are encrypting your
harddrive, right?

Next, we need to do some configuration that Thunderbird doesn't get right, in
order to work around some behavior of Gmail thatisn't like other IMAP servers.
Go to the newly created account settings, and configure the following sections:

### Server Settings

Gmail calls its IMAP folders "labels", and there are two kinds, the ones that
you defined, and the canonical ones, like Drafts, Trash, Sent Mail, etc. These
canonical folders all live under the *Gmail* folder of the IMAP folder
hierarchy. In this next step, we want to prevent Thunderbird from creating its
own copy of those canonical folders, and tell it to use the ones that Gmail
provides.

    When I delete a message: (*) Just mark it as deleted

The GMail server automatically manages its internal Trash folder, and you don't
need to explicitly move deleted messages there.

### Copies &amp; Folders

    When sending messages, automatically [ ] Place a copy in:
    
Leave this unchecked. GMail's servers will automatically place a copy of every
outgoing message into the *Sent Mail* folder, and making another copy will just
create duplicates, and it really messes up the witless Gmail web interface.

    Message Archives [ ] Keep messages archived in:

Leave this unchecked, too. Google's equivalent of the Archive folder is the All
Mail label, which stores all Mail that you've ever seen, and Thunderbird's
"Archive"command seems to be doing the right thing (as in, something other than
the "Delete" command).

    (*) Keep message drafts in: (*) Other:

We want to keep drafts in the Gmail Draft folder, not on a local computer, so
from the "Other" drop-down, choose your Gmail account, and then the Gmail/Drafts
folder.

    (*) Keep message templates in: (*) "Templates" folder in Local Folders

I must admit I've never used Templates. There is probably an argument for
storing them on your Gmail account, so they are shared between all your
installations, but Gmail doesn't have the concept of Templates, so you'd have to
create a custom label for them.

### Composition &amp; Addressing

    [ ] Compose Messages in HTML format
    
This is where it all began. People flaunting the internet mail standards. Do us
all a favor and don't perpetuate that nonsense. You'll also want to disable this
if you have installed PGP encryption, because those two don't play well together.

### Junk Settings

    [ ] Enable adaptive junk controls for this account

We should not be needing spam filters, because Gmail is doing a pretty awesome
job at filtering out junk email. It's one of the major reasons why people love
it so much. Thunderbird's junk filters do not integrate with the server-side
filtering of Gmail, so it's better to leave everything in this section
unchecked.

