---
title: Namespacing Controllers in Rails
date: 2015-11-13
tags: rails ruby namespacing
excerpt: The What, the Why, and the How of Controller Namespacing in Rails
---
It doesn't take long for the novice Rails developer to come across a
*deliberately* namespaced controller. (I use the word deliberate because the
class defined in the ever-present `application_controller.rb` itself inherits
from a namespaced class. For the sake of brevity, I will omit
the word deliberate going forward). In this post, I will explain
the reason behind namespacing a controller - i.e., how and why the technique is
commonly used.

## What is Namespacing in General?
In software development, namespacing is a technique by which a class / module /
constant or other container for code is **nested** within another such
container.  The goal of namespacing is, primarily, to avoid polluting the
top-level namespace and avoid naming collisions and, secondarily, to provide
context for the nested code.

For example, the constant `PI` in the core ruby library is nested within the
`Math` module. In order to access this constant, you'd have to use the **scope
resolution operator** (::), like so: `Math::PI`. Because `PI` is nested within
the `Math` module, I can define another constant at any other scope level and
avoid over-writing `Math::PI`. Furthermore if `PI` were ambigious, the fact that
it is nested within the `Math` module would serve as a clue to the programmer
what the nested element is / does.

## How Namespace in Rails Controllers Works
Namespacing a controller in Rails will generate a controller that:
1. Inherits from `ApplicationController`
2. Provides a base class within a namespace
2. Lives inside a folder named after the namespace
3. Provides a simple interface for having other controllers inherit from the
   base class.

For example, running `rails g controller admin/application` will generate a
file `application_controller.rb`. Because this file 1) lives inside an `admin`
directory and 2) defines a class namespaced within `Admin`, it does not clash or
overwrite the `application_controller.rb` file or class that lives one directory
above the directory tree.

Further, to generate other controllers that inherit from the newly generated
controller, the command `rails g admin/projects` would suffice.

## Why Namespace Controllers in Rails
Controllers in Rails are namespaced when you want to perform the same action on
multiple resources. Commonly, this action is authorizing a user. By
authorizing at the base-level controller and declaring other controllers to
inherit from the base, the developer is freed from having to authorize the user
in each individual controller.

## Bad Things Happen if You Don't Follow the Rails Way

I can achieve the same effect without namespacing, by creating a base controller
that inherits from 'ApplicationController'. For illustrative purposes, let's call
this controller 'UserActionsController'. I can define the authorization in
'UserActionsController', and have the controllers I need to protect inherit from
'UserActionsController'.

However, this approach has a few problems.  First, I would have to remember to
manually over-write which controller the "protected" controllers inherit from,
as `rails g controller` will default to the unprotected 'ApplicationController'.
Secondly, if I only want certain actions of the same resource protected, I'd
have to define the authorization on a per-controller level instead of at the
top-level, or I would have to create two separate controllers for the same
resource. (Because there is no namespacing, these controllers cannot have the
same name). Of course, such a change would reverberate back to the routes.

## Conclusion
In short, namespacing in Rails provides an easy way to create a new base
controller in which common actions that need to be performed on a set of
controllers can be defined.
