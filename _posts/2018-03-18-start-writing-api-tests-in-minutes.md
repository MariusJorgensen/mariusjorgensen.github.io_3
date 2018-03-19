---
title: "Start writing API tests in minutes!"
#tags: [code, cucumber, bdd, airborne, rspec, ruby, api, development, testing, test automation, automation, automation in testing]
screenshot_snippet:
  - url: /assets/posts/snippet.png
    image_path: /assets/posts/snippet.png
screenshot_pending:
  - url: /assets/posts/pending.png
    image_path: /assets/posts/pending.png
screenshot_success:
  - url: /assets/posts/success.png
    image_path: /assets/posts/success.png
screenshot_success_status:
  - url: /assets/posts/success_status.png
    image_path: /assets/posts/success_status.png
screenshot_failing_status:
  - url: /assets/posts/failing-response-codes.png
    image_path: /assets/posts/failing-response-codes.png
screenshot_user_id:
  - url: /assets/posts/user-id.png
    image_path: /assets/posts/user-id.png
layout: single
author_profile: true
read_time: true
comments: true
---

Wouldn't it be great to have a framework so you can test your API in your business' domain language?

It could result in more people across your company being able to contribute to testing the API – with real life scenarios!

Or what about helping your team building the right stuff. No more misinterpretation of a new API feature. It's already been planned, and the tests have already been written!

Well, I can tell you now. By the time you’ve finished this quick intro, you'll have all the tools to do that! Great, huh?

### The Tools

* [Cucumber-ruby](https://github.com/cucumber/cucumber-ruby) - BDD framework for writing tests in the business domain
* [Airborne](https://github.com/brooklynDev/airborne) - RSpec driven API testing framework

### Prerequisites

* [Ruby](https://www.ruby-lang.org/en/)
* [Bundler](http://bundler.io/)

### Installing everything you need

Create a new directory. I’m calling mine `api-test-automation-framework`

{% highlight terminal %}
mkdir api-test-automation-framework
cd api-test-automation-framework
{% endhighlight %}


Create a `Gemfile`, and put the only two Gems you need in it


{% highlight ruby %}
# Gemfile

source 'https://rubygems.org'

gem 'cucumber'
gem 'airborne'
{% endhighlight %}

That was easy! Wasn’t it? Imagine that! Two Gems!

Next thing you want to do, is run bundle install

{% highlight terminal %}
bundle install
{% endhighlight %}

Now that’s done, you can initialise cucumber by running

{% highlight terminal %}
cucumber --init
{% endhighlight %}

This will create the file structure you want

You can now edit the file `features/support/env.rb`

{% highlight ruby %}
# features/support/env.rb

require 'bundler/setup'

Bundler.require

include Airborne
{% endhighlight %}

What these lines does, is to require whatever is in your `Gemfile`, and including the [Airborne](https://github.com/brooklynDev/airborne) library, so that all of the built in tools are available for your tests.

And guess what – no more setup needed!

![](http://i.giphy.com/5wWf7GW1AzV6pF3MaVW.gif?style=centerme)

Now shall we? Yeah, let’s do it. Create our first test! Woho, how exciting!

### Note

The first part of this guide will be very down to the basics, so if you know a bit of Ruby and how Cucumber works, you can just skip ahead to [this part](#using-airborne).

### Your first test
You can test whatever you want, but just to be completely sure that we have everything wired up, let’s do a simple hookup test

Create a new file in the `features` directory, and call it `hookup.feature`

{% highlight gherkin %}
# features/hookup.feature

Feature: API Testing with Cucumber

Scenario: Failing Cucumber Hookup Test
  Given I have set up this project correctly
  When I add 2 + 2
  And I expect that the answer is 5
  Then this scenario will fail

Scenario: Passing Cucumber Hookup Test
  Given I have set up this project correctly
  When I add 2 + 2
  And I expect that the answer is 4
  Then this scenario will pass
{% endhighlight %}

Now; run

{% highlight terminal %}
cucumber
{% endhighlight %}

Since you haven’t implemented the code yet, the steps will be undefined, and cucumber will create the basic step definition code for you, the output should be something like

{% include gallery id="screenshot_snippet" %}

Copy the code snippet suggested by Cucumber, and create a new file: `features/step_definitions/hookup.rb`

{% highlight ruby %}
# features/step_definitions/hookup.rb

Given("I have set up this project correctly") do
  pending # Write code here that turns the phrase above into concrete actions
end

When("I add {int} + {int}") do |int, int2|
  pending # Write code here that turns the phrase above into concrete actions
end

When("I expect that the answer is {int}") do |int|
  pending # Write code here that turns the phrase above into concrete actions
end

Then("this scenario will fail") do
  pending # Write code here that turns the phrase above into concrete actions
end

Then("this scenario will pass") do
  pending # Write code here that turns the phrase above into concrete actions
end
{% endhighlight %}

Hit save, and give…

{% highlight terminal %}
cucumber
{% endhighlight %}

...another go

Your output will now hopefully be something similar to

{% include gallery id="screenshot_pending" %}

Let’s write some code to see this test come alive

{% highlight ruby %}
# features/step_definitions/hookup.rb

Given("I have set up this project correctly") do
  # Leaving this step empty for enhanced readability of scenario
end

When("I add {int} + {int}") do |int, int2|
  @addition = int + int2
end

When("I expect that the answer is {int}") do |int|
  @expected_answer = int
end

Then("this scenario will fail") do
  expect(@addition).to be @expected_answer
end

Then("this scenario will pass") do
  expect(@addition).to be @expected_answer
end
{% endhighlight %}

Cross you fingers, and.. yeah, you know the drill

{% highlight terminal %}
cucumber
{% endhighlight %}

Hopefully, the output will be more or less like this:


{% include gallery id="screenshot_success" %}


Yey! Looks good to me! You’re now all set to write your first API test using Cucumber! Awesome work, you!

![](http://i.giphy.com/pHb82xtBPfqEg.gif?style=centerme)


## Using Airborne

What I’ve found great with Airborne, is how intuitive and easy it is. You can use all of the well known API methods, like POST, GET, DELETE, PUT, PATCH, UPDATE and OPTION.

There is also a great deal of built in matchers with their [API](https://github.com/brooklynDev/airborne#api).

So, let’s try some of this stuff out, and create our first API tests!

Create a new file: `features/response-codes.feature`

{% highlight gherkin %}
# features/response-codes.feature

Feature: HTTP status testing with Cucumber and Airborne

Scenario: Get request to httpstat.us/200 will respond with a 200
  When I perform a GET request to httpstat.us/200
  Then the response code will be 200

Scenario: Get request to httpstat.us/404 will respond with a 404
  When I perform a GET request to httpstat.us/404
  Then the response code will be 404

Scenario: Get request to httpstat.us/401 will respond with a 401
  When I perform a GET request to httpstat.us/401
  Then the response code will be 401

Scenario: Get request to httpstat.us/500 will respond with a 500
  When I perform a GET request to httpstat.us/500
  Then the response code will be 500

{% endhighlight %}

Now we can add some airborne style code, to get our tests passing

{% highlight ruby %}
# features/step_definitions/response-codes.rb

When("I perform a GET request to httpstat.us/{int}") do |test_status|
  get "https://httpstat.us/#{test_status}"
end

Then("the response code will be {int}") do |expected_status_code|
  expect_status(expected_status_code)
end
{% endhighlight %}


`expect_status` will fail if the status returned from the get request is not matching the `expected_status_code`

Let's run the code, to see if our tests are passing

{% include gallery id="screenshot_success_status" %}

Perfect!

But what happens if we would have a failing test? Let's give that a go as well, just to understand what's going on here

Add a scenario to the `features/response-codes.feature`


{% highlight gherkin %}
# features/response-codes.feature

Feature: HTTP status testing with Cucumber and Airborne

Scenario: Get request to httpstat.us/403 will respond with a 403
  When I perform a GET request to httpstat.us/403
  # force failing the following step
  Then the response code will be 404
{% endhighlight %}

What happens when we run this code?

{% include gallery id="screenshot_failing_status" %}

As you can see, the error message is clean and understandable.
You were expecting that the response code would be 403, but the actual result is 404.

## More Examples

Let's take this a bit further. In the following examples, I will use [JSONPlaceholder](https://jsonplaceholder.typicode.com/) for testing purposes.

JSONPlaceholder is a Online REST API, and replicates a database with:
{% highlight terminal %}
/posts	     100 items
/comments    500 items
/albums	     100 items
/photos	     5000 items
/todos	     200 items
/users	     10 items
{% endhighlight %}

..and you can use the normal API methods on all of the objects. Ie:

{% highlight terminal %}
GET	/posts
GET	/posts/1
GET	/posts/1/comments
GET	/comments?postId=1
GET	/posts?userId=1
POST	/posts
PUT	/posts/1
PATCH	/posts/1
DELETE	/posts/1
{% endhighlight %}

So let's write a scenario. Start with `features/get-posts.feature`

{% highlight gherkin %}
# features/get-posts.feature

Feature: Get Posts

Scenario: Get all posts
  Given I have 100 posts in my database
  When I perform a GET request for all posts
  Then I receive all 100 posts
{% endhighlight %}

And the ruby code for it:

{% highlight ruby %}
# features/step_definitions/get-posts.rb

Given("I have 100 posts in my database") do
  # Leaving this step empty for enhanced readability of scenario
end

When("I perform a GET request for all posts") do
  get "#{api_base_url}/posts"
end

Then("I receive all 100 posts") do
  expect(json_body.size).to be 100
end

def api_base_url
  'https://jsonplaceholder.typicode.com'
end
{% endhighlight %}

Run the feature, and hopefully you'll get the same result

<img src="/assets/posts/get-100-posts.png" width="645"/>

What about verifying the ID of the user who wrote Post 1? That'd be a cool scenario!

Feature file `features/verify-user-id.feature`

{% highlight gherkin %}
# features/verify-user-id.feature

Feature: Verify User ID of post

Scenario: Verify User ID of post
  Given the first post is written by user with ID 1
  When I perform a GET request for the first post
  Then I can see that the first post is written by the user with ID 1
{% endhighlight %}

And ruby code

{% highlight ruby %}
# features/step_definitions/verify-user-id.rb

Given("the first post is written by user with ID 1") do
  # Leaving this step empty for enhanced readability of scenario
end

When("I perform a GET request for the first post") do
 get "#{api_base_url}/posts/1"
end

Then("I can see that the first post is written by the user with ID {int}}") do |expected_user_id|
  expect_json(userID: expected_user_id)
end

def api_base_url
  'https://jsonplaceholder.typicode.com'
end
{% endhighlight %}

And voila!

{% include gallery id="screenshot_user_id" %}


### Now what?

Try it out! Write your own scenarios, and read more about the abilities of [Airborne](https://github.com/brooklynDev/airborne). It's a lot more than what I have covered in this brief intro. All I can say is that I've found this framework really helpful in the day to day work as a tester in a team focusing on an API.

And please reach out if there's any questions! And all feedback are welcome with much love, as this is my first ever blog post.
