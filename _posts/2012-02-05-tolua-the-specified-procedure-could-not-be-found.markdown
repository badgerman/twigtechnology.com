---
layout: post
title: "tolua: The specified procedure could not be found."
date: 2012-02-05 13:02:00
comments: true
categories: 
---
Today I was going to make a tiny DLL that binds some C code with
tolua++, and I ran into this error message:

    lua: error loading module .foo. from file .daisy\debug\foo.dll.:

        The specified procedure could not be found.

<!-- more -->
That looks like my DLL is not exporting the luaopen_foo
function correctly. That is the function lua calls to initialize the
package it just opened, and tolua++ was not declaring it properly:

    TOLUA_API int luaopen_foo (lua_State* tolua_S) {
        return tolua_foo_open(tolua_S);
    };
  
TOLUA_API is defined toextern, which doesn.t mean that it will be
exported by the DLL, because on windows, the magic incantation for
that is__declspec(dllexport). The tolua++ header has no special
treatment for Windows, so I.m guessing that problem has just never
come up for them.

Once the problem is understood, the solution is easy: I added another
function to dllmain.cpp that has the correct export declaration and
calls the generated one.

    extern "C" {
        int tolua_bar_open(lua_State*);
        int __declspec(dllexport) luaopen_foo (lua_State* L) {
            return tolua_bar_open(L);
        };
    }
          
There is one caveat, because tolua++ has already created a
function named luaopen_foo, so I had to tell it to name things
differently, for which there is a command-line argument:

    tolua++ -n bar foo.pkg > foo.c
