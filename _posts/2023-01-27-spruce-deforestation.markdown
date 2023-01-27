---
title:  "Spruce Deforestation: 23rd libGDX Jam Submission"
date:   2023-01-27 17:11:00 +0300
permalink: /spruce-deforestation/post
categories:
- gamedev
toc: true
header:
  image: "/assets/bg/spruce-deforestation.webp"
  full_image: false
---

Spruce Deforestation is an FPS game I made a while back for the 23rd libGDX jam, the theme of which was "Dystopian Christmas".

# [Play the Game on itch.io](https://necrashter.itch.io/spruce-deforestation)

The web build is available on [the game's itch.io page](https://necrashter.itch.io/spruce-deforestation), as well as the desktop builds.

Furthermore, the game is now open-source and the models are freely available for everyone:
- [GitHub Repository](https://github.com/necrashter/spruce-deforestation/)
  - You can also build the game for Android from source code. Touchscreen controls are supported on mobile.
- [3D Models on OpenGameArt](https://opengameart.org/content/spruce-deforestation-models)
  - Contains the original `.blend` files for the 3D models I created during the game jam. Some are based on [kenney](https://kenney.nl/)'s models.

# Soundtrack

{% include video id="uZ_U0-OJQiE" provider="youtube" %}

The first mix was compressed more aggressively and hence louder, but I learned that YouTube applies its own normalization anyway.
Therefore, I didn't even use a compressor/limiter in the mix you see here.
The same goes for in-game version of the soundtrack.

However, I am not happy with the mix because it sounds quiet relative to other songs when you listen to it locally, i.e., not on YouTube.
Also, the second song sounds quiet in-game.
I think I should have mixed it normally and not care about YouTube's normalization.

Apart from mixing and mastering, I'm happy with how the song turned out.
It took one and half days to compose and record these 2 songs during the game jam.

I used LMMS for synths, Ardour for mixing and mastering, and Guitarix as a guitar amp.

# Some Postmortem

I have used libGDX years ago when I was a beginner at game development.
It has been a long while (about 4 years) since I last used it, so I wanted to give it a shot again.
I think it might be beneficial for a beginner to use libGDX because it teaches you how the things work under the hood since it's a low-level framework.
After learning OpenGL, libGDX feels like a thin OpenGL wrapper with some utilities for game development.

However, I have realized I really don't like Java after this project.
Garbage collector becomes more of a pain rather than a help especially for game development where performance is important.
You have to use `static Vector3` fields everywhere because you cannot allocate a simple struct containing 3 floats on the stack.

The only good remark I have about Java is the inner classes, which I exploited to implement a state-machine AI quickly.

Years ago, I preferred Java because it could run on Android and all desktop OS's.
Today, it feels like a closed ecosystem.
You can run almost anything on any platform these days.

It's said that Java was developed in order to avoid memory bugs in C/C++.
Languages like Rust, the new features in latest C++ versions, or just the pure programming experience seem like better ways to address these memory problems.

I know most of these are my personal opinions but this is my personal blog.


## AI

All of the game takes place on a procedurally-generated Perlin noise terrain, because I had [prior experience](/ceng469/hw3/) on this topic.
NPCs are simple state-machines as I mentioned previously.

Surprisingly, I received positive comments about the AI both from itch.io and real-life friends despite the simplicity of AI.
This is because the perceived intelligence does not necessarily reflect what is under the hood.

For example, one of the biggest obstacles was how to make NPCs navigate through 3D environment.
To solve this problem, I made them simply go towards the target in a straight line, and follow the boundaries whenever they hit an object.

Similarly, the original Pacman game contains many tricks to make the AI seem intelligent with extremely limited computing power by today's standards.
See the [Pacman Dossier](https://pacman.holenet.info/) for more info.


## Octree

I used an octree implementation for faster collision checking.
On modern computers, this was not needed.
But I have an old Android tablet on which I wanted to run the game.
Despite the octree, the game crashes on level start on that device.

It's not trivial to convert octree back to naive implementation now.
Thus, I don't know how much it affects performance on web builds.

Implementation of octree was much simpler than I initially thought.
