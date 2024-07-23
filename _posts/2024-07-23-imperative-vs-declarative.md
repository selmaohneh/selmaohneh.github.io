---
title: "Imperative vs. Declarative Models. Explained with beer"
header:
  teaser: /assets/images/beer.jpg
---

Do you know the difference between an imperative modelling approach and a declarative one? If not, here is a brief explanation with beer.

When hosting a party in our garage, I need to make sure the fridge is filled with beer. Of course we also organize soft drinks and water, since legend has it that there are people who don't like beer.

![beer](/assets/images/beer.jpg)

### The Imperative Model

Let's say I have space for around 30 bottles in my fridge. I put 15 bottles of beer, 10 bottles of soft drinks and 5 bottles of water in it.

>Perfect! Work done, the party can start!

But 10 minutes into the party, the work continues. The fridge is getting empty and I have to replenish the supply. My desired state of 15/10/5 bottles has already dropped to roughly 6/5/4. 

I need to move more bottles from the beer, soft drink, and water cases to the fridge to keep them cold.

Phew… that is quite tedious. The work never stops since it's my job to maintain the desired state of 15/10/5 bottles all night long.

That's why I instructed everyone to refill the fridge from time to time when they see a shortage, by sticking a note on the fridge.

But chaos soon ensues. We now have very strange fridge-bottle-ratios:

* Either there are too many bottles in the fridge, causing them to roll out and spill all over the floor,
* Or the fridge is full but doesn't contain a single bottle of beer.
* …

This pretty much sums up how an imperative model works:

* There is the job of moving bottles from the case to the fridge
* This has to be done once before the party
* The bottle count has to be checked regularly
* The job of restocking the fridge needs to be done repeatedly

>The imperative model focuses on the actions that need to be done in order to set the system up and keep it running.

### The Declarative Model
I learned from my mistakes! I don't want to go to bed at 9pm because I'm exhausted from all the work. So, we hired a bartender.

We told the bartender our desired state of 15/10/5 bottles, and he handles the rest for us.

- He sets up the fridge initially,
- He checks the ratio regularly,
- He refills the fridge as needed.
* …

In essence, he performs all the jobs I had to do. I can enjoy the party without worry.

So from my perspective, the following happened:

Instead of focusing on the actions that need to be done, I simply defined the desired end-state and delegated the responsibility of achieving that state to the bartender. I don't even care **how** he does his job, as long as he guarantees that the fridge is always filled as I requested.

>The declarative model focuses on the desired state while delegating the responsibility of the necessary actions to achieve that state.

### Why beer?

There might be better examples like:

- Setting the room temperature on a thermostat vs. switching a heater on and off all the time depending on the current temperature
- Asking a cook to provide a buffet for 20 people vs. giving the cook atomic instructions on what to buy, how to cut the ingredients, how to season, …
- Kubernetes: Scaling pods on load changes
- …
 
But I like beer. So I chose this example. Deal with it.