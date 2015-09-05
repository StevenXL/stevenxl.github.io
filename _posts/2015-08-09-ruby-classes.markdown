---
title: Ruby Classes
date: 2015-08-09
tags: ruby classes
excerpt: In this post, I discuss Ruby classes. In particular, I will go over how to define a class, how the initialize method works, and how to use instance variables and methods.
---
In this blog post, I will discuss Ruby classes. In particular, I will discuss
what classes are used for, how to define a Ruby class, how the **initialize**
method works, and how to use instance variables and instance methods.

## What Are Ruby Classes

Ruby is an **object-oriented language**. What this means is that Ruby views the
world as containing **objects**. These objects have **behavior** and **state**.
(Behavior simply means that objects define methods that allow them to take some
kind of action.  State refers to the fact that contain data). In Ruby, a program
is largely a result of objects interacting with one another - i.e., asking them
to execute a behavior or to grant access to some data.

Creating objects one by one, however, would be extremely tedious.  That's where
Ruby **classes** come in. Classes are blueprints for building objects en masse.
You define the behavior you want each object to have, and, through the
`initialize` method, you decide the individual data that each object should own.
(You can add more data later, but let's keep things simple).  That is the
responsibility of a Ruby class - to create objects that have common behavior but
individual data.

## Defining a Ruby Class

Let's create a simply Dog class that we can use as an example.

```ruby
 class Dog
   def initialize(name, breed, age)
     @name = name
     @breed = breed
     @age = age
   end

   def bark
     "Woof!"
   end

   def name
     @name
   end

   def breed
     @breed
   end

   def age
     @age
   end
 end
```

Line 1 and Line 23 is all the syntax you need to create a class. It would be a
class that doesn't actually do anything, but to Ruby, you have correctly asked
it to create a class.

However, the point of a class is to create objects that are instances of that
class. You create an instance of a class by calling the `Class.new(arguments)`
method. The `Class.new` creates an instance of the class, and then it calls
**that instance's** `initialize` method.

## What About the Initialize Method

If you look at Line 2 through Line 6 above, we have defined a **instance
method** called `initialize`. An instance method is a method that is called on
the actual instance object. Although we define instance methods in a class, if
we override the instance method in an object, it won't effect other members of
that class.

The `initialize`method is not meant to be called directly - it is the Class's
job to call this method for you. Why can't you call this method directly? Well,
remember that instance methods belong to an individual object.  Before your call
to the Class's new method, you don't have an individual object to which to call
the initialization method from.

## Instance Methods and Instance Variables

You've already been introduced to instance methods. Instance methods are defined
in a class, but they belong to an individual, particular instance object. We've
defined several instance methods, including `bark`, `name`, `breed`, and `age`.

You may have noticed that both the initialization method and the `bark`, etc.,
methods refer to variables with a prepended "@". This are instance variables. An
instance variable is special in several ways. First, an instance variable
belongs to a particular instance object. Each instance object gets its own stash
of instance variables; in general, changing the instance variables of on object
won't effect any other object, even if they are of the same class. Secondly, an
instance variable persists as long as the object is belongs to exists. This is
very different than a regular variable, which disappears as soon as the method
that it is defined in ends execution.

In conclusion, Classes are blueprints for creating objects en masse.  These
objects have their own behavior (through instance methods) and their own state
(through instance variables).
