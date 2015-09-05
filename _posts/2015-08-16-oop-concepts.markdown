---
title: Object Oriented Programming Concepts
date: 2015-08-16
tags: object-oriented
excerpt: In this post, I will discuss Object-Oriented Programming concepts, focusing specifically on the different types of variables and the scopes in which they are accessible.
---
*Variable scope* refers to the concept of making different variables visible and
accessible at different "levels" of a program, and it is one of the most
important concepts for programmers to grasp. In this post, I will go over the
various types of variables that Ruby supports, and what their visibility within
a program is.

## Local Variables

New programmers are usually introduced to programming via *local variables*. A
local variable in Ruby is "in scope" - i.e., visible and accessible - only in
the context in which it was created and defined. In the most common case, a
local variable is defined within a method. The local variable will only be
visible within that method, but not outside of it. In Ruby, initializing a local
variable would look like this: `local_var = "Steven"`.

## Instance Variables

Ruby also supports *instance variables*. Instance variables are easily
identified by the leading "@" symbol in their names.  (For example, initializing
an instance variable would look like this: `@instance_var = "Steven"`). Instance
variables are most commonly used in order to hold data - i.e., state - about an
object.  Unlike local variables, which disappear as soon as the method in which
they were initialized is done executing, and instance variable is "attached" to
the object in which it was created and will persist until that object no longer
exists. Thus, even if an instance variable is created within a method, that
instance variable will be visible and accessible to other methods within that
object.

## Class Variables

*Class variables* are another type of variable in Ruby. Class variables are
often used to hold information that is shared across instances of a class. For
example, if we want to know how many car objects our Car class has instantiated,
that is information that would belong in a class variable. Class variables are
visible to all instances of that class, as well as to descendants of that class.
Class variables are easily identifiable because they are preceded by two "@"
symbols. For example: `@@class_var = 25`.

## Global Variables

Finally, Ruby also support *global variables*. Global variables are visible
throughout a program, and some of Ruby's internal workings make heavy use of
global variables. As the name implies, global variables are visible and
accessible everywhere within a program. Global variables are easily recognizable
by the leading "$" in their name: `$a_global = "Steven"`.

##  A Discussion on Visibility

Individuals just getting started with programming usually wonder why there is
the need to have different types of variables. Why not make all variables
*global variables*, and do away with the need to worry about scope and the
visibility of variables?

This is a good question, but there is also a good explanation as to why we need
different visibility and scopes in a program. One of the strongest reasons for
having variables with differing limits on visibility helps prevent *name
clashes*. A name clash occurs when two variables have the same name, but
(should) refer to different objects.  While knowing not to re-use a variable may
be a simple matter in a small program, this would be difficult in a larger
program. (Throw in the use of libraries, and keeping all the variable names
separate becomes a real headache).
