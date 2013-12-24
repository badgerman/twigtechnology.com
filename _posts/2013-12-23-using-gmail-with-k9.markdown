---
layout: post
title: "Gmail with K-9, and an identity change"
date: 2013-12-23 19:30:00
comments: true
categories: [howto, gmail, software, email, k9, android]
published: true
---
In my previous article, I described how to set up Thunderbird for use with your
Gmail account. That solves my annoyances with the Gmail web interface, and
allows me to use PGP encryption in my emails, something I'll probably write a
post about later. However, I also have a series Android devices on which I also
read email, and the Gmail app on them has a few of the same problems, namely the
lack of encryption. How much does the NSA pay the major vendors so they don't
implement this, and make it harder on us?

In this article, I'll show you how to set up the free K-9 Mail app, and add
a new identity to it, so we can prepare our eventual exit from Gmail altogether.
You may not choose to do that yet, but I did.

<!-- more -->

## Send in the K-9 team

Before you install K-9, you should [install
APG](https://play.google.com/store/apps/details?id=org.thialfihar.android.apg).
It provides PGP encryption for Android, and for reasons I've never understood,
K-9 insists that it be installed first if you want to use both of them together.
For now, forget that you did that, I'm going to deal with encryption in a later
posting.

After that, install the K-9 Mail application on our phone. Get it from
[Google's Store](https://play.google.com/store/apps/details?id=com.fsck.k9), or
Market, or whatever your phone calls it. These app are very undemanding, and
I've installed them on anything from an old Froyo phone to the latest KitKat
tablet.

After you've installed K-9, you'll have to configure it to use your Gmail
account. As with Thunderbird, you will need to create another
[application-specific
password](https://support.google.com/accounts/answer/185833) for this if you are
using two-factor authentication. You want to use a different one, so that if you
ever lose your phone, you can just revoke the password that it used and cut off
access to your email.

From the menu, choose "add account", enter your full email address and the
password, and let the app find the incoming and outgoing server settings for
you. Just like with Thunderbird, this should happen automatically, so no
memorizing of host names or ports required. When all is done, you're prompted for
a name for the account and your own name, and taken to the account view. By
default, your account will be set up to not poll the inbox repeatedly, so you
might want to change the folder poll frequency in the settings to something like
every 15 minutes. Take a look around all the settings and consider what you want
your phone to be doing.

## New Identity

I'm ultimately trying to get out of Gmail entirely, and I have a domain that's
hosted at netbeat with a catch-all email account on their POP3 mail server. It
would be cool if I could slowly migrate to an email address on that domain, but
I don't have the patience to set up my own IMAP server for it yet.

First step in this is to configure Gmail to fetch the emails from that domain.
I've been doing this for a while now, and even though it means Google gets even
more data about me, it was worth it for their superior spam filtering.
Instruction on how to set up Gmail's Mail Fetcher can be found in Google's
support pages: [Centralize mail from different accounts with Mail
Fetcher](https://support.google.com/mail/answer/21289).

The one disadvantage of this setup is that email now takes longer to be
delivered, because on top of K-9's polling frequency, each message now has to
first go to my domain host's servers, and then gets picked up by Gmail on a
regular schedule (up to an hour), so you should take that article's advice
and set up direct forwarding, too. How you do that differs based on your domain
host, so you're on your own there. When I did it, it caused my emails to arrive
almost instantly.

Secondly, you'll want to be able to send emails through Google's servers from
your new email address. Again, this is documented in the Google support pages:
[Sending mail from a different
address](https://support.google.com/mail/answer/22370)

Once this works, you should have a drop-down in Gmail's web interface that lets
you choose which identity you want to send email from. By using your new
identity, you are slowly training everyone you communicate with that you've got
a new address - the address will get collected into their address books. their
replies will be sent to it, etc. You may also want to add a signature to your
emails that explains your new address. Just make sure not to tell the spammers
:-)

Back to K-9: In the account settings, under "Sending mail", select "Manage
Identities", and from the menu choose "New Identity" to add your alternate email
address. Then long-press on the newly created identity and choose "Move to top /
make default". K-9 will use this identity for all new messages, but in replies,
it will default to whatever address the email went to, and not switch. Don't
worry if that's confusing, you can always change whatever identity it chooses
from a drop-down at the top of the composition screen, for any message you
write.

Thunderbird has a similar mechanism for multiple identities, and you might want
to set it up there, too. Rather than describe it here, I'll just link the
[Mozilla Knowledge
Base](http://kb.mozillazine.org/Mozilla_Suite_:_FAQs_:_Mail_Aliases) and hope
you'll figure it out for yourself.
