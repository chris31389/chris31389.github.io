---
title: 'BDD: Running tests using cucumber.js'
date: 2018-06-16 09:38:22
tags:
---

## TL;DR

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

### Step Definitions

Step definitions are snippets of code that get executed per line of the scenario.  Here is an example.

``` javascript
When("she eats {number} cucumbers", function(number) {
  alice.feed(3, "cucumbers");
});
```

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

Then run 

``` npm
npm test
```

You should get

``` powershell
> cucumber-example@1.0.0 test C:\dev\cucumber-example
> cucumber-js

............

4 scenarios (4 passed)
12 steps (12 passed)
0m00.004s
```

## Test an API

## Cucumber.js and Angular's Protractor
