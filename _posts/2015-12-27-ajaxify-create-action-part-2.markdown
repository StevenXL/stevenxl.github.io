---
title: AJAXifying a Create Request via a Modal - Part 2 - Code
date: 2015-12-27
tags: ruby rails ajax
excerpt: The second part of a blog post on my recent contribution to the RailsBridge Bridge Troll
---
In the [first post](http://stevenleiva.com/ajaxify-create-action-part-1/) of
this two post-series, I introduced the problem I was trying to solve, as well as
the conceptual tools and Rails features that I was going to use to solve them.
In this post, I will go over the actual code.

## Events Controller
First, let's talk about the `Events controller`. We know that we want this
controller to contain a form to create a location (in addition to a form to
create an event). Again, the main purpose here being that we want to keep the
user on the `/events/new` page, and not have them wandering across our app to
create a new location when they simply wanted to create a new event.

In order to accomplish this, we are going to use the location form partial
located in `app/views/locations/_form.html.erb`. That form relies on an
`@location` variable, so we will need to set that variable in our Events
Controller whenever the `new` or `create` action is invoked.

~~~ruby
# app/controllers/events_controller.rb

before_action :set_empty_location, only: [:new, :create]

 .
 .
 .

def set_empty_location
  @location = Location.new
end
~~~

Now, our `Events Controller` is able to pass an empty location to the views
rendered by `new` and `create`.

**Note**: The reason why we need an empty location for the `create` action in
addition to the `new` action is that, if the user is unable to successfully
create an event, the `create` action in the `Events Controller` will render the
`new` view. That view needs the empty location.

## Events New View Form Partial
The view rendered by `Events#new` action is the `app/views/events/new.html.erb`
view. This view, in turn, calls a partial `app/views/events/_form.html.erb` to
render a form for creating a new event.

Again, we also need this partial to render a form to create a new location. We
do that by directing the `_form.html.erb` partial to render another partial:

~~~ruby
# app/views/events/_form.html.erb

<% if @event.new_record? %>
  <%= render "location_modal", modal: true %>
<% end %>
~~~

Why do we check if `@event` is a new record? We do this because there might be
other times when the Event's form partial is rendered even though the user never
intends to create a new event. One example is if the user is editing an existing
event.  In cases such as these, there is no reason to render a location form as
well, so we only call the `location_modal` partial if the user is trying to
create a new record.

**Note**: It is very important to notice that the `_location_modal` partial is
being called **after** we've closed the event form. **Nesting forms is a very
bad thing**. The client will easily get confused as to what location the form
needs to be submitted to, and you will end up with headaches. Don't nest forms.

## Location Modal Partial
The `_location_modal.html.erb` partial is one of the longer pieces of code, but
it is actually one of the simplest. It is simply a direct copy-and-paste job
from the [Bootstrap's Modal](https://getbootstrap.com/javascript/#modals) page,
with the slight modification of calling the `app/views/locations/_form.html.erb`
partial within the `#modal-body` div. In fact, I won't show all the code for
this (you can visit the link above for that), but I will show the business end
of the code:

~~~ruby
# /app/views/events/_location_modal.html.erb

<%= render 'locations/form', modal: true %>
~~~

Notice that we are passing `modal: true` to our location's form partial.
Remember our discussion of [Unobtrusive JavaScript](http://stevenleiva.com/ajaxify-create-action-part-1/)?  We mentioned
that, `jquery-ujs` looks for a `data-remote` custom attribute in order to change
the default behavior of elements, and make them communicate with our server via
AJAX instead of a regular HTTP request. When we render our location's form
partial, the `modal` variable will be set to `true`, and we will use this to
indicate that the form should have its `data-remote` attribute set to true.

## Location Form Partial
The location form partial was only slightly modified. All we need to do is make
sure that its `data-remote` attribute is set to true appropriately. Since we
pass in a `modal` attribute set to true when we need the form to communicate via
AJAX, we can use that in our location form partial:

~~~ruby
# app/views/locations/_form.html.erb

<% modal ||= false %>
 <% remote = modal ? true : false %>

 .
 .
 .

<%= simple_form_for(@location, remote: remote, html: {role: :form, 'data-model' => 'location'}) do |f| %>
    <%= render 'shared/model_error_messages', model: @location %>

 .
 .
 .
~~~

## Locations Controller
Finally, our `Locations Controller` has to respond differently to an AJAX
request than it does to an HTML request. To do that, we add the following code
to the `Locations#create` action:

~~~ruby
# app/controllers/locations_controller.erb

respond_to do |format|
    if @location.save
      format.html { redirect_to @location, notice: 'Location was successfully created.'}
      format.js   {}
    else
      format.html { render :new }
      format.js   { render action: 'create_failed' }
    end
end
~~~

## Responses to AJAX Requests
Finally, let's take a look at the actual response from the `Locations
Controller` to an AJAX response in the `#create` action. Let me say, first of
all, that I had no idea that we could send a JavaScript response back to the
client, **and** have the client automatically execute the code. However, this is
a great feature that allows us to respond to AJAX request (or any request,
really) however we'd like: First, let's discuss the **happy path**.

~~~ruby
# app/views/locations/create.coffee

$('#new-location-modal').modal('hide');

id = '<%= @location.id %>'
$location_select = $('#event_location_id')
$location_select.append("<option value='#{id}'><%= @location.name_with_region %></option>")
$location_select.val(id).trigger('change')
~~~

When this JavaScript script gets executed, it does three important things.
First, it hides the modal that the user used to send off the request to create a
new location. Secondly, it appends the freshly created location to the drop-down
menu of the user. Lastly, it pre-selects that location for the user.

**Note**: Thanks to [@tjgrathwell](https://github.com/tjgrathwell) for the above
snippet.

~~~ruby
# app/views/locations/create_failed.js.erb

$('#new-location-modal-body').html('<%= j render "form", modal: true %>')
~~~

In the un-happy path, we simply render the location form again, and replace the
entire `#new-location-modal-body` with that new form. However, remember that our
`@location` variable now contains errors, and our application knows when and how
to display those errors.

## Conclusion
So there you have it. The amount of moving pieces makes this task a bit
difficult, but the individual code changes themselves are not terribly
complicated. We set our `Events Controller` to render a location form along with
an event form in the `#new` and `#create` actions; we render the location form
partial with the form's `data-remote` attribute properly set; and we have our
`Locations Controller` respond differently to AJAX vs. HTML request, as well as
to a successful event being created versus a failed attempt.

This has been a great learning opportunity for me. Unobtrusive JavaScript is an
incredible feature to AJAXify our applications, and the ability to send back
JavaScript code that is automatically executed by the client opens up a world of
possibilities. I hope that this walk-through of the process was informative for
you as well.
