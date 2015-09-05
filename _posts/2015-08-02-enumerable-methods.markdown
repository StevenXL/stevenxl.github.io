---
title: Enumerable Methods
date: 2015-08-02
tags: ruby enumerable group_by map cycle
excerpt: In this blog post, I will discuss the Enumerable moduel in Ruby, and three of its methods.
---
In this blog post, I will discuss the **Enumerable module** in Ruby, and three
of its methods: `#map`, `#group_by`, and `#cycle`.

(Note that most of the methods below can be called without a block - in that
case, they will return an Enumerator object; this post will discuss the behavior
of these methods when they are called with a block).

## The Enumerable Module

First, let's discuss what the Enumerable module is, how it works, and how to use
it.

As a Ruby module, Enumerable is a collection of behavior / methods that is meant
to be **mixed into** class definitions, like so:

~~~ruby
 class MyClass
   include Enumerable
 end
~~~

However, the Enumerable method "piggybacks" off of a class's `each` instance
method; our class must define an each method so that the Enumerable module
"knows" how to iterate through the object.

~~~ruby
 class MyClass
   include Enumerable

   def each
     # logic on how to iterate through instance of the class
   end
 end
~~~

Once we have included the Enumerable module in our class definition, and we have
defined an `each` instance method, we can begin to use the many instance methods
that are defined in the Enumerable module.

## The `Enumerable#map` Method

One of the methods that the Enumerable module will endow our class with is the
`Enumerable#map` method.

The `Enumerable#map` method takes a block and returns an array. The returned
array will have the same number of elements as the receiving object, but each
element of the returned array will consist of a transformation of each element
in the receiving object.

For example, let's take an array that contains the ages of various family
members.

~~~ruby
 family_ages = [ 10, 13, 7, 40, 45 ]
~~~

Now, let's say we'd like to figure out how old everyone will be in ten years -
in other words, we'd like to add 10 to each element in our original data set.
This is a perfect use-case for the `Enumberable#map` method. We define the
transformation that we'd like to apply to each element in the form of a block,
and the `Enumerable#map` method will apply that transformation
to each element and return the results:

~~~ruby
 family_ages.map { |age| age + 10 }
~~~

The result from the above code would be a new array:

~~~ruby
 [ 20, 23, 17, 50, 55 ]
~~~

## The `Enumerable#group_by` Method

Another method provided by the Enumerable module is the `Enumerable#group_by`
method.

The `Enumberable#group_by` takes a block and returns a hash. The keys in the
hash will correspond to all the return values of the block, while the values in
the hash will be an array whose elements will be the elements that evaluated to
the key.

For example, let's take our original family_ages array:

~~~ruby
 family_ages = [ 10, 13, 7, 40, 45 ]
~~~

Now, let's imagine a scenario were we are planning to take the family to an R
rated movie. We'd like to invite only those individuals that are over the age of
16. In other words, we want to **group our array **by a certain property. We can
do that with the `Enumerable#group_by` method:

~~~ruby
 family_ages.group_by {|ages| ages > 16 }
~~~

Because we are doing a Boolean comparison, the block will only return false or
true, and these will be the keys for the hash returned by the `group_by` method.
The value of those keys will be arrays - one containing the elements in the
original array for which the block returned false, and the other containing the
elements for which the block returned true:

~~~ruby
 {false => [ 10, 13, 7 ], true => [ 40, 45 ]}
~~~

The `Enumerable#group_by` method is in some ways more complicated than the
`Enumerable#map` method. Most individuals find arrays (returned by the map
method) simpler to understand than hashes (returned by the group_by method).
Furthermore, while the returned hash in our example had only two keys, that is
not always the case. There will be as many keys as values returned by the block.

## The `Enumerable#cycle Method`

The final method we will touch on in this post is the `Enumerable#cycle` method.

The cycle method is perhaps the simplest of the methods we will touch upon here,
due not only to what it actually does, but to its descriptive method name.

The `Enumerable#cycle` method cycles through the receiving object as many times
as the argument passed to the method indicates.

For example, let's say we typed the following into an irb session:

~~~ruby
 family_ages = [ 10, 13, 7, 40, 45 ]
 family_ages.cycle(2) {|age| puts age}
~~~

The code above would simply output each element of the array, and then repeat
(since we asked it to do this twice via the argument to cycle).
