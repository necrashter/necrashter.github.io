---
title:  "Bevy Jam Simulator: Game Jam Retrospective"
date:   2024-01-24 20:21 +0300
permalink: /bevy-jam-4
categories:
- gamedev
toc: true
header:
  image: "/assets/bg/bevy-jam-sim.png"
  full_image: false
---

Last month, I've participated in [Bevy Jam 4](https://itch.io/jam/bevy-jam-4) with [Pyrious](https://pyrious.itch.io/), someone I met in Bevy's Discord server.

# [Play the Game](https://pyrious.itch.io/bevy-jam-simulator)

- You can play the game [on itch.io using this link](https://pyrious.itch.io/bevy-jam-simulator).
- The game is open-source and hosted in this [GitHub Repository](https://github.com/benfrankel/bevy_jam4).
- Soundtrack is [available on YouTube](https://youtu.be/0JXQqLwuy6E).


# Retrospective

We won **the second place** in Bevy Jam, the best I have achieved in a game jam so far.
The theme of the jam was **"That's a LOT of Entities!"**.
In our game, you play as a game developer participating in Bevy Jam 4, trying to make a game with a lot of entities.
As you can tell, the meta aspect of our game was its strongest suit.

This blog post is based on our retrospective discussion with Pyrious.


## Planning

Pyrious came up with the main idea behind this game early in the game jam. We considered other options that fit the game jam's theme, mainly Vampire Survivor clones, generic bullet hell games. However, we anticipated that everybody would be making these kinds of games. Therefore, we decided to use Pyrious's idea for uniqueness.

Pyrious said that the planning phase was productive and fast, and that having a 2-person team helps.
I didn't participated in any game jam with a larger team, but I can imagine that having more than two people would slow down the planning process, especially if the team members are random people all around the world.
We were done with the general planning after a few hours following the start of the game jam.

We tried to keep the scope small and allocate time for polishing towards the end of the jam.
Overscoping is one of the most common mistakes in game jams, and Pyrious observed that most successful game jam games have small scopes but lots of polish.

A game (or any artwork in general) is never finished, only abandoned.
There's always room for improvement, but we managed to implement some extra features in addition to all of the core gameplay elements we wanted.
On the other hand, despite the small scope of the game, we reached the publishable state much later than I initially thought.


## Team Work

This is the first game jam in which I collaborated with someone I don't know in real life. Despite this, it was one of the best instances of team work for me.

One of the counterintuitive things that helped is being on wildly different timezones.
Thus, we couldn't reach each other most of the time, but this allowed us to code asynchronously, without worrying about merge conflicts.
As a result, we didn't even use separate branches and just pushed our changes directly to the main branch.
We coordinated twice a day and it was enough since we agreed on the general direction.

![Screenshot](/assets/bevy-jam-4/early-ui.png)
<center><i>An early version of the main game UI.</i></center>

In the first few days, Pyrious overhauled the game UI that I made.
We couldn't decide which design would be better.
Other than that, we didn't fix or polish each other's changes most of the time.
This made asynchronous coding more effective.

One of the most important things I learned in this jam was continuous deployment.
I didn't know you could build and upload games directly to itch.io using GitHub Actions.
Since Pyrious had this template before the jam, it gave us a quick and easy jumpstart.
I will definitely consider this in my future projects.


## Difficulties

The Bevy's UI system is not fully mature yet, and unlike Godot, it lacks a graphical editor.
This made UI development harder, and unfortunately, our game was almost completely UI-driven.
Also, as I mentioned earlier, we wasted some time trying out different game UI layouts.
Initially, my plan was to join this jam solely as a musician without writing code.
But I decided to partake in coding since we were a two person team and UI seemed to be harder than expected.

Despite our efforts that went into polishing the UI, there was a tooltip positioning bug caused by monitors with different DPIs.
We were aware of this bug during the game jam, but we couldn't solve it before the deadline because we didn't know it was caused by differing DPI values.
Fortunately, [Rob Parrett](https://github.com/rparrett) solved this bug and submitted a pull request after the game is released. Thanks!

In the last day of the jam, I came up with [this workaround](/bevy-choppy-music-workaround) for the choppy music problem that occurs in the web builds during performance-heavy sections.

I began treating the game as if it's finished towards the end of the jam.
Hence, I postponed working on the Bevy web choppy music bug, performance issues, and balancing.
I underestimated the amount of work required for balancing in particular.


## Too Many Entities

To comply with the game jam's theme, we had to spawn a lot of entities.
Or rather, create the illusion that there are a lot of entities.
Since the entities count can exceed trillions (and even reach infinity, as represented by a 64-bit floating point number) towards the end of the game, it's not feasible to simulate and render each entity.

We brainstormed about many different approaches to this problem, including despawning entities that go out of bounds, having a lifetime, etc.
But in the final submission, we settled with this:
- **Entity cap:** Define a maximum number of entities to be simulated and rendered.
  - We use different entity cap values on desktop and web, since web is more limited.
- **Pooling:** Spawn all of these entities immediately at the start of the game, but don't display them.
  - We initially implemented pooling without spawning all entities. But then we noticed that spawning a new entity causes lag in the web build, so we decided to spawn all of them at the start.
- When a new entity spawns, get the next element from the pool. The next element is either an entity that never spawned before, or the oldest entity in game.

This system creates the illusion that there are a lot of entities.
Of course, any person sufficiently experienced in game development can guess what kind of tricks we use under the hood.
But during playtesting, we observed that our game can fool non-technical people into believing that there are actually a lot of entities.
Pyrious even said that one of his friends asked him about how the game manages to handle so many entities.


## Gameplay

Since this is an incremantal idle game, balancing was tricky.
We spent a lot of time playtesting and making small adjustments in the final days of the jam.

In the review period, we noticed that a lot of people were confused about the technical debt mechanic, which was left somewhat implicit to the player.
We overestimated the player ability to figure out these implicit game mechanics on their own.
Explaining the game mechanics in a transparent/implicit way is great when done correctly, but we couldn't execute this as nicely as we thought due to the time limitations in the game jam.
Besides, some of the game mechanics assumed that the player is familiar with software development.
However, not everybody participates in game jams as a coder.
Some people don't even know programming and join as an artist or musician etc.

I think people don't want to invest too much effort into playing a game jam game, and they expect game jam games to be simpler.
We think that a significant portion of the players don't even read the descriptions.



# Soundtrack

{% include video id="0JXQqLwuy6E" provider="youtube" %}

I created a synthwave track using LMMS since I thought that synthwave is appropriate for the coding theme of our game.
Thanks to Pyrious' brother, who provided some feedback.
I'm happy with the composition.
The mixing is not perfect and I think that's the weakest aspect of the track, but I hope it's good enough.

One of the new things I tried in this track was experimenting with increasing the drum reverb using automation in various sections of the track.
I think this worked well in the end, especially when combined with the abundance of the entities shown in the screen.

Alongside the soundtrack, I was responsible for all sound effects in the game.
I recorded some sound effects from scratch, including mechanical keyboard sounds and guitar sounds.
I edited some free sound effects I've found to create the upgrade sound effects.


# Conclusion

In terms of rating, this was my best game jam so far since we won the second place.
In previous jams, I would usually incorporate the theme into the story but not gameplay.
In this game jam, we turned the theme into the core mechanic of the game, which contributed to the success of our game since the theme interpretation constitutes 33% of the overall score.
Designing a simple game gave us time to work on presentation, which is another 33% of the overall score.
Finally, making the game fun boils down to good game design in combination with a lot of playtesting and balancing.
All of these require time at the end for polishing.

Thanks to Pyrious for teaming up with me in this game jam!
