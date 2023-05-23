+++
title = "Blue Lagoon: A Ramshackle JavaFX project"
description = "Or, learning through failure"
weight = 1
date = 2023-05-14

[extra]
local_image = "/projects/blue-lagoon/bluelagoon.jpg"
+++

<p align="center" width="100%">
    <img width="100%" src="bluelagoon.jpg">
</p>

## Resources

- Project Github (coming soon)
- <a href="BlueLagoon.jar" target="_blank">Jar File</a>
- <a href="BlueLagoon.exe" target="_blank">Windows Executable</a>

## Into the deepend

As part of my journey to become an expert engineer, I am enrolled in a Masters of Computing at the ANU which I attend remotely. The first course I took had a major group assignment that tasked us to build in Java - using the JavaFX engine - the (perhaps) little-known board game [*Blue Lagoon*](https://blueorangegames.eu/en/games/blue-lagoon/) by Reiner Knizia. The main purpose of this project was to cut our teeth on building larger scale projects involving multiple contributors while honing our object-oriented skills. Ironically, and not surprising at all to anyone who has done a university group assignment, my group members all went completely MIA very quickly. In short, I was left to make this game all by myself. While I have done a good deal of programming throughout my undergraduate years and on a hobbyist level, this was my first time building and persevering with a major project like this. And boy, did I learn some lessons.

As this was by no means my first time writing code, I was already familiar with [Hofstadter's law](https://en.wikipedia.org/wiki/Hofstadter%27s_law):

> Hofstadter's Law: It always takes longer than you expect, even when you take into account Hofstadter's Law. 

However, what I didn't realise at the time and perhaps what I should've known, was that this law applies in its own way to software complexity. Oh how easy it is to visual your system in your head! It should be simple right? What I quickly realised was that once I got around to implementing some feature or another, three dependencies sprung up like hydras. I was naively falling  afoul of [Murphy's Fourth Corollary](https://zira.home.xs4all.nl/murphy.html):

> Whenever you set out to do something, something else must be done first.

Furthermore, given my heretofore limited experience in software architecture, the business logic twisted, turned, and backflipped as my OO ability improved. Code refactoring was a common occurance, as my understanding of how to architect start to gain momentum. This was made tougher by the fact I was attempting to work on a 3 person job alone, without external validation to keep me in check. I often felt like I was hacking my way through CGP Grey's Forest of All Knowledge, desperately lost in tangent after tangent. 

Well, after all that wandering I have something to show for it: a game! Though unfortunately, not really. Due to time constraints (I work full-time) I was unable to fully flesh out the project and deliver a working game, at least for the moment. I made enough progress that I am proud of what I accomplished, and want to move on. Thus I felt comfortable sharing what I have made, regardless of how undercooked it is. The skills I have developed along the way will propel me to bigger and better projects. 

In this article I want to share some of the learnings I took away from this project, and so I've settled on three main takeaways.

## Software is like writing a maths proof

I know what you're probably thinking, so I apologise for the wankishness of that statement. However, I do really feel like it is true, as solving proofs (like writing software) is like fighting a dragon. For those who have never attempted tough proof-based mathematics before, that probably sounds like an incredibly ridiculous and self-aggrandising thing to say. Yet it is real. 

My favourite mathematician, Dr Griffith Ware, first showed me a cartoon many years ago that I wish I could find now. In it, a lone mathematician stands in front of a towering dragon, with no weapons besides a pen and paper. The reason for this metaphor is that solving mathematical conundrums often feels like pitting yourself an entity that can fight back. It doesn't give up easily, it surprises you - and worst of all - it can lure you in with false promises, only to dash your hopes. It can feel like writhing beast, resistant to any attempts to control it. However in the end, you slay it.

This experience I found was not dissimilar to writing code. You think it will be easy, and that the concepts in your head will translate nicely to pixels on a screen. However, more likely is the case that it fights back as well, and does not yield until significant effort is exerted. Like proofs, solving problems with code can sometimes take many implementations and iterations to find an approach that works. 

But hey, easy done is no fun. 

## Software is about people

I currently work as a project manager. In this role I have to, among other things, deal with people. Being effective in my role means not just understanding the project requirements and the client needs, but the personalities, desires, dislikes, and capabilities of the development team. I've come to see that being successful here necessitates good people skills. On the other hand, completing this Blue Lagoon project has shown me what the other end of the spectrum looks like: a one-man shop. My would-be group member fucked off and fucked off early, so I built the game, for better or for worse, by myself. 

While this did mean I could not get as much done as other groups, this was not the actually disappointing thing for me. What it did was rob me of the opportunity to experience the triumphs and tribulations of team-based software development, and all the quirks that go along with it. I did not get to learn from others, be exposed to new approaches, nor see how to do things better. What I did develop however, was a keen sense of the *value* of other people. Going it alone can be easy at first because you avoid conflict, but in reality, you sell yourself short.


## It takes longer time than you think, but also less

The third and final takeaway is a lesson in incrementality. To all the experienced developers out there, I'm sure this one comes to you as a great surprise. While I am no stranger to having an Agile mindset, I thought this project highlighted the contradictory statement's truth. Sure, it took me a month and a half to make something not even complete. Yet by finding small gaps of time here and there to make small progress, I was able to complete much more than I thought I was going to be able to. It reminds me of one of my favourite quotes, the ancient proverb that divines:

> The best time to plant a tree is 20 years ago. The next best time is today. 

And so it is with development, personal and software-related. While I dislike the hare and the tortoise fable generally, unfortunately it feels relevant here. Making those small advances, wherever and whenever possible, is everything. Or at least, I hope so.  