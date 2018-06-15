---
title: Suggesting Behaviour Driven Development (BDD) to my boss
date: 2018-06-10 08:52:16
tags: BDD
---

## TL;DR

I'm trying to integrate automated testing into the development or release pipeline, to prove that any new code, doesn't break already existing functionality.  I've managed to convince my boss its a good idea using BDD, but i'm struggling to work out how to do it.  I've got some outstanding questions:

- Do we test only our product in isolation or test external services too?  
- Where do we fit the automated testing into our release pipeline and branching strategy?
- If we test using an external OAuth2 server, how can we replicate a user logging in using Implicit Grant flow so we can test a web based API?

## The Problem

I started a project at work about a year ago, and we're getting to the point where we need to release for the first time.  Crunch Time.  As we started talking about release dates, it got me thinking:

- How we are going to prove that our product does what its supposed to.  
- How do I know that I haven't already broken something that was working?  
- How can I reliably make sure that I don't break something the next time I write some code?  

Testing is done manually.  This means that as the product grows, so will the time it takes to regression test.

This is perhaps, something that I should have thought about earlier, but better late than never.

## The Solution

### How we are going to prove that our product does what its supposed to

We need to determine and clearly present what our product can do.  We need to provide a link between the requirements of the product, and verifying the requirements are possible.  *Behaviour Driven Development* can help.

### How do I know that I haven't already broken something that was working

Automate Tests.  Easier said that done, but moving from manual testing to automated testing will give us the ability to regression test more often without consuming a tester's time.

### How can I reliably make sure that I don't break something the next time I write some code

Allow the Automated tests to be run by developers, as they are writing new code.  Getting developers closer to the testing process will highlight any potential breaking changes earlier and allow the developer to fix these issues without anyone knowing they happened.

## Behaviour Driven Development (BDD)

I found [this website](http://agilecoach.typepad.com/agile-coaching/2012/03/bdd-in-a-nutshell.html) that describes Behaviour Driven Development better than I would.  It says:

> BDD is an approach for building a shared understanding on what software to build by working through examples.

![](../images/bdd-overview.jpg)

> BDD is pretty simple, describe what you want the system to do by talking through example behaviour.Work from the outside-in to implement those behaviours using the examples to validate you're what you're building.

This approach will help clarify what our product is supposed to do, through examples.  I'm the sort of person who prefers talking in examples.  I feel it shows my understanding of the requirements.  The challenge will be validating the examples are possible within our product.

Validating examples can be done manually, or through automated testing.

## Implementing BDD and Automated Testing in our project

To implement this change in mentality within our project, everyone will need a clear understanding of what BDD is and what it hopes to achieve.  If they can see the value that it can add, then it will help them buy into the concept.  

### The Business Analyst

The Business Analyst (BA) is responsible for gathering requirements and feeding them into the Backlog in the form of Backlog Items.

The BA will need to be willing to contribute to any discussions that result in examples being produced, and be willing to verify that the examples meet the requirements and are not out of scope.  

### The Developer

The developer is responsible for implementing the requirements within the Backlog Item.

The developers will need to contribute to creating examples and possibly help implement automated tests.

### The Tester

The tester is responsible for proving the requirements within the Backlog Item have been met and marking it as complete.



### Who creates the examples and when

At the beginning of our current sprint, a tester will wait for a developer to check in work to the main branch and release to a test environment.  Towards the end of the sprint, the developer cannot start any new development as the tester will not be able to test it before the end of the sprint, and the Backlog Item will be incomplete.  This means that there is testers waiting for work at the beginning and developers not sure what to do at the end.  

Introducing the need to write examples and verify them could help keep a constant workload for developers and testers.  

My suggestion is that testers write up the examples at the beginning of the sprint, when there is not much testing to do.

## The Challenges

- Who writes the examples?
- How do we verify the examples?

## Lets chat with the boss

## Summary

The Business Analyst will start working closer with testers to verify examples meet the requirements and are not out of scope.