---
title: 'BDD: Running tests using cucumber.js'
date: 2018-06-16 09:38:22
tags:
---

## TL;DR

Here is the repository on Github [chris31389/cucumber-example](https://github.com/chris31389/cucumber-example.git)

You can clone the repository and execute the following commands:

``` npm
npm install
npm test
```

## What is Cucumber

[Cucumber](https://docs.cucumber.io/) is a way of presenting and validating a scenario of how a system should work.  It is living documentation that changes as your system changes.  Cucumber consists of Gherkins and Step Definitions.

### Gherkins

A gherkin is a human readable scenario of how the system should work.  Here is an example.

``` gherkin
# Comment
@tag
Feature: Eating too many cucumbers may not be good for you

  Eating too much of anything may not be good for you.

  Scenario: Eating a few is no problem
    Given Alice is hungry
    When she eats 3 cucumbers
    Then she will be full
```

#### Who should write them

From the [cucumber guide](https://docs.cucumber.io/guides/overview/):

> It’s usually best to let developers write Gherkin if the team is practicing BDD (test first).  
>
> If Cucumber is used solely as a test automation tool (test after) it can be done by testers or developers.
>
> It is usually counterproductive to let product owners and business analysts write Gherkin. Instead, we recommend they participate in Example Mapping sessions and approve the Gherkin documents after a developer or tester has translated it to Gherkin.

I believe that the tester is the best person to write the Gherkin as they will be able to use their skill in testing edge cases to explore more scenarios than just the happy path.

### Step Definitions

Step definitions are snippets of code that get executed per line of the scenario.  Here is an example.

``` javascript
When("she eats {number} cucumbers", function(number) {
  alice.feed(3, "cucumbers");
});
```

#### Who should write step definitions

From the [cucumber guide](https://docs.cucumber.io/guides/overview/):

> The same people who write Gherkin should be writing step definitions. This is another reason the Gherkin is best written by developers and/or testers.

Whilst I agree with this, I think that there may be times when someone different writes the step code to the person who wrote the Gherkin.

## Lets see an example

Here is a link to a [live demo](http://cucumber.github.io/cucumber-js/) from cucumber.js

To get started, you need to make sure you have installed Node.js.  If you need to you can download the LTS version [here](https://nodejs.org/en/).

Open a command prompt and navigate to a new folder. (e.g. `C:\cucumber-example`)

``` npm
npm init
```

Accept all defaults, until it asks for a test command.  Enter `./node_modules/.bin/cucumber-js`.  Accept all other defaults.

It should look like this:  

``` powershell
package name: (cucumber-example)
version: (1.0.0)
description:
entry point: (index.js)
test command: ./node_modules/.bin/cucumber-js
git repository:
keywords:
author:
license: (ISC)
About to write to C:\dev\cucumber-example\package.json:

{
  "name": "cucumber-example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "cucumber-js"
  },
  "author": "",
  "license": "ISC"
}


Is this OK? (yes)
```

Now we need to install cucumber.js and the assertion library.

``` npm
npm install --save chai cucumber
```

Add the following files

``` gherkin
# features/simple_math.feature
Feature: Simple maths
  In order to do maths
  As a developer
  I want to increment variables

  Scenario: easy maths
    Given a variable set to 1
    When I increment the variable by 1
    Then the variable should contain 2

  Scenario Outline: much more complex stuff
    Given a variable set to <var>
    When I increment the variable by <increment>
    Then the variable should contain <result>

    Examples:
      | var | increment | result |
      | 100 |         5 |    105 |
      |  99 |      1234 |   1333 |
      |  12 |         5 |     17 |
```

``` javascript
// features/support/world.js
const { setWorldConstructor } = require('cucumber')

class CustomWorld {
  constructor() {
    this.variable = 0
  }

  setTo(number) {
    this.variable = number
  }

  incrementBy(number) {
    this.variable += number
  }
}

setWorldConstructor(CustomWorld)
```

``` javascript
// features/support/steps.js
const { Given, When, Then } = require('cucumber')
const { expect } = require('chai')

Given('a variable set to {int}', function(number) {
  this.setTo(number)
})

When('I increment the variable by {int}', function(number) {
  this.incrementBy(number)
})

Then('the variable should contain {int}', function(number) {
  expect(this.variable).to.eql(number)
})
```

Then run our tests

``` npm
npm test
```

You should get something like:

``` powershell
> cucumber-example@1.0.0 test C:\dev\cucumber-example
> cucumber-js

............

4 scenarios (4 passed)
12 steps (12 passed)
0m00.004s
```

## Test an API

Lets up the game.  Lets add a feature to do a HTTP call and check its working as expected.

We need to start with the Gherkin, so lets add this feature file:

``` gherkin
# features/retrieve_posts.feature
Feature: Getting posts
    This feature is about retrieving post items from an api

    Scenario: Get single with id 1
        Given a post id of 1
        When I get a single post
        Then it is successful
```

As we need to do a HTTP request, we can add an existing library to make HTTP requests easier.

``` npm
npm install --save request
```

Lets write the Step Definitions:

``` javascript
// features/support/post-api.steps.js
const { Given, When, Then } = require('cucumber')
const assert = require('assert');
const request = require('request');

Given('a post id of {int}', function (input) {
    id = input;
});

When('I get a single post', function (callback) {
    request.get(`https://jsonplaceholder.typicode.com/posts/${id}`, (err, response, body) => {
        if (err) {
            callback(err);
        }
        else {
            apiResponse = response;
            callback();
        }
    })
})

Then('it is successful', function () {
    isSuccess = apiResponse.statusCode >= 200 && apiResponse.statusCode < 300;
    assert.equal(isSuccess, true);
});
```

Now we can run our tests again

``` npm
npm test
```

You should get something like:

``` powershell
> cucumber-example@1.0.0 test C:\dev\cucumber-example
> cucumber-js

............

5 scenarios (5 passed)
15 steps (15 passed)
0m00.223s
```

## Summary

We can use cucumber.js to verify that our system works as expected.  What we have here is just a demonstration of how to start a project.

One consideration is the world object and how to manage the sequential execution of step definitions.  The step definitions need to make sure that they can be run with any step preceding it and could be written in a defencive manor e.g. checking variables exist.
