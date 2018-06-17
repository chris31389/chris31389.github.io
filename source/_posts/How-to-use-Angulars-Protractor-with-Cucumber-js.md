---
title: How to use Angular's Protractor with Cucumber.js
date: 2018-06-17 07:23:26
tags:
- BDD
- Cucumber.js
- Angular
- Protractor
- Testing
---

## TL;DR

Here is the repository on GitHub [chris31389/angular-protractor-cucumber-example](https://github.com/chris31389/angular-protractor-cucumber-example.git)

You are required to have @angular/cli installed and can install it using

``` npm
npm install -g @angular/cli
```

You can clone the repository and execute the following commands:

``` npm
npm install
npm test
```

## Introduction

In [my previous post](/BDD-Running-tests-using-cucumber-js) I introduce Cucumber and Cucumber.js.  Here I will integrate Cucumber.js with Angular's Protractor so we can run some automated UI tests.

## Getting Started

To get started, you need to make sure you have installed Node.js.  If you need to you can download the LTS version [here](https://nodejs.org/en/).

We will need an angular project which we can generate using [Angular's CLI](https://cli.angular.io/). To install it run:

``` npm
npm install -g @angular/cli
```

With [Angular's CLI](https://cli.angular.io/) installed, we can generate an angular website with protractor already set up.  At a convenient location, run:

``` npm
ng new angular-protractor-cucumber-example
```

We can run the existing Protractor tests using:

``` npm
npm run e2e
```

## Adding Cucumber.js

Lets get started

``` npm
npm install --save cucumber @types/cucumber protractor-cucumber-framework chai @types/chai
```

Remove the `e2e/src` folder and its contents.

``` powershell
rm e2e/src -Recurse
```

Add the following files:

``` gherkin
# e2e/app/app.feature
Feature: Visit Website
    As a user
    I need to see the websites content
    So that I decide what to do next

Scenario: Visit page
    When I got to the page
    Then I see the title "Welcome to app!" 
```

``` typescript
// e2e/app/app.steps.ts
import { When, Then, Before } from "cucumber";
import { expect } from "chai";
import { AppPage } from "./app.po";

let appPage: AppPage;

Before(() => (appPage = new AppPage()));

When("I got to the page", () => appPage.navigateTo());

Then("I see the title {string}", (expectedText: string) => {
  appPage
    .getParagraphText()
    .then(actualText => expect(actualText).to.equal(expectedText));
});
```

``` typescript
// e2e/app/app.po/ts
import { browser, by, element } from 'protractor';

export class AppPage {
  navigateTo() {
    return browser.get('/');
  }

  getParagraphText() {
    return element(by.css('app-root h1')).getText();
  }
}
```

Now we need to update the protractor.conf.js

``` javascript
// e2e/protractor.conf.js
const {
  SpecReporter
} = require('jasmine-spec-reporter');

exports.config = {
  allScriptsTimeout: 11000,
  specs: [
    './**/*.feature'
  ],
  capabilities: {
    'browserName': 'chrome'
  },
  directConnect: true,
  baseUrl: 'http://localhost:4200/',
  // Use a custom framework adapter and set its relative path
  framework: 'custom',
  frameworkPath: require.resolve('protractor-cucumber-framework'),

  // cucumber command line options
  cucumberOpts: {
    // require step definition files before executing features
    require: ['./**/*.steps.ts'],
    // <string[]> (expression) only execute the features or scenarios with tags matching the expression
    tags: [],
    // <boolean> fail if there are any undefined or pending steps
    strict: true,
    // <string[]> (type[:path]) specify the output format, optionally supply PATH to redirect formatter output (repeatable)
    format: [],
    // <boolean> invoke formatters without executing steps
    dryRun: false,
    // <string[]> ("extension:module") require files with the given EXTENSION after requiring MODULE (repeatable)
    compiler: []
  },
  onPrepare() {
    require('ts-node').register({
      project: require('path').join(__dirname, './tsconfig.e2e.json')
    });
  }
};
```

Now if we run our tests

``` npm
npm run e2e
```

We get something similar to:

``` cmd
1 scenario (1 passed)
2 steps (2 passed)
0m01.140s
[08:17:46] I/launcher - 0 instance(s) of WebDriver still running
[08:17:46] I/launcher - chrome #01 passed
```

## Summary

Writing protractor test using Cucumber helps make the test human readable.  As we separate the steps from the test itself, it encourages DRY (Don't Repeat Yourself).

Here is the [completed example on GitHub](https://github.com/chris31389/angular-protractor-cucumber-example.git)