---
title: The Pundit Gem - Conceptual Notes
date: 2015-11-26
tags: rails ruby pundit gems
excerpt: Conceptual notes on how the Pundit gem works.
---
The Pundit gem is one of the most popular solutions for user authorization in
the Rails ecosystem. Both the highly regard (by me, but also others I'm sure)
*Rails 4 in Action* and *The Rails 4 Way* mention Pundit as their default
solution for authorization. In this post, I want to simply note down important
concepts on how Pundit works.

## Roles

### Table / Relation
Though not stricly a necessity, In order to use Pundit, you'll likely need to
define a relation / table called `roles`.  This table contains the information
that Pundit will use to determine what "role" a specific user has in relation to
another Active Record object. For example, if we are building a blogging
platform on Rails, our `roles` table would contain three fields / attributes.
Two of these fields would contain foreign keys referencing a `users` record and
a `posts` record. The third field would contain a role. Thus, a single record in
the `roles` table directly maps into the role that a user has for a specific
post. By extending this system, we can define roles for all users on a
project-specific basis.

### Model

As with most tables / relations in a Rails system, the records in the `roles`
table will map into a Role model and thus a Ruby object. As mentioned in *Rails
4 in Action*, it is "these *Role* objects.. that will be used to determine
exactly what actions a user can take."

#### Code
To generate the proper migration and Role model, we can use the Rails model
generator, like so:

`rails g model role user:references role:string project references`.

This will generate a proper model and migration, as discussed above.

(As always, it is recommended that the reader open both the model and migration
generated, and look up any options that s/he does not understand).

## Policies through Pundit
The `pundit` gem allows you to turn roles into permissions, and to enforce those
permissions - i.e., letting authorized users in and redirecting unauthorized
users.

### Policies
In Pundit terms, permissions are enforced through a concept of `policies`.
Policies are regular Ruby classes that inherit from `ApplicationPolicy`. Each
resource for which you want to determine permissions for will require a Policy,
and the class defined will be resemble the `PostPolicy` pattern. (I will simply
refer to such a class as `Policy` below).

A `Policy` is simply a class, and like all classes, their role is to instantiate
instances of themselves. A `Policy` class is instantiated with a User object and
a resource object, such as (back to our blogging platform example) a Post
object.

However, policies are not instantiated directly - Pundit has a helper method
called `authorize`. Authorize simply instantiates a new object from the
appropriate `Policy` class. You need to provide `authorize` with the record -
i.e., the Post object - and it will take care of passing in the `current_user`
to the `Policy.new` method. `authorize` does one more important thing - it
infers from the action in which you called `authorize` from what method on the
newly instantiated policy object it should call.

### Example
Let's put all of this together in one clean example by taking the following
code, in the `update` action of the `PostsController`.

~~~ruby
def update
  @post = Post.find(params[:id])
  authorize @post
  if @post.update(post_params)
    redirect_to @post
  else
    render :edit
  end
end
~~~

Because we passed in a Post object to the `authorize` method, authorize will
infer that there is `PostPolicy` class that it can instantiate. Furthermore, it
will call the `update?` method on that instance of the `PostPolicy` class, to
determine if the current user is authorized to access this action.

By defining the `update?` method in the `PostPolicy` class, we can determine
when `authorize` should return true and when it should return false (and thereby
raise a `Pundit::NotAuthorizedError`).

## Roles & Policies
Now that we know what Roles and Policies act, how do they interact? Well, we can
define the `update?` method in the `PostPolicy` class, so that it leverages the
Role object and relation.

Let's assume that one of our roles in our system is 'manager', and only a
manager can update a post. We can define the following method in our
`PostPolicy` class:

~~~ruby
def update?
  record.roles.exists?(user_id: user, role: 'manager')
end
~~~

Remember that `record` is an instance of Post. In order for the above to work, we
have to make sure that our Post model has a `has_many` association with our Role
model, and our Role model has a `belongs_to` association with the Post model.
(Of course, it is good hygiene to make sure other associations, such as between
User and Roles, are also included).

## Limiting what a User Sees
A user should only view links / resources that s/he has access to. `Pundit`
makes this simple to do via policies as well.

### Policy::Scope
To limit what a user sees, `Pundit` encourages developers to define a class
called `Scope` nested within a policy. Inside of this nested class, define a
method called `resolve`, which will return only those objects that the user has
access to. For example, the `resolve` method may look like this:

~~~ruby
def resolve
  scope.where(user_id: user)
end
~~~

Now, in my controller I can use code such as:

~~~ruby
def index
  @post = policy_scope(Post)
end
~~~

Now, a user will only see the posts to which they are assigned. (This is a very
crude example, but it gets the point across). It is important to note that
`policy_scope` will call the `new` method of the nested `Scope` class with the
current user as the first argument, and the argument to `policy_post` as the
second argument.
