---
title: Hashes and Objects in Ruby and JavaScript
date: 2015-08-23
tags: javascript ruby hashes objects
excerpt: How do Ruby Hashes and JavaScript objects compare? What are the major differences and similarities?
---
More and more commonly, Ruby and JavaScript are often compared to one another.
Ruby - through Ruby on Rails - has dominated as a framework to easily build CRUD
/ database-backed applications. However, attention and [momentum has recently
shifted to the MEAN stack](http://www.infoq.com/articles/rails-to-mean-js),
which focuses on JavaScript as the underlying language. (Unlike Ruby, JavaScript
can be run on both the client- and server-side; many professionals cite the
avoidance of context-switching costs as a primary benefit of the MEAN stack).

A full-blown comparison of the two languages could certainly fill a book. In
this particular blog posts, I will compare Ruby hashes to their closes
JavaScript equivalent - objects!

## Creating Hashes in Ruby

Like most other programming tasks, there are a variety of ways to create hashes
in Ruby. Both of the lines below, for examples, would bind an empty hash to the
variable `grades` in Ruby.

~~~ruby
 grades = {}
 grades = Hash.new
~~~

### Using Hashes in Ruby

Ruby hashes are instances of the class Hash, and this means that they come
pre-loaded with a ton of methods. We can return an array of all the keys with
`Hash#keys`, or we can return an array of all the values with `Hash#values`, for
example.

## Creating Objects in JavaScript

JavaScript's Objects serve the same function as Ruby Hashes, insofar as they are
a data structure that maps keys to values. Creating a JavaScript Object is
simple:

~~~javascript
 var grades = {};
~~~

### Using Objects in JavaScript

Almost all objects in JavaScript have the Object.prototype as a prototype
somewhere in their prototype chain. This means that there are pre-built methods
that can be called on an object. However, in some cases, we have to call a
function within the Object object directly to achieve what we want. For example,
to get an array of strings representing the **enumerable** properties of an
object, we'd have to pass that object to the function `Object.keys`. For
example:

~~~javascript
  Object.keys(grades);
~~~

## Bringing It All Together

As you can see, Ruby Hashes and JavaScript Objects are not terribly different -
there are other areas were the differences between the languages are greater. In
terms of Hashes and Objects, however, the key is to remember that both Ruby and
JavaScript provide a data structure that maps keys / properties to values. The
keys in both cases can be other objects, as can the values.
