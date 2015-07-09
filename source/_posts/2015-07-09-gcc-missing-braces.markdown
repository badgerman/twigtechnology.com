---
layout: post
title: "MSVC vs GCC: missing braces around initializer"
date: 2015-06-11 22:30
comments: true
published: true
categories: [c, software, ennodb]
---

Today my [EnnoDB build](https://travis-ci.org/badgerman/ennodb/jobs/70185586) failed, because I compile with -Werror, and both gcc and clang complain with a warning:

    ennodb.c:237:5: error: (near initialization for ‘myapp.data’) [-Werror=missing-braces]

<!-- more -->

The offending code looked something like this:

	struct { struct { void * ptr; } foo; } bar = { 0 };

Since these are two structs nested inside each other, there should be two sets of curly braces nested inside each other in the initialization, too. After all, I am not initializing foo to 0, but foo.ptr.

However, this passes compilation in Visual Studio, which is how it managed to slip into my commit. I do a lot of development and debugging in Microsoft's IDE, and use [Travis CI](https://travis-ci.org/) to keep myself from breaking the Linux build. That process works really well, and it might warrant a future blog post.

Intuitively, I would say that Microsoft is in the wrong here, but I want to be in a situation where code that compiles in Visual Studio has a high likelihood of also passing on Linux, to speed up my build cycles (having to wait for an integration build is slowing me down).

The simplest way to achieve parity would be to disable the warning in gcc and clang with -Wno-missing-braces, but that will just make the code rot over time, and make it incompatible with other compilers. My goal is to write C code that is as compiler-agnostic as I can make it, and compile anywhere with maximum warning levels enabled. So what I want is for Visual Studio to warn me. I've already got set the warning level to EnableAllWarnings (/Wall), and that doesn't help.

The next thing I did was to disable Language Extensions (/Za), but that just makes it impossible to use Microsoft's standard C library implementation, because it uses single-line comments everywhere, and the compiler does not recognize the full C11 standard. It also does not trigger a warning for this particular issue, which means Microsoft does not think of this behavior as a language extension. Wait a minute. Might be time to look at the standard.

Since reading the standard is somewhat like poking my eyes out, I did a Google search first, and came across [this discussion](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=25137) in the GCC bugzilla. It looks like GCC is planning to remove -Wmissing-braces from -Wall for future versions, because it isn't actually wrong, it's just easy to make certain classes of mistakes when it's disabled. It certainly wasn't pointing out an error in my code here.

So maybe I need to get off my high horse and not use the full set of -Wall? On the other hand, my own aesthetic sense says the braces ought to be there, and I'd rather deal with a failed build every once in a while than let unclean code like this slip into my project. In the end, I've decided to leave things as they were, but I'm at least considering putting in the work to enable /Za in Visual Studio.

I wonder what the chances are of getting Microsoft to react to a request for a new warning. Probably close to zero, as it would break existing projects that compile with /Wall. :-(
