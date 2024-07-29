---
title: "Embarrassment-Driven-Testing"
header:
  teaser: /assets/images/fingerpointing.jpg
---

There are lots of links in my head when thinking about testing in software:

* a good test suite indicates good software quality
* a good test suite is a safety net for developers
* a good test suite saves time in the long run
* …

A talk from [Daniel Raniz Raneland]([Daniel Raniz Raneland | LinkedIn](https://www.linkedin.com/in/raneland/)) just added another surprising perspective:

> A good test suite prevents you from embarrassment.

![embarrassment](/assets/images/fingerpointing.jpg)

I think this gives us good guidance for what our test suite needs to cover at least: it's the *one thing* your team/software is responsible for that would be totally embarrassing if not working.

* if you develop the software for a snack vending machine, make sure that the doors open correctly.
* if you develop a to-do web app, make sure that to-dos can be ticked
* if you develop a mobile camera app, make sure you can actually take a picture
* …

If the Excel export of your to-do list generates an incorrect file name, users might tolerate it for a while. But if they can't tick their to-dos, they will definitely unsubscribe from your service.

Ticking to-dos is your *one thing*. Make sure to have that covered completely by the test suite. If starting from scratch, this should be one of the first e2e-tests.

I like this idea of embarrassment-driven-testing because it relates so well to another new link I noted from a talk by [Johannes Stern]([Johannes Stern | LinkedIn](https://www.linkedin.com/in/yohstern/)). 

> Every single test needs a return of investment (ROI).

You would not invest your valuable money in a project that does not earn you money in the long run, would you? The same is correct for software tests.

We write unit tests because they are cheap and give us a fast response to our changes. This is an instant ROI!

Integration-, API-, and E2E-tests are more expensive. Depending on the quality of the existing code they can be very expensive. But no matter the price, testing your *one thing* is a necessary investment, not just to prevent embarrassment, but to save your business.

This same thought can also be true from the opposite perspective: Tests can be a bad investment. If your feature is covered by a great E2E-test, do you really need an API-test *and* integration-test *and* unit-tests? Or are they just redundant?

Don't get me wrong, I am an advocate for testing and support every test that is written. But testing is about quality and not quantity. Having the ROI in mind, maybe it's time to delete those flaky tests that you spend so much time on debugging every week, while not giving you any confidence when they are finally green.