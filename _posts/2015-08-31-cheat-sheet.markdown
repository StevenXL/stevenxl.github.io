---
title: Ruby Enumerable Cheat Sheet
date: 2015-08-31
tags: ruby enumerable
excerpt: A cheat sheet of some very common Enumerable methods.
---
# Ruby Enumerable Cheat Sheet

## Most Common Enumerable Methods

`#inject / #reduce`
: The `#inject` and `#reduce` methods are interchangeable. Their purpose is to
iterate over a collection of data - an array, a range, or any other class that
implements an `#each` method - and return a single value. For example, we can
easily sum up a range of integers using `(5..10).inject(:+) => 45`.

`#map`
: The `#map` and `#collect` enumerable methods are interchangeable. They create
a new array which, for each element in the original array, contains the values
returned by the block. For example, if we have an array of integers, we can
easily double each number using `[1, 2, 3, 4].map {|integer| integer * 2}`.

`#select`
: The `#select` and `#find_all` methods are interchangeable. These elements will
iterate over each element in a collection of data, and return only those
elements that return true from a block. For example `[1, 2, 3, 4, 5].select
{|integer| integer.even?} => [2, 4]`.

`#sort`
: The `#sort` method returns an array of the sorted elements. The sort works by
using a comperator <=>. By passing in a block, we can sort the elements by
different properties (such as length, for example), and we can indicate if we
want the items returned in ascending or descending order.
