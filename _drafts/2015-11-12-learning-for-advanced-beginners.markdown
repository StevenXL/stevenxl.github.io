---
title: Learning for Advanced Beginners
date: 2015-11-12
tags: learning beginners
excerpt: How I achieve a basic level of proficiency with new technologies.
---
As developers, we are often asked to work with new technologies. Commonly, this
entails learning a new DSL and incorporating that into a larger framework. (In
a previous post, I related my experience in having to learn YAML and Liquid
Templates, among other technologies, in order to move my blog to Jekyll). For
Rails developers, every new gem is a new API that one must learn. Having gone
through this process many times, and planning to go through it many more, I came
up with a process to follow in order to achieve a basic level of proficiency
with new technologies in a timely fashion.

## Know Where to Get Help
One of the benefits of picking the leading DSL / technology for a particular
problem is that they tend to have an active community. The first thing I tend to
do when I am learning a new technology is learn where and how to interact with
that community, and how  to get help. Does this project have an online forum or
a mailing list? What about a real-time chat system such as IRC / Gitter / Slack?

If they do have one or more of these means of communication, make sure you ask
questions correctly and follow the community norms. If the project consists of a
forum or other searchable format, take the time to search for the answer on your
own.

Most importantly, don't be afraid to ask for help. Personally, I find intrinsic
joy prodding and poking at a program, reading source code, reading
documentation, and trying to figure things out myself. I do not advocate
skipping these steps, but practice time-boxing. Give yourself a reasonable
amount of time to figure it out, and then ask for help.

## Learn Concepts, not Code
One of the key mistakes that I made when I first started out was a burning
desire to write everything down; example code, commands, methods, etc. This is
inefficient - there's no way to memorize everything, and that's what
documentation is for anyways.

The key is to learn concepts and how the system is designed. Not only is this
much more manageable, but doing so will give you a better sense of what is
possible within the system. A well-designed system will be internally
consistent, so that when you see a particular pattern in one part of it, you can
most likely apply it somewhere else. I imagine that many expert Rails developers
have developed this sixth sense - they know what the system should do, even if
they don't know how to make the system do that. (That last part is solved by a
simple visit to the Rails API).

## Read the Table of Contents
As I mentioned above, it is impossible to hold in (human) memory any of today's
most popular frameworks. Even Sinatra, a much smaller framework than Rails
(though equally beloved), would be challenging to know inside and out. What I
have found to be invaluable in these situations is to read the Table of Contents
(or its equivalent). Hopefully, doing so will give you a good idea on what
features the DSL / Technology comes with. Documentation should be randomly
accessed and lazily evaluated - knowing that Rails provides a feature for me to
change the order in which callbacks are executed is all I need to know. When I
have to actually implement this feature, I quick scan of the docs or a quick
Google search will bring me to the `prepend_before_action`.

## Have a Playground
It is important to have a way to quickly and easily switch between a
"development" context and a "play" context. Think in terms of an IRB / PRY
session for whatever technology you are currently using. You want a quick and
short feedback loop, so you can test out any potential answers to your problem.
As you develop your intuition about what your technology should be able to do,
you want to validate that intuition by popping into your playground and testing
out the code.

Learning a new technology is a constant in a developer's career. Most of the
time, we just need to get to know the basics. I've found that by following the
process above I am able to shorten the time between when I first get introduce
to a new technology, and when I am a basic level of comfort and productivity.
