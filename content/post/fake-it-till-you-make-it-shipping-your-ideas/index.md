---
title: "Fake it till you Make it: Shipping your Ideas"
date: 2023-01-04
image: "cover.jpg"
url: /fake-it-till-you-make-it
categories: 
  - "software craftmanship"
keywords: 
  - "database"
  - "clean code"
  - "gtd"
  - "aws"
  - "azure"
tags: 
  - "database"
  - "clean code"
  - "gtd"
  - "aws"
  - "azure"
---
## Procrastination
I recently decided to start a new side-project. It’s an app for android. I often have ideas for great apps — but I only ever shipped a few. Why is that?

Right after an idea emerges, I immediately start to think about architecture and technical details.

* I will definitely need a database…
* No self-hosting. I definitely need to host that in the cloud. Maybe on AWS or Azure?
* Maybe some server-less approach?
* What kind of databases does AWS provide?
* …

I then waste at least two weeks drilling down the AWS documentation and researching several irrelevant topics like `global secondary index` or `DynamoDB sort keys`. Sounds familiar? It usually ends with:

> That will be a huge application. I won’t be able to develop that on my own with that little spare time.

And just like that a great app idea dies. Right before it even started.

## Getting sh*t done!
I learned from these mistakes. The first step to fix them is easy:

> Just start coding!

Open your IDE and get going. Connect your mobile to the computer and start up the template solution. As soon as you see the first little success, you will keep going on.

## Fake it!
But what about the database? There is a simple solution for that too. And it works so well for me, I felt compelled to share it:

> Fake it till you make it!

You don’t need a full fledged cloud-database to get started. Just get your interfaces right.

A simple `IDatabase` might be enough. Add methods to it as you go and need them.

* `IDatabase.GetUser(Guid userId)`
* `IDatabase.GetPostsBy(Guid userId)`
* …

I simply implement a `LocalDatabase` class that implements the interface returning some dummy data. As I need more test data I might implement a `FileDatabase` class which returns data from a file.

That way I was able to complete the whole app before even thinking about clouds, AWS, Azure, server-less or partition keys.

> By moving hard decisions like choosing a database to the end of the project, I got rid of all fear and actually finished the app!

And even better: At that point, I had all the information I needed to choose the correct database. I knew my access patterns and which queries I need. All that without spending a single penny to AWS while developing.

If that big technical decision is the last step to finish your product, you will definitely do it. You won’t drop that much work you already invested. You won’t drop your idea when you can already see it and use it!

## Summary
Faking technical decisions in code is a psychological trick to actually finish and ship your ideas. It helps me get started. It helps me get things done. Maybe it helps you, as well.