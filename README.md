## What You Will Learn

This assignment will teach you about methods and return values. Methods are one of the most important building blocks in any programming language. Return values are what methods produce when used.

# Assignment

1. [Methods](#methods-and-return-values)
1. [Analogy - Daily Tasks](#daily-tasks)
1. [Return Values](#return-values)
1. [Arguments/Parameters](#argumentsparameters)
1. [Exercise 1](#exercise-1)
1. [Methods are on Objects](#methods-are-on-objects)
1. [Naming methods](#naming-methods)
1. [Inputs and Outputs](#inputs-and-outputs)
1. [Number of Return Values](#number-of-return-values)
1. [Exercise 2](#exercise-2)
1. [Implicit Returns](#implicit-returns)
1. [Exercise 3](#exercise-3)
1. [Exercise 1](#exercise-1)
1. [Exercise 1](#exercise-1)
1. [Further reading](#further-reading)



## Prerequisites
* Have Ruby (and `irb`) set up
* data types in Ruby (numbers, booleans, arrays)
* Conditionals
* Iteration (basics, toward the end)

# Methods and Return Values

In their simplest form, a *method* is a set of instructions for the computer to run. You might say that the entire program is a set of instructions, so here is a more specific attempt:

> A method is a set of instructions about a single thing.

Let's explore that idea. We'll refine our definition later on.

## Daily tasks
Your boss tells you to upload a new article onto the company's website, and to take out the garbage when you are finished.

How many items did they give you to complete? This pretty clearly reads as two items. In software, this should therefore be treated as two items. For example, in Ruby:

```ruby
def upload_article
  # commands to do with uploading articles onto the website
end

def take_out_garbage
  # commands to do with taking out the garbage
end
```

The above is an example of how we *define* methods in Ruby (hence `def`). We're not actually performing the actions yet. Contained in each of these methods would be the steps to complete those tasks. And those tasks could be performed any number of times. Maybe tomorrow your boss will ask you to do the same thing. In that case, you hopefully don't need to ask again how to take out the garbage or upload articles. The instructions haven't changed - only the day.

We could now *call* those methods to actually achieve them.

```ruby
upload_article # an article should now be uploaded
take_out_garbage # the garbage can should now be empty
```

So far, we haven't saved any lines of code by defining methods. If your boss asks you to do the same tasks tomorrow, however, we can now simply do exactly the same thing:

```ruby
upload_article # another article should now be uploaded
take_out_garbage # the garbage can should be empty again
```

It's the same thing as above, but notice that we didn't need to redefine the methods (`def upload_article`, etc.). The methods are just instructions, and we're carrying out those instructions on a different day. We remember the instructions and can perform them on any day, just by *calling* the relevant method - `upload_article` or `take_out_garbage`.

Why did we need to separate these into two different methods? Because your boss might ask you to only accomplish one of those things. It would be weird if you were asked to take out the garbage, but then sat at your desk trying to upload an article. These are two very separate things, and they should be treated that way. We can simply call `upload_article` or `take_out_garbage` if we only wish to achieve one of them.

You might find, however, that you are being asked to do the same set of things every morning. In other words, you are given the same set of high-level instructions every day: upload the article, then take out the garbage. And a method is just a set of instructions. So can we group these two things together? Let's try to do so:

```ruby
def perform_morning_tasks
  # commands to do with uploading articles onto the website
  # commands to do with taking out the garbage
end
```

Our code is shorter, but now we can't ever perform only one of these tasks. Let's try again, leaving the original two methods as they were:

```ruby
def upload_article
  # commands to do with uploading articles onto the website
end

def take_out_garbage
  # commands to do with taking out the garbage
end

def perform_morning_tasks
  upload_article
  take_out_garbage
end
```

`perform_morning_tasks` is now a set of high-level instructions - do the two things we already know how to do. And those tasks get their own methods, because they are separate tasks that we might want to perform in isolation.

In the morning, now, we can save a line of code and simply do this:

`perform_morning_tasks`

If there is no garbage today, you can simply do `upload_article`. Similarly, if there is no article to be uploaded, you can still `take_out_garbage`.

The task of grouping methods becomes more challenging the more code you write. If done well, however, the payoff is very high in terms of readability. Go back and read `perform_morning_tasks`. Look how easy it is to understand its responsibilities!

> If you have a method name with the word `and` or `or` in it, there is a good chance your method is doing at least two things.
Split it up!

## Return values

So far, your boss has seemed pretty aloof because they haven't asked you to tell them to report back. If you tried to upload an article but it didn't work, you should make sure you tell your boss.

Let's expand on our empty `upload_article` method to provide a return value.

```ruby
def upload_article
  # commands to do with uploading articles onto the website
  return true
end
```

You can return any object from a method in Ruby, but `true` and `false` are very common. They are a simple way to tell your program that everything went well or that it didn't work.

Now, therefore, you are always telling your boss that you succeeded in uploading an article, even if you failed to do so. Let's fill in some pieces of the method to allow for us to handle different cases/conditions.

```ruby
def upload_article
  # commands to do with uploading articles onto the website
  # let's assume the variable 'it_worked' is now true or false
  if it_worked
    return true
  else
    return false
  end
end
```

or, more simply:

```ruby
def upload_article
  # commands to do with uploading articles onto the website
  # let's assume the variable 'it_worked' is now true or false
  return it_worked # returns true or false because it_worked is true or false
end
```

Now your boss can act on the success/failure of your task. It's up to them to do whatever they want with that information. Let's look from their perspective, then: they are now telling you to perform certain tasks.

If they don't care about whether you succeeded or failed (maybe not a good boss!), they can simply say:

`upload_article`

Now you'll upload upload the article - and that's all. If they want to actually do something with that information, they might save it into a variable and do something with it:

```ruby
upload_success = upload_article
if upload_success
  # congratulate prized employee!
end
# ...and so on
```

To be clear, then, if `upload_article` returned `true`, then the first line in the above example was equivalent to:

```ruby
upload_success = true
```

Or `upload_success = false` in the other case. In any of these example, we say that we are saving the result (or return value) of the method call `upload_article` into the variable `upload_success`. And if you mentally substitute the return value of a method like that, it's much easier to see what is going on in your code.

In fact, returning a value is the **primary purpose of a method**. Programming is all about the flow of information in your program. From an information standpoint, then, it doesn't really matter what `upload_article` accomplished if it doesn't tell you anything. You can't act on nothing! Anything else the method accomplishes is considered to be a *side effect*.

It may not be obvious to you at this point that you need to consider whether `upload_article` succeeds or fails, so let's use a more obvious example: we want to get a random number between 0 and 1.

There is a Ruby method that will do this for us called `rand`. If we simply call the method, it's pretty useless to us:

`rand`

But if we do something with its return value, it is much more useful:

```ruby
num = rand # now, for example, num is 0.65832, which is between 0 and 1
if num > 0.5
  # do something cool
end
```

In the above example, we are saving the result (or return value) of the method call `rand` to a variable called `num`. We don't always need to save the result into a variable, though, as long as we don't plan on reusing it:

```ruby
output = "The number #{rand} is a random number."
puts output
```

Above, we are saving the result of calling `rand` into a string. We can't reuse our random number, but we can reuse the string we saved it into. We could also pass it directly into another method call:

```ruby
puts rand
```

Now `rand` is called, returning a value of (let's say) `0.342`, which is used by the `puts` method to output the random number to the screen. This is the equivalent of doing:

```ruby
puts 0.342
```

## Arguments/Parameters

Sometimes more information is needed to perform a task. For example, `upload_article` is actually pretty vague. Which article? Upload it to where? There could be other required data as well.

Any information passed into a method is called an *argument*. For example:

```ruby
upload_article 'This is my first post!'
```

The string `'This is my first post!'` is the **argument** to `upload_article`. We say that we are *passing* the argument. Another example would be passing the numbers `5` and `7` to a method called `add`:

```ruby
add 5, 7 # this had better return 12
add(5, 7) # the same thing!
```

We can just use the `+` sign normally, but it's a nice simple method as an example. Let's take a look at how the `add` method might be defined:

```ruby
def add(first_num, second_num)
  return first_num + second_num
end
```

Remember that we passed two pieces of information to the `add` method: `5` and `7`. When we define the `add` method, then, we need to accept both pieces of information. We call these **parameters**. First, let's see what happens when we don't use any:

```ruby
def add
  return first_num + second_num # error!
  # NameError: undefined local variable or method `first_num' for main:Object
end
```

We have to tell Ruby that we are waiting for two pieces of information. If we don't, we just don't have anything to work with in our method.

Notice the names of the parameters. We called them `first_num` and `second_num`, but they can be called anything you want! Descriptive names are always better, but this is still valid Ruby code:

```ruby
def add(purple, elephant)
  return purple + elephant
end
```

If we change the names of the parameters, we have to change our usage of them inside the method. We had to change all uses of `first_num` with `purple`.

So far we've only passed the method numbers: `5` and `7`. Let's do that again, but this time using variables:

```ruby
apples = 5
oranges = 7
total_fruit = add(apples, oranges) # should still be 12
```

It is common at this point to wonder about the names `apples` and `oranges`. Why did that work? Shouldn't we have to use `first_num` and `second_num`, or `purple` and `elephant`?

Short answer: no, the names you use when you call the method have nothing to do with the names *inside* of the method.

The first time we called `add`, we did `add 5, 7`. Did we care about the 'names' we passed in there? Nope! We only care about the *data*. In other words, given that this method is supposed to be adding things, we had better pass two items that can be added. In fact, they need not even be numbers:

```ruby
total_fruit = add('Hello', 'world!') # should be 'Helloworld!'
```

Notice that I didn't paste the definition of `add` along with this code. Why? Because we shouldn't really need to know what's inside of it. If the method is called `add`, we should expect that it will add things together. In fact, you will never see the code inside of most built-in Ruby methods. It's easier to just read about them in the documentation (a topic for another time).

The point here is that defining a method and calling one are two very separate tasks - they are *decoupled*. When you call a method, you shouldn't need to know what names are used inside of that method. Knowing what the method actually does is good enough.

Let's put it all together before moving on:

```ruby
def add(first_num, second_num)
  return first_num + second_num
end

apples = 5
oranges = 7
total_fruit = add(apples, oranges) # should be 12
cost = add(6, 13) # we can now use `add` whenever we want!

another_number = add(add(1, 1), add(1, 1)) # also valid
# equivalent to add(2, 2) -> another_number should therefore be 4
```

> ###### Note on terminology
>It is common to use `parameter` and `argument` interchagably. You don't need to remember the difference - just how methods work!

## Exercise 1
1. Write a method called `multiply_3` that multiplies three numbers together and returns the result.

1. Write a method called `cube` that takes a number and multiplies it by itself three times. It should return the result.
  * Hint: we already have a method that accomplishes part of this.

1. Write a method called `impress_friends` that takes a number as an argument. It should cube that number and return a string such as: `I know numbers bigger than 9261, do you?`


## Methods are on Objects

You haven't yet learned about *objects* in Ruby, but there is an important concept to briefly tackle.

Your boss hires another employee. Now there are two of you that can upload articles and take out the garbage (whew!). So he can't just yell "upload article!" into the void anymore and expect it to get done. Instead, he has to tell one of you to do it.

If we have a variable for each employee, perhaps the variable that represents you is `you` and the variable for the other employee is `ramona`. If your boss wants to get anything done, they will have to be more specific:

```ruby
ramona.upload_article("First article")
you.take_out_garbage
```

Looks like you didn't get out of garbage duty this time, but maybe tomorrow your boss wants things done differently:

```ruby
ramona.take_out_garbage
you.upload_article("First article")
ramona.upload_article("Second article")
```

It turns out that methods in Ruby are never isolated. They are not just a set of instructions, but a set of instructions *on* something. In our example, that something might be `Employee`. We won't go into the details of how to define such a thing now. The important takeaway is that whenever you call a method, that method is being called *on* something. For example:

```ruby
new_string = "abcde".reverse # new_string is now "edcba"

my_array.each do |item|
  puts item
end
```

Both the `reverse` method and the  `each` method are defined objects. Specifically, `reverse` is defined on both `String` (so it can be done on strings) and `Array` (so it can also be done on arrays). Similarly, `each` is defined on `Array` (so you can use it on arrays).

Whenever you see syntax using the "dot notation" (eg. `my_string.each`), we can figure out two things:

  1. `each` is a method
  1. `my_string` is some kind of object that understands `.each`.

This is a really important, and it will come up over and over again. You may, however, be wondering if the methods we define ourselves are defined on other things. It turns out that they are, though the details are not important. So when we used our `add` method, we were, implicitly, telling Ruby to do `something.add`, where we haven't taught you about the `something` part.

If that last point doesn't make sense, that's okay; what matters is that we know that all methods are defined on things, rather than in isolation. Therefore, let's refine our definition of "method" again:

> A method is a set of instructions defined on something. A method should have a single purpose.

That should do nicely. Again, you'll learn more about objects in Ruby soon enough! Feel free to come back to this section later in the course.


### Naming methods

If you look back at all of our method examples above, you will see that they are all verbs or actions. This is because methods are tasks or actions - they do things! If your boss tells you to "go to the store", then you might make a method called `go_to_store`.

They should also be given descriptive names. We don't know anything about this imaginary application, but we can bet that `go_to_store` involves going to a store. That might seem obvious, but it is common to see poorly named methods. For example, the method name `go` would tell you that it has to do with going, but nothing to do with the store. When in doubt, be more obvious! **Readability counts.**

>###### Method naming guide
Method names should be:
  * descriptive
  * verbs or actions
  * snaked_cased (rather than CamelCased)


## Inputs and Outputs

Your boss tells gives you a new task: "find me the longest book on the library website".

Before you even start thinking about how to *code* something like this, you need to understand two things:

1. Inputs
2. Outputs

Inputs and outputs are the two most important things about methods. Earlier we said that return values are the purpose of a method - and that's still true. But we don't know how to produce the return value unless we know what information we have to work with.

If we were writing a method to solve our boss's problem, the questions we would want to ask are:

* Which library?
* What do you want me to do with the book when I find it?
  * Eg. provide the name, place a hold on the book, etc.

The "Which library?" part is a question about the *input*. Maybe your boss doesn't care which library. Or maybe they want to be able to name a specific library and your method should return the largest book.

The second question is about the *output*. It's very important to consider what the purpose of your method is before you write it. In fact, if you're writing a method, you should generally know *why*. The output is the *why*.

Occasionally we don't care what a method returns. A simple example might be the method `puts` - it returns `nil` 100% of the time because its job is to output text onto the screen (not to return it). Our `add` method, on the other hand, is useless if it doesn't return anything.

>The two most important parts of methods are:
> 1. Inputs
> 2. Outputs


## Number of Return Values

We've already said that each method should be responsible for exactly one thing: checking the temperature, completing the daily tasks, etc. Similarly, each method is only allowed to return one value/object.

Let's first try to return multiple times and see what happens.

```ruby
def get_street_number
  return 5
  return 6
end
```

Stepping through this method, the first thing it does is `return 5` - and that's all. Once Ruby hits a `return` statement, the function stops. So it never even gets to `return 6`.

If you want a method to return multiple values - say, a list of addresses - then you can return an array:

```ruby
def get_addresses
  return ['25 Wilmer', '117 Yonge']
end
```

or a hash:

```ruby
def get_addresses
  return {
    'Doug' => '25 Wilmer',
    'Julia' => '117 Yonge'
  }
end
```


## Exercise 2

You're given a list of tasks to accomplish. Consider and write down what you think the inputs and the outputs of each would be. There are often multiple valid answers. Come up with a name for each method as well. Some outputs we've seen so far are `true`, `false`, `nil`, numbers, and strings, but remember that a method can return anything!

1. Build a house using bricks, wood, and glass.

1. Explore the moon! We have a launch pad and a rocket.

1. Please find my phone - I think I left it in the hallway downstairs.

1. Find a list of capital cities.

1. I could use a list of items in the grocery store with their aisle number.


## Implicit Returns

So far, whenever we have wanted to return a value, we have explicitly used the `return` keyword in Ruby. For example,

```ruby
def add(num1, num2)
  return num1 + num2
end
```

It turns out that Ruby tries to make this easier for us by not requiring the `return` keyword at all! For example, the following method is identical to above:

```ruby
def add(num1, num2)
  num1 + num2
end
```

How does Ruby know you want to return? If it doesn't find a `return` statement - those always take priority - it will return the *last evaluated expression*. In other words, the last thing that Ruby calculates will get returned. Let's try it out:

```ruby
def get_number
  two = 1 + 1
  3 * 4
end
```

Calling `get_number` will return `12` because `3 * 4` is 12. The method calculates `1 + 1`, then saves it into a variable. It then simply calculates `3 * 4`. Nothing exciting  yet - but we get to the end of the method without a return value! Ruby therefore returns the last thing we calculated - namely, `3 * 4`.

Another example:

```ruby
def offer_deal(amount)
  new_amount = amount * 0.95 # 5% off
  "I'm offering the car for #{new_amount}."
end
```

In this example, we calculate a `new_amount`, and then we implicitly return a string that contains that value (using string interpolation). The method did not find a `return` statement, so it returned the last thing it found: our string `"I'm offering the car for #{new_amount}."`.

This starts to get trickier when we use more complex code. Fortunately, `irb` can help us! Open it up if you haven't already. Put in a simple expression into `irb`:

```
irb(main):001:0> 2 + 5
=> 7
```

`irb` always prints out - wait for it - the last evaluated expression! (ie. `=> 7`) It's just that we normally only write one expression at a time in `irb`. This makes it a really powerful tool to understand return values in Ruby. It's impossible to understand implicit returns without knowing what your code is trying to return.

As another example:

```
irb(main):003:0> puts 'Hello'
Hello
=> nil
```

Notice that although the `puts` method prints "Hello" to the screen, it actually returns `nil`. Or, a multi-line example: let's save an array called `veggies` and perform an action on `each` vegetable:

```
irb(main):004:0> veggies = ['carrots', 'celery', 'potatoes']
=> ["carrots", "celery", "potatoes"]

irb(main):005:0> veggies.each do |veggie|
irb(main):006:1*   puts "I love #{veggie}!"
irb(main):007:1> end
I love carrots!
I love celery!
I love potatoes!
=> ["carrots", "celery", "potatoes"]
```

Our `each` statement, which took up three lines, ended up printing out:

```
I love carrots!
I love celery!
I love potatoes!
```

but actually returning:

```
=> ["carrots", "celery", "potatoes"]
```

Apparently `each` returns the array of items you gave it! If we were to write a method that ended with an `each` statement, then, we know what it would return: the array of items you were iterating over.

You don't have to memorize what every method in Ruby will return. It's easy to test - just call it in `irb`! The more often you experiment with and explore the code you write, the better you'll get at programming!

If you want to try this inside of a file instead of in `irb`, simply save whatever you're interested in into a variable:

```ruby
my_return_val = veggies.each do |veggie|
  puts "I love #{veggie}!"
end
```

Now `my_return_val` should have the same value as `veggies`! It turns out that everything in Ruby has a re



## Exercise 3

Try each of the below code snippets and determine what they return:

1.
```ruby
3 + 7
```

2.
```ruby
puts 3 + 7
```

3.
```ruby
num = 5
if num > 3
  "Hello"
end
```

4.
```ruby
def my_method
end
```

5.
```ruby
def express_love(items)
  items.each do |item|
    puts "I love #{item}!"
  end
end
```

6.
```ruby
toys = ['horse', 'car', 'transformer']
express_love(toys) # using method from above
```

7.
```ruby
puts express_love(toys)
```




# Further Reading

There's lots to learn! You will gain a stronger understanding of methods and return values with time and effort.

### Code exercises
*


### Great books
*

## Additional Resources
*
