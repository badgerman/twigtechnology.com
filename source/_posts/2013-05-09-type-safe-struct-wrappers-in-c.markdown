---
layout: post
title: "type-safe struct wrappers in C"
date: 2013-05-09 19:41
comments: true
categories: [code, c]
---
Lately, I find myself writing more and more code that uses void* pointers as
handles to implementation-specific data. This can get confusing when there is
more than one type of handle. Imagine we have these two types:
{% codeblock lang:c %}
typedef void *HBITMAP;
typedef void *HWINDOW;
{% endcodeblock %}
It is then possible to assign a value of type HBITMAP to a variable of type
HWINDOW, with no complaints from the compiler.
<!-- more -->
Let me show you some example code to illustrate:
{% codeblock lang:c %}
typedef void *HBITMAP;
typedef void *HWINDOW;
HWINDOW CreateWindow(void);
HBITMAP CreateBitmap(void);
void DestroyBitmap(HBITMAP);
void DestroyWindow(HWINDOW);

int main(void) {
    HWINDOW wnd = CreateWindow();
    HBITMAP bmp = CreateBitmap();
    DestroyBitmap(wnd);
    DestroyWindow(bmp);
    return 0;
}
{% endcodeblock %}
The C compiler will compile this without a warning, even though I obviously
mixed up the arguments to the Destroy calls.

The goal of today's excercise is to get a warning from the compiler. While C
does not distinguish different typedef's that map to the same internal type
as multiple types, it does recognize different structs. So we'll define our
handle-types like this:
{% codeblock lang:c %}
typedef struct { void *ptr; } HBITMAP;
typedef struct { void *ptr; } HWINDOW;
{% endcodeblock %}
With that change, these two are different types, and the compiler will give
us an error message:
    $ gcc -Wall -c types.c
    types.c: In function ‘main’:
    types.c:11: error: incompatible type for argument 1 of ‘DestroyBitmap’
    types.c:12: error: incompatible type for argument 1 of ‘DestroyWindow’
Note that these types are just as big as the void* in the struct (a struct
causes no additional memory overhead), so copying them is no less efficient.
Assignment works just like before, because C assigns structs by copying.

This elegant little epiphany was brought to me by [Kevin](http://luminance.org/)
and Imran, who is fanatic about type-safety, and was doing this to basic types
like int, float and char, to avoid accidental implicit conversion between them.
