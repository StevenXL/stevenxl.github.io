---
title: Lessons from Ruby Koans
date: 2015-12-10
tags: ruby koans
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
interest results I came across.

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

## Hash#fetch Method

## A Hash's default value is the same object

## Strings

### Single- vs. Double-Quoted Strings
The main difference between single- and double-quoted strings is that
single-quoted string **will not** perform interpolation and, with two
exceptions, will not recognize escape sequences. For example:

~~~ruby
name = 'Steven'

p "Hello my name is #{name}" # => Hello my name is Steven
p 'Hello my name is #{name}' # => Hello my name is #{name}
~~~

**NOTE**: As mentioned above, single-quoted strings do recognize **two** escape
sequences. For example, the string `'\''` is interpreted as a single quote. This
is necessary - otherwise, there would be no way for a single-quoted string to
contain a literal single quote. Secondly, `'\\` is interpreted as a single
backslash. These are the only two escape sequences supported by single-quoted
strings.

### Flexible Quoting
**Flexible quoting** in Ruby is a technique that addresses creating complex
strings with many quotes. Let's take a look at some code:

~~~ruby
a = %(flexible quotes can handle both ' and " characters)
~~~

Here, we are using flexible quoting to create a string that contains both
single- and double-quotes. Normally, in order to create this string, we'd have
to escape the `'` and `"` characters, since the Ruby interpreter would treat
these as the start of a string.

Flexible strings can also handle multiple lines, like so:

~~~ruby
b = %(
It was the best of times,
It was the worst of times.
)
~~~

Now, `b` is a 3 line string. The first line consists solely as a newline
character `\n`.

**NOTE**: Here documents work in a very similar, though not identical,  way to
flexible quoting. Like almost everything else in Ruby, there are several ways to
skin a cat. Read up on each technique, and choose one if the community has not
done so already.
