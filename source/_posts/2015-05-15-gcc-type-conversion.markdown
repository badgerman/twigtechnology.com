---
layout: post
title: "Floating point conversion rules"
date: 2015-05-15 21:00
comments: true
published: false
categories: [c, eressea, software]
---

We try to support a wide variety of compilers for Eressea, but in practice either gcc or Visual Studio see the most use by our developers.

As good C programmers, we have enabled all warnings in Visual Studio (/Wall), but because we check our github pull requests with Travis, we depend on gcc and clang to find any errors, and would like the Travis build to warn on a super-set of the warnings that Visual Studio generates so as not to inadvertently break compatibility.

<!-- more -->
## Implicit type conversions are evil 

* unsigned/signed comparison
* avoiding float
