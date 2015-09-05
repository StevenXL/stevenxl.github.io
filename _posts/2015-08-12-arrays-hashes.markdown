---
title: Arrays and Hashes
date: 2015-08-12
tags: arrays hashes ruby
excerpt: In this post, I will discuss two fundamental data structures in Ruby - arrays and hashes.
---
Arrays and hashes are two fundamental data structures, not only in Ruby, but in
programming in general. In this blog post, I will go over what arrays and hashes
are, how they are alike, how they differ, and how to use them.

Note that Ruby hashes are *ordered*. In most programming languages, hashes are
**not** ordered, and professional programmers avoid using Ruby's ordered hash
feature. The following blog post will heed that *advice*, and ignore the fact
that Ruby hashes are ordered.

## Arrays

An **array** is an **ordered** collection of objects. Unlike in other languages,
in Ruby you can mix-and-match the type of objects that a single array holds.
Arrays are useful because they give us a way to group a collection of data that,
conceptually, belongs together, and allows us to refer to this collection using
a single variable. For example, let's say that you are trying to hold the first
name, last name, and age of a person. (Note, depending on your needs, an array
may not be the best data structure to use here, but it illustrates the point)..
Without an array, the code would look like this:

~~~ruby
 first_name = "Steven"
 last_name = "Leiva"
 age = 28
~~~

That doesn't seem to be now, but what if you had to do that for multiple people?
You're code would then morph into an ugly beast:

~~~ruby
 first_name_1 = "Steven"
 last_name_1 = "Leiva"
 age_1 = 28

 first_name_2 = "Stephen"
 last_name_2 = "Leipha"
 age_2 = 28
~~~

With an array, we could simply group a person, like so:

~~~ruby
 person_1 = ["Steven", "Leiva", 28]
 person_2 = ["Stephen", "Leipha", 28]
~~~

## Accessing Data in an Array

Now, we can access the data in the variable `person_1` by using syntax such as
`person_1[0]`. Because arrays are ordered, this code snippet will provide the
first name.

## Arrays Can Hold Any Data

Arrays, remember, are useful for grouping together data. In our code
above, we created two arrays, each of which holds the information for a
person. We can also group this data together:

~~~ruby
 persons = [ ["Steven", "Leiva", 28], ["Stephen", "Leipha", 28]]
~~~

The above is called a **two-dimensional array**, since each element in the array
is another array.  Now, when we access `persons[0]`, we would get back an array
containing `["Steven", "Leiva", 28]`. To get to the first name of the first
person, we'd use the syntax `persons[0][0]`

## Hashes

Hashes are similar to arrays, in that they can be used to store a collection of
data. However, while we use the element's **index** to access the data within an
array, we use a hash's **keys** to access the data within a hash. In other
words, hashes are **dictionaries**, they map a key to a **value**. This property
is very useful. Let's go back to our example of storing the data for an
individual, but this time, let's do so via a hash. The code would look something
like this:

~~~ruby
 person_1 = {"first_name" => "Steven", "last_name" => "Leiva", "age" => 28}
~~~

What's the big deal of what we have done? Well, when we were using an array to
store a person's information, we had to remember that we stored the first name
in the first element, the second name in the second element, and the age in the
third element. Now, with hashes, we don't have to remember any of that. We can
access the age by simply using `person_1["age"]`.

## Hashes Are Generalized Arrays

In fact, Hashes are a generalization of arrays. We can create the
**functionality** of an array in a hash, like so:

~~~ruby
 person_1 = { 0 => "Steven", 1 => "Leiva", 2 => 28 }
~~~

I can't think of a single reason why you would do the above, but to Ruby, we've
created a hash. That hash, though, behaves like an array, in that I would have
to remember what "index" stores what information.
