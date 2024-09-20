---
title: "How to implement Retries without Cluttering your Code"
date: 2024-07-24
image: "cover.jpg"
categories: 
  - "software craftmanship"
keywords: 
  - "retry"
  - "polly"
  - "api"
  - "dry"
  - "clean code"
  - "pattern"
tags: 
  - "retry"
  - "polly"
  - "api"
  - "dry"
  - "clean code"
  - "pattern"
---
## Imperfection
One important lesson I learned working as a software engineer in the production industry for many years:

> Some things don’t work on the first try.

The real-world hardware and corresponding drivers I met and worked with always had some quirks.

- Some hardware disconnects from time to time.
- Some drivers throw an exception when commands are executed too fast after each other.
- Some sensors don’t answer your commands at all. Maybe after the third try.

>The world is not perfect and everyone makes mistakes. Most of the time, hardware and drivers don’t work as you would expect.

## A naive Solution

A common solution to avoid these irregular effects, if you cannot fix the root cause, is introducing a retry logic for your action. This often looks similar to the following:

```csharp
const int maxAmountOfTries = 3;

int currentAmountOfTries = 0;

while (true)
{
    if (currentAmountOfTries > maxAmountOfTries)
    {
        break;
    }

    try
    {
        currentAmountOfTries++;
        DoSomethingThatCanThrowAnException();
        
        break;
    }
    catch (Exception)
    {
        // ignore or log exception
    }
}
```

This is a great piece of code! It is easy to read, and everyone glimpsing at it knows that this is some retry logic immediately. They could even tweak the number of allowed retries without any deep dive into the code — it’s maintainable, too!

But as the code base grows, you will find that construct all over the solution. Even worse: always a little different.

- Most of the time, the variable names differ
- Sometimes the maximum number of tries is set, and sometimes, the maximum number of retries is set.
- Some classes retry on exceptions, some on specific results, some on both.

These repetitions lead to confusion, which results in bugs.

>Don’t repeat yourself!

## The Extraction
A smart team, of course, notices this flaw and tries to eliminate it. Let’s extract that retry logic so it can be reused! I saw several solutions for that over the years:

- A dedicated service for retrying like `IRetryService.Retry(...)`
- An extension method on `Action` or `Func`

These solutions worked most of the time. But they required a significant amount of work! Especially if you want to support retries for both sync and async actions. Still, nearly every bigger project contains some functionality like this.

## Party Parrot to the rescue!
There is already an elegant solution to this problem out there! You can use the library [Polly](https://github.com/App-vNext/Polly) to de-clutter your code. The icon of this library is a colorful parrot. That’s why I can’t resist making bad [Party Parrot](https://cultofthepartyparrot.com/) jokes about it — Party or Die!

>Don’t reinvent the wheel!

Here is what the upper code looks like when Polly comes into place:

```csharp
RetryPolicy policy = Policy
                  .Handle<Exception>()
                  .Retry(3);

policy.Execute(DoSomethingThatCanThrowAnException);
```

Far less code. In this example, just five lines of code. And it is still clear and precise. That’s a great benefit we have already achieved!

It gets better. You remember how I said that hardware behaves strangely from time to time? I once met a sensor that returned `double.NaN` sometimes. If it did, you just had to let it rest for some seconds and schedule a retry.

Polly can do that, as well. Here is how I would additionally handle the invalid result and introduce a waiting time between every retry:

```csharp
RetryPolicy<double> policy = Policy<double>
                              .Handle<Exception>()
                              .OrResult(Double.NaN)
                              .WaitAndRetry(3, (_, _) => TimeSpan.FromSeconds(5));

policy.Execute(DoSomethingThatCanThrowAnException);
```

## The Advantages
Let’s focus on the advantages of using Polly for retries:

- Great API: Short, precise, and easy to read.
- Polly is widely used and tested. Don’t reinvent a buggy solution.
- You can reuse common policies by sharing them between classes.

I want to emphasize the API once more. Consider the following method: `public double DoSomethingCool(CancellationToken token).` I love that it takes a `CancellationToken`.

Why? Because any user of this method, every client, knows that it supports cancellation! And even better: Every user of this method can decide how they want to handle cancellation. They can cancel with a timeout, cancel when the user clicks some abort button, or cancel however they want.

>A great API forces the user to use it correctly.

We can use the same idea for our retry logic with Polly. Look at the following updated method prototype: `public double DoSomethingCool(ISyncPolicy policy, CancellationToken token)` .

I love it! This method seems to support cancellation and execution policies — I know that just by looking at the prototype.

I can call it in any way I choose! Maybe with five retries on exceptions? Maybe with three retries on `double.NaN` with a timeout of two seconds max? It’s up to your clients — your method supports it no matter what they decide. Thanks to Polly and the great API design!

Definitely check out the [readme](https://github.com/App-vNext/Polly) of Polly because it can do so much more:

- rate-limiting
- timeouts
- caching

Thanks for reading!