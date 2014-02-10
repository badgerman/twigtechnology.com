---
layout: post
title: "Code Kata: Unrolled Linked List in C"
date: 2011-04-24 06:00:06
comments: true
categories: [code, kata, algorithms]
---

The other day I needed a linked list for the umpteenth time, and
instead of going with the old (data, next) pairs, I decided I wanted
something a bit more efficient, like an [unrolled
linked list][http://en.wikipedia.org/wiki/Unrolled_linked_list
"Wikipedia"). This also provided a good opportunity to use the
CuTest unit testing framework and do some test-first
development.

<!-- more -->
The result is rather nice, testing actually found a small bug,
despite the fact that I was sure can do linked lists in my sleep, and
I was so pleased with the performance characteristics (better cache
efficiency and far fewer allocations) that I replaced all the lists in
Eressea with it.

## Code Sample

{% codeblock lang:c %}
quicklist *q, *ql = 0;
int i;
// insert element at index:
ql_insert(ql, 0, foo);
ql_insert(ql, 0, bar);
assert(ql_get(ql, 1)==foo);
assert(ql_length(ql)==2);
// push element at end, get indexed element:
ql_push(ql, baz);
assert(ql_get(ql, 2)==baz);
// iterate over list:
for (q=ql,i=0;q;ql_advance(&amp;ql, &amp;i, 1)) {
    printf("%p ", ql_get(q, i));
}
{% endcodeblock %}
The code is on [github](https://github.com/badgerman/quicklist) for you to
use as you please.
