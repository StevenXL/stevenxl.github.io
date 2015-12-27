---
title: Lessons from Ruby Koans - Symbols
date: 2015-12-12
tags: ruby koans symbols
excerpt: A list of interesting findings as I work through the Ruby Koans, this time focusing on symbols!
---
This is a continuation of my posts on the interesting bits of knowledge I'm
gaining as I work through the [Ruby Koans](http://rubykoans.com/). You can find
the [original post](http://stevenleiva.com/ruby-koans-arrays/) to get a better
sense of why I started this series.

## Symbols
Symbols in Ruby have two very important properties.

First, symbols are **immutable**. You can't add, change, or subtract from a
symbol. (This is one of the few times where the saying "it is what it is"
actually makes sense).

Secondly, symbols are **unique**. The symbol `:abc` refer to the *exacty* same
object regardless of where you see it in a program. This is very different than
strings, where the string `"hello"` and `"hello"` are two *different* objects
which happen to have the same content.

## Ruby Converts Names Into Symbols
Anywhere that you create a name - be it a class, a method, a module, or any of
the many types of variables - Ruby will add that name, *as a symbol* to its
internal **symbol table**. You can see what symbols are in this table by using
`Symbol.all_symbols`.

## Symbols Are Not Symbolic
Despite their name, symbols don't represent anything. They are not symbolic. For
example, if we type `name = "Steven", the variable `name` is a symbol, in the
generic sense, for the string `"Steven"`. However, the symbol `:name` doesn't
refer to another object - it is not symbolic or a stand-in for anything else.
