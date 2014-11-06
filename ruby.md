### Barrel Development Best Practices

# Ruby / RoR Best Practices
- [Method Suffixes](#method-suffixes--and-)
- [Defer non-critical, expensive jobs](#defer-non-critical-expensive-jobs)
- [Turbolinks](#use-turbolinks---when-it-makes-sense)
- [Fat Models, Skinny controllers, and Scopes](#fat-models-skinny-controllers-and-scopes)
- [Shallow Nesting](#shallow-nesting)
- [And/Or Keywords](#dont-use-the-and--or-keywords)
- [Test-Driven Development](#test-driven-development)
- [Dup, Clone, and Freeze](#be-careful-with-dup-clone-and-freeze)
- [The try() Method](#the-try-method)
- [Non-Database-Backed Models](#non-database-backed-models-rails)
- [Default Scope](#default-scope-rails)
- [Unless / Else](#unless--else)

## Method Suffixes (! and ?)

You can use ! or ? at the end of a method name to be more descriptive. The convention is:

```ruby
class Person

  # Without a sufix, we reference the value of the attribute
  attr_accessor :can_talk

  # With a !, we modify the object in place
  def can_talk!
    self.can_talk = true
  end

  # With a ?, we expect a true/false return
  def can_talk?
    self.can_talk == "Yes" ? true : false
  end

end
```

There are exceptions to the previous convention, but **DO NOT** write a method with a suffix unless it has a non-suffixed counterpart.

```ruby
class Person

  attr_accessor :age

  # We don't use a '?' because there is no other 'can_talk' method
  def can_talk
    self.age > 2 ? true : false
  end

end
```

## Defer non-critical, expensive jobs
If a request includes a labor-intensive job (the classic example is when sending an email to a user), that is not critical to the response you are sending to the user, then that job should be deferred to a queue.

##### The default process:

1. User send request – *client side is now blocked*
2. Server receives request – *server is now blocked*
3. Server completes basic functions
4. Server completes the expensive function – *server is now unblocked*
5. Server sends response to client – *client side is now unblocked*

##### With a job queue:

1. User send request – *client side is now blocked*
2. Server receives request – *server is now blocked*
3. Server completes basic functions and queues the expensive job – *server is now unblocked*
4. Server sends immediate response to client – *client side is now unblocked*
5. Server completes the expensive function whenever a processor is free

Obviously, if you need to include the result of the expensive job in your response to the client, then this is not an option.

The gem "Delayed Job", which was extracted form the Shopify code base, makes all of this super easy. Plus, it plays nice with hosting plans like Heroku.

##### Resources
[Delayed Job](https://github.com/collectiveidea/delayed_job)  
[Railscast on Delayed Job](http://railscasts.com/episodes/171-delayed-job-revised)  
[Heroku Documentation](https://devcenter.heroku.com/articles/delayed-job)

## Using Turbolinks

[Fuck Turbolinks.](https://coderwall.com/p/bylmmw) There, I said it. -N

## Fat Models, Skinny controllers, and Scopes

Keep those controllers slim. Move any code that doesn't directly relate to the response into the model.

##### Bad:

``` ruby
# controllers/tasks_controller.rb

def index
  @complete_tasks = Task.where(complete: true)
  @incomplete_tasks = Task.where(complete: false)
end
```

##### Good:

``` ruby
# models/task.rb

scope :complete, -> { where(complete: true) }
scope :incomplete, -> { where(complete: false) }
```

``` ruby
# controllers/tasks_controller.rb

def index
  @complete_tasks = Task.complete
  @incomplete_tasks = Task.incomplete
end
```

The real advantage here is that we can now use the ```:complete``` and ```:incomplete``` scopes in other parts of the app. For example. If we want to see all incomplete tasks for a specific user, we could do something like:

``` ruby
@user = User.find(:id)
@user_tasks = @user.tasks.incomplete
```

## Shallow Nesting
Developers are often tempted to nest all of their resources like so:

```
myapp.com/projects/12/tasks/193/comments/148
```

Try to avoid nesting more than 2 levels deep, as it will gum up your link helpers in Rails, and create unnecessarily long URLs.

You should also consider using shallow nesting, which results in routes like this:

```
myapp.com/projects/12
myapp.com/projects/12/tasks
myapp.com/projects/12/tasks/new
myapp.com/tasks/193
myapp.com/tasks/193/comments/
myapp.com/comments/148
```

This gives you the semantic benefit of nested routes, but without the long URLs.

##### Resources
[Rails Routing Guide](http://guides.rubyonrails.org/routing.html)

## Don't use the and / or keywords

Avoid the keywords ```and``` and ```or```, as they behave slightly differently than ```&&``` and ```||```, and it's not worth the added confusion.

## Test-Driven Development

##### RSpec

Although Rails ships with a test suite, it's a good idea to rip that sucker out and replace it with RSpec - it has a more human-like syntax and a few extra cool features, which are very helpful when running large numbers of tests.

##### Naming Specs

Don't write tests that start with "should". This gets very repetative and your descriptions will begin to run together. Make you test names active and use the present tense.

Bad:
``` ruby
describe User do
  it "should require a valid email" do
    ...
  end
end
```

Good:
``` ruby
describe User do
  it "requires a valid email" do
    ...
  end
end
```

##### FactoryGirl

Be careful when using FactoryGirl. Every time you call `FactoryGirl.create` you're hitting the database, which gets expensive. Unless you need your record to persist, use the `FactoryGirl.build` method:

``` ruby
# spec/models/user_spec.rb

describe User do
  it "requires a valid email" do
    user = FactoryGirl.create(:user)
    ...
  end
end
```

##### Use Contexts to Group Specs

RSpec has two high-level block types: describe blocks and context blocks. The two are the same, functionally, but have an important contextual difference. **Describe blocks** are used to group specs that test similar pieces of functionality. **Context blocks** are generally used inside of describe blocks to group specs that test functionality when a particular set of conditions are the same.

For example, here we wrap all of our specs in a describe block because they are all testing the User model. We have two separate contexts, though. One for when the user is male and one for female.

``` ruby
describe User do

  before(:each) do
    @user = FactoryGirl.build(:user)
  end

  context "male" do
    before(:each) do
      @user.sex = "male"
    end

    it "sometimes wears dirty socks" do
      ...
    end

    it "leaves the toilet seat up" do
      ...
    end
  end

  context "female" do
    before(:each) do
      @user.sex = "female"
    end

    it "really likes chocolate" do
      ...
    end

    it "is the boss" do
      ...
    end
  end

end
```

##### Resources
[FactoryGirl](https://github.com/thoughtbot/factory_girl)  
[Rspec](https://github.com/rspec/rspec)  
[How did you learn Rails testing (preferably RSpec)? (SO)](http://stackoverflow.com/questions/9934351/how-did-you-learn-rails-testing-preferably-rspec)

## Be careful with .dup, .clone, and .freeze

Ruby includes a few basic object methods, some of which are fairly nuanced.

##### .freeze

```.freeze``` lets you "lock" an object to prevent future changes (like they're your children that you don't want to grow up):

``` ruby
class Person
  attr_accessor :grown_up
end

jon = Person.new
jon.grown_up = false
jon.freeze

jon.grown_up = true	# RuntimeError, jon is a frozen object
```

Arrays are a bit trickier. When you freeze an array, the array object is frozen, but not the objects that make up the array:

``` ruby
numbers = ["one", "two", "three"]
numbers.freeze

numbers[2] = "four" 	# RuntimeError, you can't modify what objects make up the array

numbers[2].replace("four")		# allowed, you aren't changing the array, just a specific object
numbers   # ["one", "four", "three"]
```

##### .dup vs .clone

Both methods make a copy of the receiving object, but there are some slight differences:

```.dup``` does not copy singleton methods, ```.clone``` does

``` ruby
this = Object.new
def this.size
  99
end

this.dup.size   	# NoMethodError
this.clone.size 	# 99
```

The "Frozen" state is not preserved via ```.dup```, but is with ```.clone```

``` ruby
class Person
  attr_accessor :name
end

jon = Person.new
jon.name = "Jon"
jon.freeze

jon.dup.name = "Greg"   # Allows the change
jon.clone.name = "Greg" # RuntimeError
```

## The try() Method
Super handy, easy to forget about.

The `try()` method (Rails only) attempts to call a given method on an object and rescues if it encounters a nil response. That means that this:

```erb
<% if @user.hometown %>
  <%= @user.hometown.name %>
<% end %>
```

...can become this:
```erb
<%= @user.hometown.try(:name) %>
```

## Non-Database-Backed Models (Rails)

Many times, you'll be working with a form that requires validation, but doesn't necessarily need to hit the database (ex: an email contact form). Rather than writting your validations by hand, you can lean on ActiveModel using a [non-database-backed](https://gist.github.com/nathanhackley/872ef14b18767ca81355) model.

Basically, this means you can create objects that behave like standard Rails models, without binding them to a database table.

## Default Scope (Rails)

Don't use it unless there's a really good reason to. It will make your life Hell.

## Unless / Else

Don't do this, it's confusing:

```ruby
unless @user.boy?
  puts "Girl!"
else
  puts "Boy!"
end
```

Do this:
```ruby
if @user.boy?
  puts "Boy!"
else
  puts "Girl!"
end
```
