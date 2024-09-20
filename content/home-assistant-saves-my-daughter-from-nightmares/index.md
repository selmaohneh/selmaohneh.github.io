---
title: "Home Assistant saves my Daughter from Nightmares"
date: 2024-09-08
image: "cover.jpg"
categories: 
  - "smart home"
keywords: 
  - "home assistant"
  - "automation"
  - 
tags: 
  - "home assistant"
  - "shelly"
  - "aqara"
---
## The Problem
When our daughter is (finally) asleep, we like to watch a few more series to wind down. Currently, we are into relatively violent stuff like [The Boys](https://amzn.to/3XyVo7U).
My wife likes to tell the anecdote that she used to crawl out of bed as a child, then secretly sit on the stairs and listen to her parents or the television. 
Once, she even had nightmares because her parents were watching a horror movie and she couldn't get the horrifying screams out of her head.

But back to the present:

> I don't want my child to see exploding heads or dismembered people, let alone dream about them. 

![Homelander in The Boys](/homelander.gif)

## Home Assistant to the rescue!
The first challenge was to figure out how to detect my daughter's escape attempts in time. The solution is super simple:

I installed a [cheap little door contact](https://amzn.to/3ZfLBF6) on the door to her nursery. *Installed* is an exaggeration, these things are just stuck on.

I then integrated it into Home Assistant. When integrating new sensors, it's best to remember to tidy up right away: give them a sensible name and disable any entities you don't need.

![The door contact integrated as device in Home Assistant](/doorsensor.jpg)

Now I can notice my little runaway as soon as she opens the door!

The opening of the door is the trigger of my automation. When this trigger occurs, I pause the playback on my [Fire TV Cube](https://amzn.to/47gcJ99) and let my [speakers](https://amzn.to/3Xeq657) make a little warning announcement. 

Of course, this only happens when the cinema is currently enabled and it's our daughter's sleeping time. Otherwise, we would hear the announcement all day long…

```yaml
alias: Notify at Fire TV Cube when Lia opens her door
description: ""
trigger:
  - platform: state
    entity_id:
      - binary_sensor.liasroom_door
    from: "off"
    to: "on"
condition:
  - condition: state
    entity_id: binary_sensor.lia_sleeping
    state: "on"
action:
  - condition: state
    entity_id: input_boolean.livingroom_cinema_enabled
    state: "on"
  - target:
      entity_id: tts.home_assistant_cloud
    data:
      cache: true
      message: Lia is awake!
      media_player_entity_id: media_player.wohnzimmer
    action: tts.speak
mode: single
```

## An authentic evening on the couch
* My wife and I are watching a gore-slasher-nightmare-horror movie
* Our little escapee sneaks out of her bed and opens the door. 
* The movie pauses immediately; there are no scary noises or destructive images. 
* The announcement confirms that our daughter seems to be awake. 
* One of us can quickly go upstairs and put our daughter back into bed before she even has a chance to look at the screen.

## Conclusion
That's why I love Home Assistant! Such a little automation adds so much value. Sweet dreams!