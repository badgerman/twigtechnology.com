---
layout: post
title: "Security, Norwegian style"
date: 2015-04-10 14:00
comments: true
published: true
categories: [security, norway, software]
---
Today I had to log in to NAV’s pages to read a response to a message I
sent them. To do this, I need to log in. To log in, I need to use one
of their supported authentication mechanisms. All of them are terrible.

<!-- more -->
## Two-factor authentication (2FA)

Two-factor authentication is an important technology, and it is great
to see it get adopted more and more. Every Norwegian bank that I am
aware of uses either a key generator or a physical one-time pad.
Skandiabanken, my bank, sends me a credit-card sized card of 50 codes
every time I need a replacement. combined with a user id (my SSN, or
personnummer) and password, this lets me authenticate with their
website.

This is good practice. To authenticate a user online, one should ask
for something they have (here, their SSN), something they know (the
password), and a secure one-time code of some kind. You could send
this code by SMS to my phone, and it would still be pretty good: An
attacker must steal my username, force me to tell them my password
(assuming I did not put it on a post-it), and have access to my
one-time pad (or my phone). It is easy for me not to store all this
information in one place.

So, Norwegian banks: Have a cookie. You are doing this mostly right,
although using my SSN as a user ID is still awful, because that number
is part of identity theft 101. It is on numerous forms and letters
that I exchange with the state, and while it’s meant to be secret, it
really hardly is, especially because so many companies use it as the
private key to my user data. This is bad, and no software developer
worth their salt would ever do this, yet there are still too many
worthless software developers out there.

## The websites of NAV

Now, on to my original complaint: NAV is part of the Norwegian state.
They handle my unemployment claims and my pension, so security is
somewhat important. How do they implement two-factor authentication?

This is their login screen:

![NAV login page](http://i.imgur.com/arP5gRw.png)

Here I get a choice of four (technically five) authentication
mechanisms, all implemented by external partners.

The first one is MinID, which asks me for my SSN (bad username, again)
and a password, then sends an SMS to my registered mobile phone.
Technically, that is two-factor authentication done right, although
sending an SMS is only as safe as the phone system. Logging in with
this mechanism is considered by NAV as being too insecure to allow me
to read messages coming from NAV.

The second authentication mechanism is BankID. This is, I believe, a
proprietary protocol of BankID AS by which anyone in possession of a
Norwegian SSN can use the authentication mechanism used by their bank
of choice. It requires that I obviously have a bank account, don’t ask
how difficult it is for a foreigner who is new to the country to have
a conversation with the Norwegian state, it’s tragic. To log in, I
need a username (again, my SSN), a password (which is somehow
maintained in the BankID system), and whatever one-time code
generation mechanism my bank uses. I am sure the BankID people get
royalty payments from the Norwegian state and the banks that are out
of this world. Funny little programmer in-joke: Their marketing slogan
is currently “Now without Java”, because the system was previously
known to be a browser compatibility nightmare, and the only reason to
install Java on a Desktop.

If you thought that making the state pay a private company for
questionable authentication methods was a terrible idea, brace
yourself for the next two methods.

Mechanism number 3 is Buypass. The name tells you everything you need
to know. It requires the user to install a smart card reader on their
laptop, and to purchase a card for the price of 379.- kr (ca $50),
which is valid for 3 years. I’ve obviously never used this system, but
seeing the Java applet come up when I select it tells me most of what
I needed to know. I am certainly not paying $50 to read an email
regarding my unemployment status. I would be surprised if there isn’t
a cost to the state for implementation of this system, too!

Entry number four in this horror show is the Commfides USB stick. This
stick, again valid for just 3 years, costs the stately sum of 1,180 kr
(ca $150), and in addition to providing me with an electronic ID, it
also contains a 8 GB USB drive. And it does not work on Mac or Linux,
as their webpage informs me.

Last but not least is an option to log in with username and password.
Note that there is no second-factor authentication at all here, so I
assume I won’t be allowed to read my messages, even if I could get
this to work. To get a username, talk to a human being in the local
NAV office. The internet, everyone!

The problem of good two-factor authentication was solved many years
ago. Time-based One-time Passwords (TOTP) are widely implemented, in
fact, you may know this algorithm as “Google Authenticator”. GMail and
github are just two services implementing it that nearly every
software developer uses on a daily basis, and yet we end up with
expensive abominations like the ones listed above that provide less
security while being more annoying.

As this article suggests, I am indeed currently without employment. If
you are in a position to fix this, please talk to me about job
opportunities.
