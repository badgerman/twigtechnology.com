---
layout: post
title: "SDL on Android"
date: 2012-02-20 22:00
comments: true
categories: [code, sdl, android, mobile]
---
Here's a quick summary of how I got SDL to work on Android. First off, this is
my setup:

* Android NDK r6
* Android SDK r12
* SDL-2.0.0-6284

The first piece of bad news is that Android support is only in the upcoming SDL
2.0, not in the current stable 1.2. Although I'm sure it could be backported,
SDL2 is pretty similar, so I decided to upgrade. The README.android file does a
pretty good job of explaining the steps, but there were a few kinks:

1. in jni/src/Android.mk, the line LOCAL_SHARED_LIBRARIES := SDL is wrong. That
   needs to be SDL2
2. Java didn't like that all the classes in src/org/libsdl/app/SDLActivity.java
   are in one file. I had to make separate SDLMain.java and SDLSurface.java files.
3. In the onTouch method, I get a compile error (cannot find symbol)
   for event.getActionIndex() and event.getActionMasked(), so I commented out some
   of that code and haven't used touch events yet. I still need to crack this nut.
4. Because my SDK is installed in C:\Google\android-sdk-windows, I had to edit
   local.properties to say `sdk.dir=/Google/android-sdk-windows`

Now I have one of the SDL test app running on my phone, drawing rectangles. Time
to see if I can do something useful with that!

{% img http://25.media.tumblr.com/tumblr_lzpph2hil01qhsievo1_1280.jpg "Not bad for a single morning. This is what holidays are for!" "Screenshot" %}
