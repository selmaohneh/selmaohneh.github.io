---
title: "Beyond the Dogma: An unpopular opinion on Clean Code"
date: 2024-07-15
image: "cover.jpg"
categories: 
  - "software craftmanship"
keywords:
  - "clean code" 
  - "dry"
  - "yagni"
  - "kiss"
tags:
  - "clean code" 
  - "dry"
  - "yagni"
  - "kiss"
---
## An uplifting read
Part of being a good developer is reading books and pursuing lifelong learning. One of the books that greatly influenced me as a young professional was [“Clean Code” by Robert C. Martin](https://amzn.to/3XZ8j3J). 

The best part? It’s an uplifting read. Even with limited software knowledge, you can easily grasp Martin’s concepts. I remember sitting on the train, reading with a smile on my face, and nodding in agreement throughout.

> Martin is spot on! “Clean Code” is the real deal!

## The Hardliner
That’s when my “Clean Code” phase began. I treated all of Martin’s principles as established facts; a guidebook that all good developers must adhere to. I became rather dogmatic, sometimes blindly applying Martin’s suggestions.

`DRY` (Don’t Repeat Yourself) — one of the key tenets of Clean Code. I internalized Martin’s ideas and shared them with my colleagues like a prophet. If I noticed something duplicated three times, I’d get nervous and immediately address it through extraction.

> “Extract ’til you drop!” — Martin’s mantra.

## Coming of Age
Becoming a Senior Developer required me to question Martin’s principles. Don’t get me wrong, Martin is still correct and “Clean Code” remains essential. However, blindly adopting all his rules is a mistake. Ponder them; some rules even contradict each other. `DRY` can lead to complex architectures that breach the `KISS` (Keep It Simple, Stupid!) principle. It can also lead to disregarding `YAGNI` (You Ain’t Gonna Need It).

I’ve come to realize that 6 lines of duplicated code are often cleaner than, for instance, implementing an observer pattern. With those 6 lines, I can instantly understand the code, without having to navigate through multiple classes and interfaces. If those 6 lines turn into 7 lines in 5 years, so be it. It’s an acceptable growth.

If you find yourself with 12 lines of duplicated code growing every month, it’s a sign that refactoring should be scheduled.

## Some Advice
I believe every dedicated developer experiences a dogmatic Clean Code phase, so I won’t advise you to skip it. But keep it brief by critically evaluating Martin’s rules and adopting them only when they truly fit!

Another rule I always remember: 

> The developer who has to debug your code is a serial killer who knows where you live!

Will you offer them 6 lines of code adding items to a list, or will you guide him through the Visitor Pattern to gather those 6 items?