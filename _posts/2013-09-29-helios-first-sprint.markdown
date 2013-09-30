---
layout: post
title: "Helios, first sprint retrospective"
date: 2013-09-29 20:51:35
comments: true
categories: [atlantis, code, production]
---
This month, I started developing a new Atlantis game, code-named Helios. I am
hoping that other developers will join my effort eventually, so I am spending
more time than usual on project management and documentation. Today sees the
end of my very first sprint, and I am pretty stoked about how much I got done.

<!-- more -->

## Project planning

In agile development, the project gets broken down to sprints -- intervals of time
that are easy to plan for. In my case, I am planning to do a print per month,
because it forces me to prepare a ton of design work, and because my time is
easier to predict for a month than a week. I think I can commit to 5 full days
of work during a month while still having a job.

For each of the sprints, I pick a few user stories, break them down to
bite-sized tasks which can be finished in less than a day, and assign as many of
these to the sprint as I think are possible to finish in a month. Since the
tasks are small and the design for their user story is complete and written
down, new contributors should be able to pick up any one of them that they feel
comfortable working on. For this purpose, I keep my [task board on Trello]
(https://trello.com/b/EzbIyrUS/new-atlantis) updated.

Even as a single-team developer, this helps me focus on my work. There is
satisfaction in seeing how many tasks I have moved, and I never wonder what
needs to be done next, so I don't work on stuff that isn't important.

## What got done

I have planned an aggressive [list of features]
(https://github.com/badgerman/atlantis/wiki/Helios-Game-Design-Document)
for Helios, and I wrote Design rationale and a task breakdown for all of them.
This gives me material for all my future sprints until the start of the game.

The core feature I worked on for first was the most difficult one. I believe in
addressing risk head-on, so I worked on multi-hex movement for ships. Talking to
the atlantisdev folks helped me narrow the scope to only ships, which don't have
to worry about guards, terrain modifiers, and other complications to the source.
I also decided to delay the new action system I have planned, and in the end it
turned out to actually be pretty straightforward.

I got a lot of other small and large things shipped. My other focus was making
more things configurable on a per-game basis, by moving them into the json
configuration file:

* players start with 10 men and $2000, which are configurable values.
* the width/height of the map is configurable, but stays fixed after world
  generation, and does not need to be recalculated every time the data file is
  loaded.
* Regions have a unique id, and the neighbor-graph of all regions is stored in
  the data file, which makes coordinates a merely cosmetic attribute of a
  region. They are no longer needed to rebuild the region::connect array.
* All regions are stored in a hash table, so they are fast to look up if you
  know their unique ID.
* Ship classes are configurable, with speed, cost, and capacity. The game is no
  longer limited to only three types of ship, although the Helios game will
  probably still have a longboat, clipper, and galleon.
* I killed the original world generator, leaving only the code that was used in
  Atlas. This will eventually be modified, too. It looks like the next sprint is
  going to have world generation as one of its themes.
* Terrain types are configurable. For Helios, all production in a region has
  been reduced by 80%.
* The code compiles on stock Ubuntu 13.04 again.
* I compiled and familiarized myself with the ALH code.

## What did and didn't work

The atlantisdev group is a great sounding board for ideas. I am not a game
designer who creates things in a vacuum, and talking about my plans with other
people helps me a lot.

I am still looking for other developers to help me. Two people have stepped up,
and I have added them as collaborators to the Trello board, but none of them has
started on any task work.

Communication with other developers is mixed. I started an #atlantis IRC channel
on irc.lunarnet.org, but only one person has been showing up regularly.
Hopefully, this is going to get better. Please, if you are interested in
Atlantis developments, even just as a lurker, I want to talk to you.

Investigating ALH helped me get some ideas about world coordinates sorted out,
realizing that the code didn't need them, and as long as the client can draw a
map, neither do the players. I am probably going to let players choose how they
want them laid out on a per-faction basis, with a default preference for the
ALH layout.
