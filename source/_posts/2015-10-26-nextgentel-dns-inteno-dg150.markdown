---
layout: post
title: "NextGenTel Inteno DG150 DNS settings"
date: 2015-10-26 19:30
comments: true
published: true
categories: [hardware, networking]
---

Lately, my internet has been hardly working at all. I finally traced
this down to the Inteno DG150 router that NextGenTel provides to its
customers. It seems that occasionally, the DNS server in that box just
dies and doesn’t restart. I have an additional wireless AP, two
laptops, a gaming PC, two servers, a phone and a tablet, plus
occasional guests hooked up to it, so I may be stressing this thing
more than the average user, but restarting it twice a day was getting
old, so I had to look for solutions.

<!-- more -->

![little guy is pretty busy](http://i.imgur.com/9ITj77Y.gif)

# The Problem
Here’s what was happening: All the computers in my network receive
their IP address and DNS information through DHCP from the router. The
router itself has address 10.0.0.1, and all other devices in my home
are of the form 10.0.0.*. The DHCP information tells all my devices to
use 10.0.0.1 as the sole DNS server, which means the router is
probably relaying DNS for my ISP. In the router’s web interface, I
found the NextGenTel DNS servers configured, and I made sure that they
were working, but the one on the router was not:

![nslookup results](https://i.imgur.com/4BhOwtc.png)

The best solution would be a fix for the router, but I am already running the latest firmware. I could call NextGenTel and ask them to replace it, but they have already done this three times in the past, and every one of these boxes has been broken in one way or another, so I’m helping myself. I am also not buying my own DSL router, because who wants to invest in that kind of ancient technology anymore? It’s bad enough that I can’t get fiber to my house.

After a few false starts, and manually configuring all my devices to use Google DNS instead of the one assigned by DHCP, I finally found the option in the router firmware that let me configure what server it announces. I am pretty sure I had that configured before, but a factory reset probably killed it? I’ll have to keep an eye on it. Anyway, here’s what to do:

# The Solution

Log into the web interface at [http://10.0.0.1/](http://10.0.0.1/) with username “user” and password “user”. I know, right? This is what goes for security in that business, I am so ashamed of my profession. I haven’t found a way to change those accounts, either (but I found there is another one for admin, and one for support, I suppose those are used by NextGenTel staff to diagnose your router remotely?)
In the router’s web interface, select Advanced Setup and LAN, and enter the Google DNS servers in the DHCP information (8.8.8.8 and 8.8.4.4 respectively), like this:

![Google to the rescue](https://i.imgur.com/jqzbiEK.png)

You may have to force your devices to get a new DHCP lease, or wait until it expires (4 hours seems to be the default), and voila! you’ve got working Internet!

## Side effects
The Google DNS servers are really fast, but average ping to them is still a tiny bit slower than to NextGenTel. But without the additional cost of the DNS forwarding going on inside the very underpowered router, they may even work out to be faster.
The third positive effect of this setup is that The Pirate Bay started working again. Norwegian ISPs are required by new censorship laws to DNS-block certain domains, but Google’s DNS servers are under no such obligation.

# Summary

What have we learned from this exercise, other than “DNS censorship doesn’t work”? The DSL routers provided by major ISPs are usually terrible pieces of plastic with some of the worst software in the world on them. It pays to take a look at their capabilities, though, and you’ll be able to fix some annoyances yourself.
