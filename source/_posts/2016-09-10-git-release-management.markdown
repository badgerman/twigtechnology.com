---
layout: post
title: "Release Management and version numbering with GIT"
date: 2016-09-10 03:13
comments: true
---
## Simple version numbering

Eventually, every program you write needs a version number. When customers file bug reports, it's useful to know what version of the program they were using, for example. Here's a very simple program that does nothing but print a version number:

	#include <stdio.h>
	#define VERSION "1.0.0-beta"
    void main(void) {
		printf("foobar version %s\n", VERSION);
	}

For the longest time, this is how I managed the version numbering in [Eressea](http://www.eressea.de/), but the more I automate things, the more I realize that this is really annoying.

## Version numbers do not belong in code 

Defining the version number in the code, like we do here, causes us to have to change the code every time we build a new release, which possibly creates a new commit, merge conflicts, and a whole number of problems we don't want. So let's not do that, but define the version externally:

	#include <stdio.h>
    #ifndef VERSION
	#define VERSION "1.0.0-beta"
    #endif
    void main(void) {
		printf("version %s\n", VERSION);
	}
 
Now we can pass a -DVERSION=\"1.1.0\" argument to gcc, and this version number will override the one in the source. However, now we don't store the current version anywhere except in the final executable. This is where git's tags come in. Tags are a way of marking a version in git with a name, and picking a standard format for our releases, we can give the command `git tag -f v1.2.0` to mark our code with a version number. We can get the most recent tag with `git describe --tags`, and if the current checkout is not directly on the tag, git will add a postfix to the tag to describe the revision we are at, like so: `v1.2.0-439-gb1a8ccd` (where b1a8ccd is the SHA1 hash of the current revision). Putting it all together, we get:

	gcc main.c -DVERSION=\"$(git describe --match 'v*.*.*' --tags)\"

Depending on your build system, this belongs in your Makefile or your CMake rules.

## Results

I now have a system that automatically sets the version string of my program to something that is both descriptive and useful. Every time I make a new release, I have to tag it (my release script does this automatically). For significant releases, I still update the VERSION definition in the source code, but my own builds mostly don't care about it. 

Most importantly, every time I get a bug report that includes a version number from a build that I have made, it relates directly to a git revision. I can try to reproduce that bug with the exact code that was used to produce it in the first place!
