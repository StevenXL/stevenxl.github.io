---
title: AJAXifying a Create Request via a Modal - Part 1 - Problem and Tools
date: 2015-12-26
tags: ruby rails ajax
excerpt: A blog post on my recent contribution to the RailsBridge Bridge Troll
---
Lately, I've been
[contributing](https://github.com/railsbridge/bridge_troll/commits?author=StevenXL)
to the [RailsBridge Bridge Troll](https://github.com/railsbridge/bridge_troll)
project.  It is a great learning experience - the application is complex enough
to be a challenge, and it is well-written and exemplifies best practices in the
Rails community, giving me an opportunity to learn from great examples. Besides,
as someone that is looking for a software / web development position, it is a
great way to expand my network.

In this post, I want to go over [pull request #421](https://github.com/railsbridge/bridge_troll/pull/421), as it was a
particularly fruitful learning experience. This first part will go over the
problem I was addressing in my pull request, as well as the tools and conceptual
map needed to solve the issue.

## Issue #378
This particular commit was in response to
[Issue #378](https://github.com/railsbridge/bridge_troll/issues/387). You can
read the issue yourself, but the crux of the matter was that every event has to
be associated with a location. If the appropriate location is not yet in the
system when an organizer tries to create an event, they have to create a new
location. That used to mean that the organizer had to go to a different page,
create the location, and then return to the page in which they can create their
event. This was a big diversion from the organizer's original intent. Now, the
location is created via an AJAX request.  The location form pops up in a modal,
and it is closed when the location is created, making the entire experience much
more seamless for the user. So, in effect **my job was to AJAXify an organizer's
ability to create a location from the event creation form**.

When I stop to think about it, my overarching goal was simple. I had to
intercept the normal behavior of a pre-determined set of links, forms, and input
elements so that they would function over AJAX. Furthermore, the server had to
recognize these AJAX requests so that it would respond differently than it would
to an HTTP request. Let's introduce the tools and concepts that allow us to do
that.

## The Tools
Before we dive into the actual code in the commit, let's talk about the
different pieces that we will be using, and how they fit together. In
particular, below I introduce `jquery-ujs` and HTML5's [data attributes](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_data_attributes), and their roles in this process.

### jquery-ujs
Unobtrusive JavaScript relies on JavaScript code to identify a set of elements
whose normal behavior has to be overridden.  (Yes, JavaScript code will be
executed at various stages of this process. This is the first; keep count, as it
will allow us to better understand what code and technology is responsible for
what part of the process). Although there are various ways to add code that will
identify the relevant elements, most tutorials and actual source code will use
the `jquery-rails` gem and then require `jquery` and `jquery-ujs` in the
`application.js` manifest file.  (The [GithHub Page](https://github.com/rails/jquery-ujs) will provide more thorough
installation instructions).

What does this `jquery-ujs` do? It intercepts remote links, forms, and inputs
and overrides their click event to submit information to the server via an AJAX
request. But how does `jquery-ujs` know what elements should have their behavior
changed? In other words, how does `jquery-ujs` identify which elements are
"remote" elements? That's where HTML5 comes in.

### HTML5
HTML5 supports a feature in which we can create [custom attributes](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_data_attributes) simply by pre-pending those attributes with `data-`. For example, if I want a link to have
an attribute called `bgcolor`, and I want to set that attribute's value to
`#990000`, I can write the following code:

~~~html
<a href="#" data-bgcolor="#990000">Custom Attribute Added</a>
~~~

The ability to easily create custom attributes, and to set those custom
attributes to any value I desire, creates a very powerful effect. I can now
easily and cleanly target specific elements on my webpage, and change their
behavior based on the value of a custom attributes.

### Unobtrusive JavaScript - jquery-ujs and HTML5 working together
The ability to add custom tags via HTML5 allows for Unobtrusive JavaScript. It
does this by allowing us to give custom attributes to elements which then
**triggers** JavaScript code, as explained above. Unobstrusive JavaScript
removes the need for inline JavaScript, increasing **separation of concerns**,
**maintainability**, and **DRYness**. Let's take a look at two pieces of code.
Both of the pieces of code below create a link that, when clicked, will change
the background color of the link; however, the first one does it with inline
JavaScript, and the second code snippet does so with Rail's Unobstrusive
JavaScript.

~~~html
<!-- Obtrusive JavaScript -->
<a href="#" onclick="this.style.backgroundColor='#990000'">Click Here</a>
~~~

~~~html
<!-- Unobtrusive JavaScript -->
<a href="#" id="change-bg" data-bgcolor="#990000">Click Here</a>
~~~

**Note**: Credit to Ryan Bates at [Railscasts.com](http://www.railscasts.com)
for the above code snippets.


### Putting the Pieces Together
As you can see, the obtrusive JavaScript code contains JavaScript directly
inline. However, the unobtrusive JavaScript code contains what conceptually
amounts to a variable (`bgcolor`) set to `#990000`. We can run a JavaScript
script to look for all elements with the data-bgcolor attribute and override
their click behavior to change the background color of the element to the value
of `bgcolor`.

For our purposes in this post, `jquery-ujs` does something very similar. It
looks for all elements on a page that have the data attribute `data-remote`.
Once it finds these elements, it intercepts their normal behavior and
communicates to the server via an AJAX request instead of an HTTP request. Rails
has already put in much of the hard work for us; it has provided JavaScript code
that looks for elements with `data-remote`, and it has provided code that
extracts information from that element (such as what the `href` attribute is) in
order to provide that information in an AJAX request. In the second part of this
post, I will look at how this feature of Rails can be leveraged in a real
application.
