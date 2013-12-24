---
layout: post
title: "International Characters on a US Keyboard in Windows 7"
date: 2013-12-23 13:00:00
comments: true
categories: [howto, keyboard, software, windows]
published: true
---
Like many programmers, I use a US keyboard layout instead of my native German
(or Scandinavian) keyboard. This makes programming easier, because I don’t need
to put my right hand into contortions to get at the keys for angle brackets,
curly braces, or the backslash, and I am able to use my coworkers’computers and
vice versa (except for those Dvorak types). But sometimes, I want to write a
letter home, and I need the Umlauts and all that other stuff. There are four
basic solutions to this, all of which don’t work for me, and the final one that
I use today (skip right to the end if that is all you want to see).

<!-- more -->
## The Windows language bar

I could install three keyboard layouts, and switch between them with the Left
Alt + Shift macro, as the Windows designers intended. Personally, I hate this,
because it’s designed as a per-application setting, but my brain does not
remember per-application state. The Alt-Shift combo is one that I frequently hit
by accident, and the task bar indicator only shows you its state on hover, while
the floating version of the language bar is a huge eyesore.

## Key codes

Windows allows you to key in characters by their code point. Alt + 132 is &auml;, for
example. I would have to learn 14 of these three-digit combinations, and they
only work on the numeric keypad. This is a serious impediment to typing speed,
and in fact it’s easier to remember and faster to type out the HTML entity for
each of them. Most laptops, and my FILCO keyboard, do not have a numeric keypad,
so that option is out for other reasons, too.

## AllChars

For a very long time, I was a huge fan of
[AllChars](http://allchars.zwolnet.com/), which is a small program that runs in
the task bar and adds a "compose" key. So if you wanted to type an &auml;, you type
the sequence ESC &quot; a. Sadly, this little piece of open source magic stopped
working reliably with later versions of Windows, and it was causing very erratic
behavior, so I had to stop doing that.

## US-International keyboard layout

There is a special version of the US English keyboard layout called United
States-International. You can install it from the Control Panel, just like you
would install a German or Norwegian layout. In this layout, international
(mostly Western European) characters are assigned to combinations of Right Alt +
character, with special characters roughly located in the vicinity of
similar-looking ASCII characters. &Auml; is Right-Alt + Q, for example.
Alternatively, you can compose keys, because a number of keys are "dead keys":
typing ~ + n gives you &ntilde;. I don’t need those dead keys, they get in the way:
when trying to type an apostrophe, I need to type the ‘ followed by a space, or
I risk it being conjoined with the following character.

## International layout without dead keys

Microsoft has published a tool that allows anyone to edit their own keyboard
layout, and there is a kind soul who felt the same way that I did about the dead
keys, and published his solution. This is the solution I use today, [United
States-International (no dead
keys)](http://freeman2222.mywebcommunity.org/us-intnd.zip). It behaves exactly
like a US keyboard, with the exception of the Right-Alt key being a modifier to
access the international characters. I have this set as my default keyboard
layout on all my Windows computers.
