#### Barrel Development Best Practices

#Ruby on Rails Best Practices
----------------------------

## Test-Driven Development
Write your tests first, then write the code that makes the tests pass. This helps to ensure that you write the fewest lines of code needed to get the desired functionality. Plus, it will make your life easier down the road by potentially saving you hours of work when something breaks.

**Resources**

Rspec: https://github.com/rspec/rspec  
How Ryan Bates uses tests: http://railscasts.com/episodes/275-how-i-test




## Avoid expensive text helpers
Rails come with a ton of awesome helper methods for modifying text. 

**A few common ones:**

- ```time_ago_in_words``` – converts a timestamp to a string like "about 10 minutes ago" or "2 years ago"
- ```truncate``` – shortens a string
- ```pluralize``` – uses Rails' language engine to conditionally pluralize a word

In moderation, these helpers are fine, but keep in mind that they are fairly expensive and can easily be deferred to the client-side.

**Resources**

TextHelper API: http://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html





## Defer non-critical, expensive jobs
If a request includes a labor-intensive job (such as sending an email to a user), that is not critical to the response you are sending to the user, then that job should be deferred to a queue. 

**The default process:**

1. User send request – *client side is now blocked*
2. Server receives request – *server is now blocked*
3. Server completes basic functions
4. Server completes the expensive function – *server is now unblocked*
5. Server sends response to client – *client side is now unblocked*

**With a job queue:**

1. User send request – *client side is now blocked*
2. Server receives request – *server is now blocked*
3. Server completes basic functions and queues the expensive job – *server is now unblocked*
4. Server sends immediate response to client – *client side is now unblocked*
5. Server completes the expensive function whenever a processor is free


Obviously, if you need to include the result of the expensive job in your response to the client, then this is not an option.

The gem "Delayed Job", which was extracted form the Shopify code base, makes all of this super easy. Plus, it plays nice with hosting plans like Heroku.


**Resources**

Delayed Job: https://github.com/collectiveidea/delayed_job  
Railscast: http://railscasts.com/episodes/171-delayed-job-revised  
Heroku Documentation: https://devcenter.heroku.com/articles/delayed-job






## Use Turbolinks (when it makes sense)
As of Rails 4, Turbolinks is included in every app by default. 

Turbolinks speeds up page load time by replacing the ```<body>``` and ```<title>``` tags, rather than loading a whole new page. If you're developing a straightforward, page-based app, Turbolinks is probably going to help you out. If you're developing a javascript heavy app, you may want to think about excluding it.

**Known Issues:**

In earlier versions, there was an issue with Twitter Bootstrap and jQuery UI compatibility, but I believe these have all been worked out. However, if they do give you issues, there are various third-party gems that bridge the gap (notes in the Railscast on Turbolinks).

**Resources**

Turbolinks: https://github.com/rails/turbolinks/  
Railscast: http://railscasts.com/episodes/390-turbolinks





## Fat Models, Skinny controllers, and Scopes
Keep those controllers slim. Move any code that doesn't directly relate to the response into the model.

**Bad:**

controllers/tasks_controller.rb
``` ruby
def index
  @complete_tasks = Task.all :conditions => {['complete == ?', true]}
  @incomplete_tasks = Task.all :conditions => {['complete == ?', false]}
end
```

**Good:**

models/task.rb
``` ruby
scope :complete, lambda { where('complete == ?', true) }
scope :incomplete, lambda { where('complete == ?', false) }
```

controllers/tasks_controller.rb
``` ruby
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

**Resources**

Rails Routing Guide: http://guides.rubyonrails.org/routing.html








## Be careful with .dup, .clone, and .freeze
Ruby includes a few basic object methods, some of which are fairly nuanced.


**.freeze**

```.freeze``` lets you "lock" an object to prevent future changes (like they're your children that you don't want to grow up):

```
class Person
  attr_accessor :grown_up
end

jon = Person.new
jon.grown_up = false
jon.freeze

jon.grown_up = true	# RuntimeError, jon is a frozen object
```

Arrays are a bit trickier. When you freeze an array, the array object is frozen, but not the objects that make up the array:
```
numbers = ["one", "two", "three"]
numbers.freeze

numbers[2] = "four" 	# RuntimeError, you can't modify what objects make up the array

numbers[2].replace("four")		# allowed, you aren't changing the array, just a specific object
numbers   # ["one", "four", "three"]
```


**.dup vs .clone**

Both methods make a copy of the receiving object, but there are some slight differences:

```.dup``` does not copy singleton methods, ```.clone``` does

```
this = Object.new
def this.size
  99
end

this.dup.size   	# NoMethodError
this.clone.size 	# 99
```


The "Frozen" state is not preserved via ```.dup```, but is with ```.clone```

```
class Person
  attr_accessor :name
end
jon = Person.new
jon.name = "Jon"
jon.freeze

jon.dup.name = "Greg"   # Allows the change
jon.clone.name = "Greg" # RuntimeError
```










