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

- How we are going to prove that our product does what we intend it to do.  
- How do I know that I haven't already broken something that was working?  
- How can I reliably make sure that I don't break something the next time I write some code?  

Large companies with hundreds of developers contributing to the same code base must have this problem, so there must be some solutions already out there.

## What is Behaviour Driven Development (BDD)

To start to solve the problem, I needed to determine what our product was intended to do.  That might be a funny thing seeing as we're almost a year into the development of a product, but the idea of the product over the last year has changed considerably.  We use an Agile Methodology which allows for a fluid change in requirements.

I found [this website](http://agilecoach.typepad.com/agile-coaching/2012/03/bdd-in-a-nutshell.html) that describes Behaviour Driven Development better than I would.  It says:

> BDD is an approach for building a shared understanding on what software to build by working through examples.

![](../images/bdd-overview.jpg)
