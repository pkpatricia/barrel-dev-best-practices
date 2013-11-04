#### Barrel Development Best Practices

#Ruby / RoR Best Practices
----------------------------

## Test-Driven Development

Do it.

##### What is TDD?
With TDD, you write your specs(tests) first, then write the actual code that makes the specs pass. This helps you make sure that you're writting the fewest lines of code necessary to get the desired functionality.

Writing good specs also gives you a nice dev guide that breaks the app up into small, managable chunks - it forces you to focus.

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

FactoryGirl is a gem that replaces the default Rails "fixtures" with what they call "factories". This helps DRY up your test code by letting you do cool stuff like:

``` ruby
# spec/factories/user_factories.rb
require 'factory_girl'

FactoryGirl.define do

  factory :user do
    name 'Jason Bourne'
    email 'jason.bourne@cia.gov'
    password 'TerroristsSuck!'
  end

end
```

Which can be used in your tests to build a new User object:

``` ruby
# spec/models/user_spec.rb

describe User do
  it "requires a valid email" do
    user = FactoryGirl.build(:user)
    ...
  end
end
```


Be careful, though. Factories can be expensive, and when you have 200+ tests running, it can really slow you down. To help alleviate this, use standard Ruby techniques when a factory isn't completely necessary.

Good:
``` ruby
describe User do
  it "requires a valid email" do
    user = FactoryGirl.build(:user)
    ...
  end
end
```

Maybe Better, if you don't need all of the user's attributes:
``` ruby
describe User do
  it "requires a valid email" do
    user = User.new(email: "jason.bourne@cia.gov")
    ...
  end
end
```


##### Resources
[FactoryGirl](https://github.com/thoughtbot/factory_girl)  
[Rspec](https://github.com/rspec/rspec)  
[How did you learn Rails testing (preferably RSpec)? (SO)](http://stackoverflow.com/questions/9934351/how-did-you-learn-rails-testing-preferably-rspec)









## Avoid expensive text helpers
Rails come with a ton of awesome helper methods for modifying text. 

##### A few common ones:

- ```time_ago_in_words``` – converts a timestamp to a string like "about 10 minutes ago" or "2 years ago"
- ```truncate``` – shortens a string
- ```pluralize``` – uses Active Support's fancy language smarts to conditionally pluralize a word

In moderation, these helpers are fine; but keep in mind that they are fairly expensive and many can easily be deferred to the client-side.

##### Resources
[TextHelper API](http://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html)








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









## Use Turbolinks - when it makes sense
As of Rails 4, Turbolinks is included in every app by default. 

Turbolinks speeds up page load time by replacing the ```<body>``` and ```<title>``` tags, rather than loading a whole new page. If you're developing a straightforward, page-based app, Turbolinks is probably going to help you out. If you're developing a javascript heavy app, you may want to think about excluding it.

##### Known Issues:

In earlier versions, there was an issue with Twitter Bootstrap and jQuery UI compatibility, but I believe these have all been worked out. However, if they do give you issues, there are various third-party gems that bridge the gap (notes in the Railscast on Turbolinks).

##### Resources
[Turbolinks](https://github.com/rails/turbolinks/)  
[Railscast on Turbolinks](http://railscasts.com/episodes/390-turbolinks)











## Fat Models, Skinny controllers, and Scopes
Keep those controllers slim. Move any code that doesn't directly relate to the response into the model.

##### Bad:

``` ruby
# controllers/tasks_controller.rb

def index
  @complete_tasks = Task.all :conditions => {['complete == ?', true]}
  @incomplete_tasks = Task.all :conditions => {['complete == ?', false]}
end
```

##### Good:

``` ruby
# models/task.rb

scope :complete, lambda { where('complete == ?', true) }
scope :incomplete, lambda { where('complete == ?', false) }
```

``` ruby
# controllers/tasks_controller.rb

def index
  @complete_tasks = Task.complete.all
  @incomplete_tasks = Task.incomplete.all
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










