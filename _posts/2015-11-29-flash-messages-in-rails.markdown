---
title: Flash Messages in Rails
date: 2015-11-29
tags: ruby rails flash-messages
excerpt: How to implement flash messages in Rails.
---
Flash messages in Rails are a simple method of implementing one-way
communication, from the server to the client. Most often, flash messages are used
to inform the user if a CRUD action that they have tried to perform was
successful or not. In this post, I take a look at Flash messages, what they are,
how to implement them, and best practices on where to put the code.

## What is the flash?
According to the [Ruby on Rails
Guides](http://guides.rubyonrails.org/action_controller_overview.html#the-flash),
the `flash` is a special part of the `session`. What is special about it, and
how does it differ from a regular 'ol `session`? There are two important
distinctions between the `flash` and the `session`:

1. First, the `flash` is designed so that, once its contents are read, those
   contents disappear. (Remember that the `session`, on the other hand, is
designed to persist data. We use it to track if a user is logged into our
application, among other things.)

2. Secondly, the `flash` is designed to be available on the **next** request.

Let's explore this second point in more detail.

## Available on the next request?
While most developers have no problem wrapping their heads around the first
point above, new developers do tend to get confused with the second point. Let's
try to clear up this confusion with a bit of code.

Let's say that you have a stable, and that users to your application are able to
perform CRUD actions on the *horses* resource. The `update` action of the
`HorsesController` might look like this:

~~~ruby
def update
  @horse = Horse.find(id: params[:id])

  if @horse.update(horse_params)
    flash[:success] = "You have updated #{@horse.name}."
    redirect_to horse_path(@horse)
  else
    flash.now[:error] = "You have not updated #{@horse.name}."
    render :edit
  end
end
~~~

Notice here that if we go down the *happy path* - i.e., the record is
successfully updated - we set a flash message **and** we invoke
`redirect_to`.  This is very important, because `redirect_to` actually creates a
**brand new** request. Even though we set the flash message on the current
request, we ask the client to make a new request, and it is that new request
which will have access to the contents of `flash[:success]`.

Contrast this with going down the *sad path*. In the sad path, the record is not
updated, we set a `flash.now[:error]`, and **in the same request** we render the
`edit` view. Notice that we did not use `flash`, but instead used `flash.now`.
This is because we want to display this flash message to the client in the
current request - i.e., when we render the `edit` view - and `flash.now` makes
the flash message available to the current request. Unlike an invocation of
`redirect_to`, `render` does not result in a brand new request from the client.

So, in summation, `flash` makes the flash message available to the next request,
and is normally used in conjunction with redirect. `flash.now` makes the flash
message available to the current request, and is used when we are rendering, in
the current request, the page that should display the flash message.

## Where does the presentation logic to display flash messages go?
Now that we now how to set a flash message and when to use `flash` vs.
`flash.now`, let's look at code regarding where to put the presentation logic of
displaying flash message.

The short answer is that the presentation logic **belongs in the layout**. In
most applications, the appropriate place would be `application.html.erb`.
Because we want the message to be easily viewable by the end-user, the logic
belongs after the header, but before the yield call to incorporate the rest of
the content.

## OK, so what's the actual code?
If you have any experience with Ruby hashes, you automatically know how to use
the `flash`. The `flash` is an instance of
[FlashHash](http://api.rubyonrails.org/classes/ActionDispatch/Flash/FlashHash.html). Thanks to the fact that it mixes in the `Enumerable` module, we can iterate over the `flash` just like we could iterate over any Ruby hash:

~~~ruby
flash.each do |key, value|
  # logic
end
~~~

The key in our example would be either `success` or `error`. (Note that even
though we set our keys to be Ruby `symbols`, the `FlashHash` class converts
those to strings when initialized). More appropriate than the block variable
name `value` would be `message`, as that is what that block variable would
evaluate to.

One interesting solution to the fact that the above could will iterate over all
message types - i.e, success and errors - is to use the key as a class. Then, we
can style each class differently, so that success messages look different than
error messages. A great example can be found at the [Agile
Warrior](https://agilewarrior.wordpress.com/2014/04/26/how-to-add-a-flash-message-to-your-rails-page/)
page as well as on this [Stack
Overflow](https://stackoverflow.com/questions/9390778/best-practice-method-of-displaying-flash-messages)
answer.

## Do not put the presentation logic in the views
Oftentimes, new Rails developers will put this logic in the view. Going back to
our stable example, the thinking goes as follows:

*This flash will only be set if the user successfully updates a horse.
Therefore, the logic to present the flash message should go in the `show` view.
Furthermore, the flash error will only occur if the user tries and fails to
update a horse, so I will also put logic in the `edit` view.*

The problem, of course, is that a well-run stable (and any non-trivial
application), will have many resources - saddles, horseshoes, etc. This design
requires that the logic to present flash messages be repeated in many views. By
putting the logic in the layout, the logic lives in one central place.

## Conclusion
Here's what you need to know to successfully use flash messages:

* Use `flash` when you are redirecting, and `flash.now` when you are rendering.

* Presentation logic belongs in a layout, or you will have a lot of duplicate
  code.

* Remember that the `flash` acts as a Ruby Hash. Use that to your advantage.
