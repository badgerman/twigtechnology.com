---
layout: post
title: "Android NDK shared libraries"
date: 2012-02-26 23:11
comments: true
categories: [code, android, mobile, java]
---
This morning, I got stumped by the Java/NDK integration. I was trying to add
another shared library (Lua) to a project, and kept getting these dynamic linker
error messages:

    I/ActivityManager( 1386): Start proc org.libsdl.app for activity org.libsdl.app/.SDLActivity: pid=25826 uid=10087 gids={1015}
    W/dalvikvm(25826): Exception Ljava/lang/UnsatisfiedLinkError; thrown during Lorg/libsdl/app/SDLActivity;.&lt;clinit&gt;
    W/dalvikvm(25826): Class init failed in newInstance call (Lorg/libsdl/app/SDLActivity;)
    W/dalvikvm(25826): threadid=1: thread exiting with uncaught exception (group=0x40020ac0)
    E/AndroidRuntime(25826): FATAL EXCEPTION: main
    E/AndroidRuntime(25826): java.lang.ExceptionInInitializerError
    E/AndroidRuntime(25826):        at java.lang.Class.newInstanceImpl(Native Method)
    E/AndroidRuntime(25826):        at java.lang.Class.newInstance(Class.java:1429)
    E/AndroidRuntime(25826):        at android.app.Instrumentation.newActivity(Instrumentation.java:1021)
    E/AndroidRuntime(25826):        at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2577)
    E/AndroidRuntime(25826):        at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:2679)
    E/AndroidRuntime(25826):        at android.app.ActivityThread.access$2300(ActivityThread.java:125)
    E/AndroidRuntime(25826):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:2033)
    E/AndroidRuntime(25826):        at android.os.Handler.dispatchMessage(Handler.java:99)
    E/AndroidRuntime(25826):        at android.os.Looper.loop(Looper.java:123)
    E/AndroidRuntime(25826):        at android.app.ActivityThread.main(ActivityThread.java:4627)
    E/AndroidRuntime(25826):        at java.lang.reflect.Method.invokeNative(Native Method)
    E/AndroidRuntime(25826):        at java.lang.reflect.Method.invoke(Method.java:521)
    E/AndroidRuntime(25826):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:868)
    E/AndroidRuntime(25826):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:626)
    E/AndroidRuntime(25826):        at dalvik.system.NativeStart.main(Native Method)
    E/AndroidRuntime(25826): Caused by: java.lang.UnsatisfiedLinkError: Library main not found
    E/AndroidRuntime(25826):        at java.lang.Runtime.loadLibrary(Runtime.java:461)
    E/AndroidRuntime(25826):        at java.lang.System.loadLibrary(System.java:557)
    E/AndroidRuntime(25826):        at org.libsdl.app.SDLActivity.&lt;clinit&gt;(SDLActivity.java:53)
    E/AndroidRuntime(25826):        ... 15 more
    W/ActivityManager( 1386):   Force finishing activity org.libsdl.app/.SDLActivity

What it was was that my Activity (which is a small java stub) needed to call
System.loadLibrary("Lua"); to load that new library. How is that related to the
error message? I have no idea.

Sigh. Sometimes missing a single line will cost you half a day.
