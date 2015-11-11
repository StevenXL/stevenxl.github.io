---
title: Technical Debt - Moving My Blog to Jekyll
date: 2015-11-11
tags: technical-debt jekyll
---
During <strong>Phase 0</strong> of DBC - i.e., before we ever stepped foot on
campus - we were asked to start a blog on [GitHub
Pages](https://pages.github.com/). While we received clear instructions on how
to publish to GitHub Pages, the instructions for how to create the blog itself
were, frankly, rudimentary. We were instructed to create a single HTML page,
carefully consider its syntax and structure, and use that file as a template;
for each new blog post, simply copy and paste the entire `*.html` file, and
modify as needed. In this post, I speak briefly about my own modest experience
with technical debt in the context of this "blogging platform".

(As an aside, I understand and am in agreement with DBC's decision not to
introduce my cohort and I to a content management system or static site
generator during Phase 0. The goal was to get us up and running as quickly as
possible, and asking folks to learn any framework at this stage would have been
premature.)

## The Problem
Any individual familiar with software design principles would immediately see
the problem with this system - it leads to a ton of **duplicated code**. That,
in turn, decreases **mantainability**; even the simplest change requires
changing the same piece of code / markup / text in multiple areas. Not only is
this a rather boring chore, but it is incredibly error-prone. Finally, a system
is that is difficult to change is a barrier to *progressive enhancement*; once
the application is working, no one wants to touch it.

In the words of Sandi Metz, *technical debt* is a loan that you take on whenever
you compromise your system's design; today's compromise makes it costlier to
change your system in the future. With the current system, any change was going
to be a complete nightmare.

## The Solution
The solution to the problem was to use a framework that would encourage building
a DRY, modular, easily-modifiable system. I chose to go with Jekyll, primarily
because it was small enough that I could learn the basics in a day, it is
endorsed by GitHub Pages, it is well-documented, and there is a large community
from which I could receive and give help. Making the decision to go with Jekyll
was the fun and easy part; as a technologist, I am always interested in
exploring new platforms, languages, and system. Implementing the decision,
however, was anything but fun or easy.

## Implementation
In implementing the change from a copy-a-page-and-modify-as-needed platform to
Jekyll, I had to change a lot of pieces.

First and foremost, I had to **learn how to use Jekyll**. Luckily, the Queens
Library System offers each card-carrying member a free subscription to Team
Treehouse, where I was able to find a tutorial on using Jekyll. That, coupled
with reading the documentation and a dogged determination to add syntax
highlighting, sufficed to get me up and running.

Secondly, I had to **clean-up my HTML markup**. Although the site under the old
system *looked* consistent, the underlying HTML was not. This was a result of me
trying to make small changes after creating my base template, but not getting
around to doing so on all the pages. This also meant I had to **rename** my
files to conform to Jekyll conventions; convert all my **code blocks** into
markdown code, as I had previously used custom CSS to style code; and I had to
**normalize** all my headings. Even on on individual basis, doing this for
roughly 20 documents is not fast, is not fun, and it most certainly shouldn't be
necessary.

Finally, I had to learn how to work with the various Domain Specific Languages
used by Jekyll - this includes Liquid Templates, Font Matter, the YAML format,
and Markdown. (OK, I was already familiar with Markdown from working on GitHub).

## Benefits
Despite the hassle of moving to Jekyll, the benefits are well worth it.

From a technology standpoint, I know have a system that I can change easily. I
can change the styling with minimal effort (in fact - I think I will; the
headers on each section as of the time of this writing are looking poor). I get
consistency across my site, as each page is using a template that I've
specified. And I know have a website that I can continue to build features on
top of, such as pagination, linking tags, etc.

The technical benefits, however valuable, are small compared to what I'll term
the *behaviorial benefits* of such a system. A system where I can open up a
markdown file, start writing down my thoughts, and then publish encourages me to
blog. It breaks down a barrier to doing so because I don't have to think about
the monotony of copying a file, staring at HTML source code, and then figuring
out how to modify it. I don't have to worry about the updating the links in my
blog index page.  I have a system that takes care of all the mundane details for
me and allows me to write.

In short, I interact with this system a lot differently than I ever interacted
with *that* system.
