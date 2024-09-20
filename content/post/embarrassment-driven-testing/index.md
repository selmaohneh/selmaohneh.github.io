---
title: "Embarrassment-Driven-Testing"
date: 2024-07-29
image: "cover.jpg"
categories: 
  - "software craftmanship"
keywords: 
  - "testing"
  - "clean code"
tags: 
  - "testing"
  - "clean code"
---
## A new perspective
There are lots of links in my head when thinking about testing in software:

* a good test suite indicates good software quality
* a good test suite is a safety net for developers
* a good test suite saves time in the long run
* …

A talk from [Daniel Raniz Raneland](https://www.linkedin.com/in/raneland/) just added another surprising perspective:

> A good test suite prevents you from embarrassment.

## The `one thing`
I think this provides us with valuable insights on the minimun requirements of what to test: 

>It's the `one thing` your team/software is responsible for. The `one thing` that would be totally embarrassing if it didn't function correctly.

* if you develop the software for a snack vending machine, make sure that the doors open correctly.
* if you develop a to-do web app, make sure that to-dos can be ticked
* if you develop a mobile camera app, make sure you can actually take a picture
* …

If the Excel export of your to-do list generates an incorrect file name, users might tolerate it for a while. But if they can't tick their to-dos, they will definitely unsubscribe from your service.

Ticking to-dos is your `one thing`. Make sure to have that covered completely by the test suite. If starting from scratch, this should be one of the first E2E-tests.

## Return of Investments of tests

I like this idea of embarrassment-driven-testing because it relates so well to another new link I noted from a talk by [Johannes Stern](https://www.linkedin.com/in/yohstern/). 

> Every single test needs a return of investment (`ROI`).

You would not invest your valuable money in a project that does not earn you money in the long run, would you? The same is correct for software tests.

We write unit tests because they are cheap and give us a fast response to our changes. This is an instant `ROI`!

Integration-, API-, and E2E-tests are more expensive. Depending on the quality of the existing code they can be very expensive. But no matter the price, testing your `one thing` is a necessary investment, not just to prevent embarrassment, but to save your business.

This same thought can also be true from the opposite perspective: Tests can be a bad investment. If your feature is covered by a great E2E-test, do you really need an API-test *and* integration-test *and* unit-tests? Or are they just redundant?

Don't get me wrong, I am an advocate for testing and support every test that is written. But testing is about quality and not quantity. Having the `ROI` in mind, maybe it's time to delete those flaky tests that you spend so much time on debugging every week, while not giving you any confidence when they are finally green.