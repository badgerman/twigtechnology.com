---
layout: post
title: "Prefix Search, Part 2: crit-bit trees"
date: 2012-05-26 08:35:33
comments: true
categories: [kata, code, algorithms]
---
The problem with the prefix search problem I described in my previous post is
that there is most likely no good data structure in your language to do this
with. There certainly isn't one in C. But the good news is that we get to build
our own! How often do we still get to write data structures from scratch in this
day and age? And when is it ever a good idea?

<!-- more -->
Of course, you should always try to go with the simplest thing that works: In my
case, that's building an array of your million strings, and to search for a
prefix, just strncmp each entry. We'll benchmark that, see how well it performs,
and use it as our baseline for a good implementation.

Have a look at the [naive
implementation](https://github.com/badgerman/critbit/blob/master/naive.c) that I
wrote. Finding a needle in a haystack of a million strings takes almost no time
at all, but building the array takes a while, so I'll just do 1000 searches,
each for a single needle, in a set of a million strings, followed by 1000
searches for a prefix that occurs exactly 11 times:

    $ bin/naive -x 100 1m 111111
    init: 0.30s
    find: 0.82s
    $ bin/naive -x 1000 1m 111111
    init: 0.26s
    find: 7.51s

8 ms per lookup may not sound like much, but it's actually not a very good time
at all. Excellent! We like this in a naive solution that we want to beat :-)

## Introducing crit-bit trees

The data structure we want for this sort of thing is conventionally known as a
radix-tree or trie. I think that's either pronounced like **try** or
like **tree**, but either way it's really confusing. Also, it really helps
to think of the data we are operating on as a string of bits, not characters.
The specific type of trie that I decided to implement is from
[this paper](http://cr.yp.to/critbit.html) by the talented Mr. 
[djb](https://en.wikipedia.org/wiki/Daniel_J._Bernstein "Daniel J. Bernstein").

The general idea of this approach is that we have a binary tree, and if some of
our bit-strings have a common prefix x such that one string has a prefix of x0
and the other x1, then the tree shall contain a node where all the strings
beginning with x0 are found to the left of the node, and all strings starting
with x1 are found to the right.

What's neat is taht we don't need a node for every possible bit position either,
but only for those where actual strings in or dataset diverge. To give an
example, if we had only two strings 0001001 and 0100100, (these start to differ
at the second bit), then we create an internal node in my tree that contains the
bit-position, and two child pointers which point at external, or leaf, nodes
that contain our two strings. To find a string in this, we follow a trail of
internal nodes from the root, check the bit position in our needle string, and
recurse down to the respective child node. Once we reach a leaf, we compare our
needle to the string stored here, because it's possible that our needle and the
stored string differ at other bits that we did not check on our way down.

Apart from being a data structure for key lookups, an interesting property of
tries is that all entries beginning with a particular substring are children of
the same node. That makes this data structure the perfect candidate for our
problem of prefix search. So, how fast is it? Actually, it's way too fast to
measure with just 100 searches, but we can do simple divisions, so let's do a
million lookups and prefix searches instead:

    $ bin/benchmark -x 1m 1m 111111
    init: 0.62s
    find: 0.25s
    $ bin/benchmark -x 1m 1m 11111
    init: 0.66s
    find: 1.09s

250 nanoseconds per lookup, and a microsecond per prefix-search. That's a pretty
neat improvement of four orders of magnitude! In my next post, I am going to
talk a little bit about the actual implementation and interface. I have published
my source code on [github](https://github.com/badgerman/critbit).

## Some Thoughts

* My dataset is pretty small. A million keys like this can probably fit into a
  modern CPU's L2 cache if they are small enough. But trust me, the numbers are
  only getting better when the dataset grows!
* Each internal node is really small: I need two pointers, a word to store the
  bit position (my implementation is excessive and uses size_t, plus a byte to
  store a bit mask. You could go a little smaller at the cost of performance
  and/or size limitations, but it's on the order of one or two dozen bytes. All
  internal nodes are the same size, so you would want to pair this with a clever
  memory allocator.
* Initialization is somewhat slower, because creating the tree is more
  complicated than just doing a couple of allocations. However, you could
  conceivably store the final tree in a compact format on disk once it's
  generated, and load it as a big chunk of memory, where you replace a pointer
  to an internal node with an integer index into your array of internal nodes.
* The crit-bit tree also allows fast deletion, which I haven't benchmarked,
  mostly because I have nothing to compare it to.
* All times are taken with clock() on a JoyentCloud VM. I have no idea what kind
  of CPU that translates to.
