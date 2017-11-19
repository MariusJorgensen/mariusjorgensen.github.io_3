---
title: "Test your API with Cucumber and Ruby!"
tags: [code, cucumber, bdd, airborne, rspec, ruby, api, development, testing, test automation, automation]
layout: single
author_profile: true
---

Have you ever had the epiphany of how great it would be if everyone in the business could help to get your API more tested? Wouldn’t that be just fa-ntastic? I know, right? Not just unit tested, but real life tested.

Well, I can tell you now, that by the time you’ve finished this intro, you would have the tools to do that! Great, huh?

### The Tools

* [Cucumber-ruby](https://github.com/cucumber/cucumber-ruby) - BDD framework for writing tests in the business domain
* [Airborne](https://github.com/brooklynDev/airborne) - RSpec driven API testing framework

### Prerequisites

* [Ruby](https://www.ruby-lang.org/en/)
* [Bundler](http://bundler.io/)


### Installing everything you need

Create a new directory. I’m calling mine `api-cukes`

{% highlight terminal %}
mkdir api-cukes
cd api-cukes
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

<img src="/assets/snippet.png" width="645"/>

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

<img src="/assets/pending.png" width="645"/>

Let’s write some code to see this test come alive

{% highlight ruby %}
# features/step_definitions/hookup.rb

Given("I have set up this project correctly") do
  # I am letting this just be an empty step
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

<img src="/assets/success.png" width="645"/>

Yey! Looks good to me! You’re now all set to write your first API test using Cucumber! Awesome work, you!

![](http://i.giphy.com/pHb82xtBPfqEg.gif?style=centerme)


## Using Airborne

What I’ve found great with Airborne, is how intuitive and easy it is. You can use all of the API methods, like POST, GET, DELETE, PUT, PATCH, UPDATE, OPTION.

There is also a great deal of built in matchers with their [API](https://github.com/brooklynDev/airborne#api).

So let’s try some of this stuff out, and create our first API tests!

Create a new file: `features/response-codes.feature`

{% highlight gherkin %}
# features/response-codes.feature

Feature: HTTP status testing with Cucumber and Airborne

Scenario: Get request to httpstat.us/200 will respond with a 200
  When I do a GET request to httpstat.us/200
  Then the response code will be 200

Scenario: Get request to httpstat.us/404 will respond with a 404
  When I do a GET request to httpstat.us/404
  Then the response code will be 404

Scenario: Get request to httpstat.us/401 will respond with a 401
  When I do a GET request to httpstat.us/401
  Then the response code will be 401

Scenario: Get request to httpstat.us/500 will respond with a 500
  When I do a GET request to httpstat.us/500
  Then the response code will be 500

{% endhighlight %}

Run cucumber again to generate the code snippet, and add it to a new file `features/step_definitions/response-codes.rb`


{% highlight ruby %}
# features/step_definitions/response-codes.rb

When("I do a GET request to httpstat.us/{int}") do |int|
  pending # Write code here that turns the phrase above into concrete actions
end

Then("the response code will be {int}") do |int|
  pending # Write code here that turns the phrase above into concrete actions
end
{% endhighlight %}

Now we can add some airborne style code, to get our tests passing

{% highlight ruby %}
# features/step_definitions/response-codes.rb

When("I do a GET request to httpstat.us/{int}") do |test_status|
  get “https://httpstat.us/#{test_status}”
end

Then("the response code will be {int}") do |expected_status_code|
  expect_status(expected_status_code)
End
{% endhighlight %}


`expect_status` will fail if the status returned from the get request is not matching the `expected_status_code`

Let’s run the code, to see if our tests are passing

<img src="/assets/success_status.png" width="645"/>
