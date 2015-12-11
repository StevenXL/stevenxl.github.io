---
title: Lessons from Ruby Koans - Arrays
date: 2015-12-11
tags: ruby koans arrays
excerpt: A list of interesting findings as I work through the Ruby Koans.
---
My progression as a software developer has steadily led me to  more and more
abstract tools.  I began software development by taking the incredible
[CS50x](https://www.edx.org/course/introduction-computer-science-harvardx-cs50x),
moved on to learning Ruby via various means, and now I am continuing to
strengthen my familiarity with Rails. Each one of these languages / frameworks
is more abstract than the other. That's not necessarily a bad thing, as
abstraction allows us to build great software quicker. However, not only do I
enjoy learning what's under the *abstraction covers*, but it is crucial
knowledge for those times when you are trying to fix a bug, the framework
doesn't implement a feature that you need, or you are trying to find a novel
solution to your specific business issue.

Because I think it is important to know how a framework achieves what it does, I
have made sure to continue practicing Ruby, even if it is at a small scale. I
have decided to do that with [Ruby Koans](http://rubykoans.com/), a free,
command-line project that teaches the Ruby syntax and concepts through
test-driven development. In this blog post, I will note down some of the more
interest results across a series of posts, starting with arrays.

## Slicing Arrays
There are a couple of interesting results when slicing arrays in Ruby. In
particular, let's discuss when you will get `nil` vs `[]` from a slicing
operation.

~~~ruby
array = [:peanut, :butter, :and, :jelly]

array[4]    # => nil

array[4, 0] # => []

array[5]    # => nil

array[5, 0] # => nil
~~~

At first glance, there appears to be an inconsistency here. When you try to
index into the 5th element - `array[4]` - `nil` is returned because the 5th
element does not exist. `array[5]` returns `nil` for the very same reason.
However, notice what happens when we try to slice `array[4, 0]` vs. `array[5,
0]`. The first operation returns an empty array, while the second returns nil.
Why do we get this (seeming) inconsistency? If both the 5th element and the 6th
element do not exist, shouldn't we get the same result when we try to slice
starting at an element that doesn't exist?

The answer to this is a **resounding no**. Slicing and indexing are very
different. When you slice, you are indicating the **goal post** you want to
start at with the first parameter, and how many elements you want returned to
you with the second parameter. When you index, you are indicating with the first
and only parameter which **element** you want returned.

What do I mean by saying that, with slicing, the first element indicates a goal
post? Goal posts simply mean the spaces to the left and right of an element.
Like most (all?) number concepts in computer programming, these goal posts start
counting from position 0. Let's take a look at the elements of our array, but
let's include the goal posts:

~~~ruby
     :peanut     :butter     :and     :jelly       # <- array elements
  0           1           2        3          4    # <- goal posts
~~~

As you can see from the above, each element is "enclosed" by two goal posts. So,
when we ask Ruby to slice from the 4th goalpost, we get an empty array back.
This makes sense because the 4th goal post is in bounds, albeit just barely.
When we ask Ruby to slice from the 5th goal post, we get `nil` because the 5th
goal post is out of bounds.

So remember, slicing and indexing **are different**, despite similar syntax.
When you slice, you are dealing with **goal posts**, not indexes, and these goal
posts are numbered starting at 0.

Note: I do not know the proper name for the goals posts; I just thought goal
post was good enough.
