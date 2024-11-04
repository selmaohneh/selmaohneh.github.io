---
title: "I have nothing to hide - A dangerous mindset in the age of OSINT"
date: 2024-11-04
url: /i-have-nothing-to-hide
image: "cover.jpg"
categories:
  - "cyber security"
keywords: 
  - "phishing"
  - "social engineering"
tags: 
  - "phishing"
  - "social engineering"
---
I often hear the argument "I have nothing to hide" when talking about data privacy. This argument, of course, is a little naive. Some time ago I came across a story that shows how easy it is to find enough private information about a person in publicly available sources (also called OSINT - Open Source Intelligence) to perform a scary phishing attack.

No matter if the story actually happened or is just made up, I want to rethink the steps of that attack from the role of the attacker to show how trivial it actually is. After reading it, be honest with yourself—would you have fallen for it?

### Finding the Target
The first step is to find a company we want to attack. After some Googling, we find a promising company called *Generic Company*. We defined that company as interesting because of two facts:
1. The company has a high revenue every year. A high revenue also means a high potential bounty. This data is publicly available since the company is legally bound to publish a report periodically. There are multiple websites we can use to see the numbers, i.e., [annualreports.com](https://www.annualreports.com/).
2. The company has to have an impressum on its website. The impressum lists the email address of the secretary: flobdop.jenny@generic-company.com. We noticed that *Flobdop* is a pretty uncommon last name. This is promising for further investigations.

### Finding the Weak Point
Now we start researching *Flobdop* with some additional information, like the city of the company. We find an article from *Generic School* that says that Sarah Flobdop (14) won the school's art competition. We also find a death notice stating that Jenny, Mike, and Sarah are mourning the loss of their beloved Marge Flobdop. It's not rocket science to figure out that Sarah seems to be the daughter of our secretary Jenny from *Generic Company*.

### Setting the Scene
Now we open up Google Maps and take a look at *Generic School*. We look for the nearest hospital and find *Generic Hospital*. The website shows us that there is an emergency ambulance. There is also a "Meet the Team" section. We now even know the name of the emergency doctor at the hospital, *Doctor Generic*. We could even take a little walk, visit the hospital, and take a photo of the shift schedule to know who will work next week.

### Preparing to Phish
We go back to the website of the school and download some of the available documents. They all have a nice-looking school letterhead. We simply copy that for our own document we are about to write. We also note down the name of the school counselor.

### The Attack
Let's swap roles again! We are now Jenny Flobdop.

It's a Tuesday morning. Kids are in school, and we are back at *Generic Company* doing work. Then an email pops up. You immediately recognize the design of your daughter's school. It says that they are sorry to inform you that your daughter Sarah had a school accident and was sent to *Generic Hospital*. Attached you'll find the admission report from Doctor Generic, signed by *Generic School Counselor*.

Of course the attachment was opened. The secretary was under emotional stress. **Boom**. As soon as the .pdf is opened, the ransomware starts...

According to the speaker this click cost *Generic Company* [over 4 million bucks](https://www.youtube.com/watch?v=Zn-vOGp-e4c). 

### Conclusion
This attack is powered solely by publicly available information. There is no Matrix-style hacking involved; it's just a well-crafted phishing email that was created using the information we gathered through OSINT. So please reconsider your "I have nothing to hide" mentality.
