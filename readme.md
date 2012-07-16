## ActiveRecord and Arrays

Now that we've started our projects we're going to take a look at some of the capabilities that ActiveRecord has to offer us in addition to manipulating Array's (a very valuable skill for any Rubyist). We're going to focus heavily on documentation and how to look stuff up. 


## Install

Fork and clone the repo to your local machine. Navigate to this directory and run:


    $ bundle install

Then follow the directions in this readme.

## 1) Ruby Docs && Arrays

For the first part of this exercise we'll be doing something a bit different. Rather than try to cram everything we need to learn into a Rails project we're going to do some Ruby exercises. In addition to learning about arrays, we'll also learn a little about testing. In a programming language as flexible and dynamic as Ruby, writing tests are a good way to know that your logic is valid. You make assertions and when you run the tests, you find out if your assertions were correct or not. 

There are many different testing frameworks and many styles of testing. You may hear of TDD (test driven development). For now we are going to use 'test/unit' which comes with Ruby. If you open the file `test/array_test` you'll see our test class `ArrayTest`. It inherits from `Test::Unit::TestCase`. We define tests by making new methods with distinct method names that start with `test_`. Here I've written a number of tests, it is your job to get them passing. In programming when two people code together and one writes the tests and the other gets them to pass, this is called ping pong pairing! Though usually they swap between tests.

Run the test file by navigating to this directory in your terminal and then running the test file with ruby

    $ ruby test/array_test.rb

You should see a number of failing tests go by, at the bottom you will get a recap of which tests passed and which failed like this:

    Finished tests in 0.000928s, 1077.5862 tests/s, 1077.5862 assertions/s.

    1 tests, 1 assertions, 0 failures, 0 errors, 0 skips

Your numbers will be different. Once you've confirmed that we have failing tests we'll need to make them passing. 

In the output you should see a line that looks like this:

    test_true(ArrayTest) [test/array_test.rb:6]:
    Failed assertion, no message given.

Here it is telling us that in the class ArrayTest which lives in the file `test/array_test.rb` we got a failure in a method called `test_true`. Open up `test/array_test.rb` and find that method. It should look like this:


    def test_true
      assert false
    end

You can fix this test by changing the false to true. Do this and run the tests again. You should now see one less failing test.

Before we go any further find the ruby documentation on Array's, use Google if you have to. Once you've found the Array page find the `Array#reject` method. This is an alias to `Array#delete_if` though I like the semantics better, since the method is aliased we must find the `Array#delete_if` documentation. Once you've found it, copy the link and enter it into Question # 1 in `questions.md`. If you're having a hard time finding it use the navigation on the left of the page your link should contain an anchor character `#` and if you paste it into chrome, you should go straight to the `delete_if` method.

Now take a look at the example from the docs, you should see something like this:

    a = [ "a", "b", "c" ]
    a.delete_if {|x| x >= "b" }   #=> ["a"]


Here we are setting up a junk array to variable `a` with the values "a", "b", and "c" we then call the `delete_if` method on that array and delete if the element is greater than or equal to "b". This might seem unusual, but consider in computer programming all characters are mapped to integers. All the characters that match that criteria are removed from the array, this leaves us with only "a". 

If you look at the docs again you'll notice this line:

    delete_if {|item| block } → ary


This is short hand to show us one way of calling `delete_if`. In this short hand we see we can pass it a block, and that it will return us an Array. This is why in the above example the result was `["a"]` and not simply `"a"` because this for of the method will always return an array.

Feel free to open up IRB and play with this method. While I'm developing I often have a session open (or `rails console` if I'm working with Rails). When I find a method I haven't used before I copy and paste the examle into IRB and make sure it runs, then I try to make up my own example and make sure I get a response that makes sense to me. For `delete_if` I might try something like this:


    a = [1,2,3,4,5]
    a.delete_if {|item| item == 2 || item == 5 }

Before I run it I make a mental note of what I think the outcome should be, in this case we are deleting if the element in the array is 2 or if it is 5 so I would expect [1,3,4] to be the output. Then I would go ahead and run it:

    $ irb
    > a = [1,2,3,4,5]
    > a.delete_if {|item| item == 2 || item == 5 }
    => [1, 3, 4]

Perfect, worked just like we thought it would. Since reject is an alias of delete_if, lets try replacing `delete_if` with `reject` and see if we get the same outcome:


    a = [1,2,3,4,5]
    a.reject {|item| item == 2 || item == 5 }
    # => [1, 3, 4]


Awesome. Now that we've got a handle on reject lets fix a test together. Open up your `tests/array_test.rb` and find a test called `test_reject`. Here I've already written a failing test that looks like this:


    def test_reject
      array = [1,2,3,4,5,6,7,8]
      # put your code here
      assert_equal [1,3,5,7], array
    end

I want you to use the `Array#reject` method to make the test pass without changing the first or last line. Let's do this one together.

In programming there is are seemingly unlimited ways to complete a given task. As you learn more you'll be better equipped to pick out the right tools for the right jobs. Just as a master carpenter wouldn't pick a screwdriver to hammer in a nail. For now we're more focused on getting the nail in than how it got there, but lets consider how we could get these two arrays to equal one another with the `reject` method. 

First of all, reject takes out items...so it helps that [1,3,5,7] are all elements in the variable `array`. If we look at the difference between the two you'll notice we're missing `[2,4,6,8]` so while I'm pretty sure we could get away writing something like this:

    array = array.reject {|x| x == 2 || x == 4 || x == 6 || x == 8}

It's not super easy to read and i'll bet there is some way to see if a number is divisible by to (which 2,4,,6 and 8 are). So lets take a look at the docs for our numbers. In IRB let's get the class of our number:


    $ irb
    > 8.class
    => Fixnum

So we're using the `Fixnum` class. Find the `Fixnum` documentation online. The easiest way to find it is with google. The second easiest is to navigate to the `Core` section of the ruby docs and find `Fixnum`.

> Enter the url to the fixnum documentation down in `questions.md`


If you've done computer science before you might be expecting this next section to talk about the modulus operator `%`, if you want to use that you can. Note that the docs for it point at `Numberic.divmod` and then when you finally find the right method http://www.ruby-doc.org/core-1.9.3/Numeric.html#method-i-25 the example is a bit cryptic. Some methods are documented better than others you'll also find that the docs aren't always the best way to discover methods, but they're crucial for understanding them.

If you went down the `%` rabbit hole, let's go back to `Fixnum` docs and scan the available methods on the left side. You'll see that there are a number of methods such as `+` and `-` which don't look like methods at all (though in ruby they are!). If you keep reading until you hit the "e" methods you'll see we have a method called `even?`. Click on it

> Enter the url to the `even?` operator into questions.md`

So we could use this in our test example we could simply reject all the even elements. Open `test/array_test.rb` and find the `test_reject` method again, add this code between the two existing lines so it looks like this:

    def test_reject
      array = [1,2,3,4,5,6,7,8]
      array = array.reject {|x| x.even? }
      assert_equal [1,3,5,7], array
    end

Now run your tests again, you should see one less failure! Great job! There are several other ways to make this test pass for example using the `odd?` method with the bang operator (`!`) which reverses the logic: 


    def test_reject
      array = [1,2,3,4,5,6,7,8]
      array = array.reject {|x| !x.odd? }
      assert_equal [1,3,5,7], array
    end


Or we could even use the modulus operator we hinted at previously:

    def test_reject
      array = [1,2,3,4,5,6,7,8]
      array = array.reject {|x| x%2 == 0 }
      assert_equal [1,3,5,7], array
    end

At the end of the day, how you get the nail in doesn't matter so much for now. Pick the solution that is the most readable and understandable to you. Sometimes when I'm working on a project and can't find an easy way to manipulate something (like an array, string, hash, etc.) i'll flip through the available methods and see if something pops out that I hadn't seen before. Sometimes though, i know exactly what I want to do such as find an "even fixnum in ruby" and i'll search for it [even fixnum ruby](http://lmgtfy.com/?q=even+fixnum+ruby). I often find that the docs will be on the first page of results.

Hopefully by now you shouldn't feel completely intimidated by the docs. Answer a few more questions and then it will be time to set you loose on fixing all those test failures.

> Link to `Hash` docs:

> Link to `Hash.keys` docs:

> Link to `String` docs:

> Link to `String#upcase` docs:


Head back to the `Array` docs and we'll fix the rest of the tests. In addition to `assert` and `assert_equal` there are a number of different assertion statements we can use. Since are test class inherits `Test::Unit::TestCase` we can look at those docs. From there you'll see that we include a module called `Test::Unit::Assertions` here you'll find quite a few assertions we could have used http://www.ruby-doc.org/stdlib-1.9.3/libdoc/test/unit/rdoc/Test/Unit/Assertions.html. 

Go through the rest of the methods in the `test/array_test.rb`. Switch between writing code and looking up docs. Once you get a test to pass move on to the next until all are passing. There are comments above each method that should help you complete your task.

When all the tests are passing, commit your questions and the fixed tests to git.


If you think you're being super smart and look at other people's pull requests, you're only hurting yourself. If you get stuck use Google or StackOverflow. Feel free to work with others, but make sure you understand how to get all of the answers by yourself.



#2) ActiveRecord

Okay, you've survived the onslaught of Array questions. Hopefully you've got some awesome array methods under your belt and you're ready to tackle ActiveRecord. Like we did last time we're going to be using tests. If seeing a bunch of failures bothers you while you're coding you can go through and add `skip
` to the first line of tests, making sure to remove them as you're working on getting that test fixed. We will be using ActiveRecord without Rails in this exercise. Because of that you won't have the usual tools like `$ rails console` to play around with queries. Open up `test/active_record_test.rb`. 

In the first few lines you'll see that we're requiring a relative file `active_record_support.rb` this code will create an in memory SQLite database and using ActiveRecord. 


If you want to use this in your irb you can copy the contents of `support/active_record_support.rb` and paste it into irb. You can then create `Dealerships` and `Cars`. To help you out you can also copy the code in `support/faek_data.rb` and paste it into the same irb session. This will create a number of fake dealerships and cars for you to run SQL queries on. 

In the last section we looked for ruby documentation. In this section we'll be using ActiveRecord which is also the ORM that Rails uses. Since it does not come with Ruby, you will have to find the ActiveRecord documentation, some of it is included in Rails documentation and some of it is stand alone. There are two main sources for rails documentation, one is method documentation like we saw with ruby and the other is a set of Guides. The guides are structured in more of a storytelling manner and more focused on examples than technical implementation. First off open a new tab and visit http://guides.rubyonrails.org/active_record_querying.html then find the link to conditions on the right and click on `Conditions` add this to your questions file:

> Link to Rail's ActiveRecord querying conditions 

Here we see much of what was discussed in class, how to avoid SQL injection and how to use some common query methods. The Rails Guides are by far my favorite resource when working with Rails code. This page is both comprehensive and understandable. If you're currious about a specific active record condition this would be a great place to start.

The second place you'll find docs is on http://api.rubyonrails.org. Visit http://api.rubyonrails.org and on the left side you can search for documentation based on class name. Here we're using ActiveRecord::Base so you can select ActiveRecord and then Base. You should now see something that looks like http://api.rubyonrails.org/classes/ActiveRecord/Base.html. This documentation is much closer in style to the Ruby documentation. It is mostly auto generated through a program called rdoc that allows programmers to embed documentation inside of comments along side of their code. There are other programs available (I personally prefer [YARD](http://yardoc.org/) to document code). The quality of this documentation can vary greatly between topic, class, and method. Sometimes I prefer to use APIdock to browse the Rails docs. It is the same documentation, but I prefer the layout. Go ahead and find the apidock link to ActiveRecord::Base, add this to your questions:

> Link from APIdock to Rail's ActiveRecord querying conditions  

While we're linking things add a link to t-he Rails guides to your question file.

Now that we've got our docs all ready we're set to fix some tests.


Run:

    $ ruby test/active_record_test.rb 


You should see some tables being created:


    -- create_table(:dealerships)
       -> 0.0091s
    -- create_table(:cars)
       -> 0.0008s

Don't worry this is only "in memory" or in RAM. As soon as we're done running our tests our database will be gone, so we can keep running the same tests. Because of this we can add data to our database every time it is run to perform our tests. I've already added data, but now it's your job to pull it out.

Lets take a look at `test_regular_where` in your tests file. It should look something like this:


    def test_regular_where
      name = Faker::Company.name
      d    = Dealership.create(:name => name)
      # put your code here
      assert_equal d, dealership
    end

Here we're using a library called Faker to provide us with human readable fake names. First we assign a name variable, then add a dealership to the library. Now it's our job to pull the same dealership from the database and assign it to a variable of `dealership` we can do that using a where query and our hash syntax. Since we don't know what the random name variable is, we'll need to use it again in our query. This will make the tests pass:


    def test_regular_where
      name = Faker::Company.name
      d    = Dealership.create(:name => name)
      dealership = Dealership.where(:name => name).first
      assert_equal d, dealership
    end


We could have also used the string substitution method (?).

    Dealership.where("name = (?)", name).where

By now you should be fairly used to finding records like this. Run the tests to make sure you got that one passing. Continue on with the rest of the test unitl you get all of them passing.


When all the test pass commit to git and push to github.


## Fin

That's it. You got your first taste of tests, and got a somewhat forced introduction to documentation. Programming is kinda complicated, so the easier you can make it on yourself with docs and google the quicker you will improve. While they're helpful, they're not a magic bullet. Spending a few minutes browsing documentation can save you hours of hair pulling. Not that you have to read word for word every bit of docs on every page you see, but you'll certainly develop an appreciation for critical reading. 

You got a healthy dose of Array's and ActiveRecord. Hopefully these will help you with your projects. As a Ruby programmer especially if you're working with Rails, the two are invaluable resources. If you feel like you didn't really learn anything, and that you cheated your way through by looking everything up...don't worry, you're not expected to keep every method definition of very class in your head. In fact some of the best programmers I know constantly check what they think they know with docs. 

