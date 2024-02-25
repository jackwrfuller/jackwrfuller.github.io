+++
title = "Week Notes - 7th Week of 2024"
weight = 1
date = 2024-02-16
+++

# Notes - Week 7, 2024

- This week was the first full week I spent developing our GovCMS command line application which we so far call (perhaps uninspiringly) [govcms-cli](https://github.com/govcms-tests/govcms-cli). Like pretty much all our projects, they are funded for the benefit of the Australian public and therefore open-source. Feel free to check it out and follow along!
    - This project is the first time I have worked with Go, and I have to admit, the language is growing on me. I haven't yet had experience where I can't write something in the way I think it should be written. Other languages I have been practising (looking at you C++) have not been so forgiving.
    - We are building the CLI app using the well-known [Cobra](https://github.com/spf13/cobra) framework written by the legendary Steve Francia, whom I learned about only recently. I have to admit I am in awe that someone can bounce between so many well-known and important projects as he has, all the while creating many useful tools that I now use.
- By thursday, I had completed the main functionality of one the commands. While I am a big proponent of TDD, I regrettably did not practise it while developing this command - I think because I was overwhelmed with learning Go and Cobra at the same time. I then spent thursday and friday writing some tests to get the coverage and confidence up, however that was a challenge unto itself. There are not a huge amount of resources out there about testing Cobra CLI applications, especially with respect to dependency injection. This was challenging becuase I realised I needed to mock both the filesystem and database and needed to figure out how to do so. Given the lack of resources out there on this subject, I think I might release a blog post soon covering this topic.
- Wednesday was Valentine's, and I booked a nice semi-formal italian-fusion restaurant nearby for E. and I to go to. The food was amazing, and actually very reasonably priced. I got lamb shanks with some amazing sides, all for \\$33. This was very reasonably priced, when you consider that a very mid-tier chicken schnitzel from a local RSL will easily set you back $30 these days.
- It was confirmed that I will be attending [Drupal South 2024](https://drupalsouth.org/events/drupalsouth-sydney-2024) in Sydney this year! As the maintainers of the GovCMS project, it is healthy to connect with the wider Drupal community. This will be my first conference, let alone developer conference, so I am excited and curious to see how things play out. One of our senior engineers, Joseph, will be giving a [talk](https://drupalsouth.org/events/drupalsouth-sydney-2024/schedule/3038) there about the work we have been doing to improve testing on GovCMS. 

Reading:

- Normal Women by Phillipa Gregory

Watching:

- Welcome to Wrexham (season 2, Disney+)

Listening to:

- A Little Hatred by Joe Abercrombie, read by Steven Pacey

