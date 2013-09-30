---
layout: post
title: "Atlas: Postmortem Notes"
date: 2013-09-20 00:32
comments: true
categories: [atlantis, games]
---
Atlas, my game of Atlantis 1.0 (with small modifications) is finally ending in
turn 22, by vote of the remaining players. There are 6 factions left, and it's
time to look at what worked and what didn't. I kind of knew a lot of this
beforehand (A1 is pretty well-known), but I have not seen a critical
retrospective of past games, and think it might be constructive to do a
write-up. I know a lot of this is fixed in A5 or Eressea, but remember I wanted
to go back to basics and see what the world was like 20 years ago when Russell
Wallace first created it.
<!-- more -->
## What worked

1.  The game runs smoothly on all three types of machines I used: Windows (x64),
    Ubuntu (x86) and Raspbian (ARM).
2.  I have a very good setup for processing turns.
3.  I can run multiple games side-by-side on the same machine (untested).
4.  Kept a regular schedule, the automatic turns worked (almost) every weekend.
5.  Few serious bugs: I had to re-run only 2 turns.
6.  Hex maps: major new feature that didn't break.
7.  JSON for reports: One player wrote a JavaScript mapping tool, and I wrote an order-template generator.
8.  I killed the TEACH command, which reduced a ton of micro-optimization.
9.  open-source game meant I was getting bug reports from people who read the code!

## What didn't work

1.  The map generator created too many unconnected continents.
2.  Some players were on an island all by themselves.
3.  Community: There wasn't much of a sense of community. I think that a forum
    does not replace a weekly in-world newspaper.
4.  The world in general was too big: I spawned three hexes per player, but in
    the endgame, my faction is spread over two dozen hexes. Player drop-off really,
    really hurts.
5.  All regions are exchangeable, and mostly the same. I recruit peons in every
    region, I tax as much as I can, and I build longbows/ships in forests vs.
    swords/armor in mountains. There is no strategic component to terrain at all.
    Why are swamps even in the game?
6.  Movement is incredibly slow. Getting raw materials out of a mountain or
    troops to the other end of the world is arduous.
7.  Micromanagement: I spent way too much time writing orders to pass money and
    materials around between units in the same region.
8.  Way too many units! The way skills work make it beneficial not to merge
    similar units, and the two major factions today both have in excess of 100
    units.
9.  Ships take too long to build. There are two longboats and one clipper, and as
    many again that are still under construction.
10. Buildings: ditto. A single faction built all three buildings that exist in
    the world.
11. There is a huge surplus of weapons and money. Taxation and Work are the main
    sources of income.
12. There hasn't been a large-scale war, and even though factions have
    researched all spells on levels 1+2, they haven't gotten to use them.
13. Community tool building: There is only one mapping tool that I know of, and
    the player kept it to himself.
14. Hex directions: I gave the two additional directions unconventional names
    (Mir and Ydd for NorthWest and SouthEast, respectively), so as not to have four
    directions start with the same 2 letters (abbreviations are frequently used by
    players when moving). This turned out to confuse people.
15. Wrap-around world. While it meant that nobody was at the edge of the map,
    this hardly mattered (no ships), and players were confused about the relative
    location of region (17,17) to (0,0). Not having tools made this worse.
16. The best strategy I observed was to send scout units out to look for players
    that never made a turn. Each crunchy starting unit contains a yummy core of
    $5000, which makes it worth to invest your entire starting capital to this rush
    strategy.

Did you play? The above are obviously just my impressions, but if you played and
disagree, or have something to add to my list, please add to it. I like
flattery, but also enjoy serious criticism. I'm doing this to learn, after all.

## What's next?

The game is playable, and anyone who would like to experience it should be able
to just pull it from github and play it. I think that in itself is a huge
success. At the same time, I didn't add a lot of bloat to it: You don't need an
XML library or OpenGL to run it, in fact my Raspberry Pi compiles it straight
from a fresh checkout :-) I wouldn't mind doing more development, but it would
take a bigger group of hackers. I have a huge list of ideas for features, in
fact, just not enough time to write them all. Let me list off what I came up
with during my hiking vacation (got to make the brain do something when you're
walking in the rain for 5 hours):

1.  Movement is kind of my #1 pet peeve. The current system makes ships
    pointless, and there is no way to make a haste spell, or give riders a movement
    advantage, or differentiate terrains. A system for multi-step moves needs to be
    added.
2.  Micromanagement sucks the joy out of the game. I'm currently favoring
    centralized upkeep from a faction-wide treasury, and city-based taxation instead
    of local TAX orders.
3.  Visual aids. I was toying with the idea of a GL renderer for maps, but did
    not get very far.
4.  Smaller ships. They take too long to build, and exploration only kicks off
    when everything has already been discovered by land. Two types of ship are
    probably enough.
5.  Actual empire-building. The current buildings are bunk, but I have a design
    for cities in my head, as economic centers of a player's empire, where taxation
    and recruitment are located.
6.  Fewer, better, units. I want to do Olympia-style stacks of nobles and common
    units, and I have most of that code already written.
7.  Starting a player off with more units and less money to take out the rush
    strategy.
8.  Make initial hiring of units for a rush harder by limiting recruiting to once
    every season (every N turns), from cities only. I'm sure that has other fun
    side-effects, too.
9.  Brainstorm on strategic value of terrain types. Movement penalties,
    obviously. Limit ships to landing in plains only.
10. New combat rules, with more strategic bonuses from nobles and unit stacking.
11. Eliminate skills in favor of unit archetypes. Archers, Knights, Pikemen,
    etc. have innate combat bonuses, and you recruit them for a combined price of
    money + items.
12. No more money on units. All money is locked in the treasury of a city, and
    cities pay upkeep or recruit units.
13. No more individual weapons and armor carried around by the units. If you
    recruit an Archer, it is assumed that she has a longbow, and certain fixed
    combat stats, plus situational modifiers. Some exceptions: mounts (horses,
    griffons) for movement bonuses, and money, to transfer it between cities. It may
    be that only nobles can carry money.

### Want to help?
So what I'm proposing is a pretty radical change of the game design, and the
outcome is probably more Olympia than Atlantis 5 or Eressea, with a few actually
different elements. My time is sadly limited, to the point where for the past
weeks I have delegated the majority of my faction's order writing to somebody
else (yes, the micro-management is that bad).

My past experience with trying to build a new games has been mixed. In Eressea,
we were three programmers with quite different ideas about the game we wanted to
build, and the end result was pretty successful. Building a new game from the
Eressea code has failed at least twice now, either because I was the only
programmer, or because the baggage of the old game was so big. That experience
was my main motivation to look at A1 again. Which means the ideal people I would
like to work with check these requirements:

*   Programmers. Adding non-programmers to a project increases the chatter, and
    does not increase actual productivity. Knowing what's possible in the code base
    should inform what features are designed :-)
*   Seasoned C programmers. Portable C99 is my favorite language, and the game is
    always going to be C at its core. I don't like C++ all that much, please do not
    suggest it.
*   Atlantis Players. I would still want to run new games upon reaching feature
    milestones, and developers should play in those games to see what works and what
    doesn't.
*   Basic computer skills. Must be able to set up their own development machine,
    know how to use IRC, ssh, git and a compiler.
*   Must be able to spell and form complete sentences. It's sad that this needs
    to be said, but this is the world we live in.

If you feel that you match this profile, and you have time and energy to work
on, drop me a note, and I'll see if there is enough of a response to kick this
off. It may not be today or this month, consider this offer good for a while.

Apart from people to work on a new game, I'd also like to work on better
tooling. The ideas I have been tossing around are a GUI client, compatible JSON
reports for A5 and Eressea (for cross-game tool sharing), a unit scripting tool
(Lua), and a mobile app (at least a map). There's something to be said for
sharing this kind of work among as many games as possible, and I'm favoring
things that are interoperable.
