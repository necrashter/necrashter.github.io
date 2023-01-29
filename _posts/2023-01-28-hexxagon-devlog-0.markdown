---
title:  "Hexxagon Devlog: A new game in Godot & Rust"
date:   2023-01-29 00:57:00 +0300
permalink: /hexxagon/devlog/0
categories:
- gamedev
toc: true
---

I started developing a board game called "Hexxagon", which is basically [Ataxx](https://en.wikipedia.org/wiki/Ataxx) played on a hexagonal board.

![Screenshot](/assets/hexxagon/ss0.1.0.webp)

<center><a class="btn btn--primary btn--large" href="/hexxagon/0.1.0/" >Play 0.1.0 on Web</a></center>


# Tech Stack

I used a custom build of [Godot Engine](https://godotengine.org/) 3.6 Beta with a GDNative library written in Rust.
AI and game logic is handled with Rust, the rest is GDScript, including animations, menus, etc.
Overall I liked this approach.
Because there are many cumbersome tasks which are easier and quicker to accomplish with GDSCript.
Performance critical sections of the code can be written in a GDNative-supported language such as C/C++ or Rust without recompiling the engine.
This makes it possible to iterate rapidly.

I preferred Rust over C/C++ not because of memory safety, which is what Rust is known for.
It's not a must-have if you have enough experience in C++.
I preferred Rust primarily because of it's toolchain and language features.
It's build system `cargo` makes it really easy to start and organize projects.
Rust have taken the concept of zero-cost abstractions into heart and thus allows you to write low-level optimized code without sacrificing readability or maintainability.

However, compiling Rust GDNative library proved to be a challenge because that part of the `godot-rust` project is not mature yet.
After wasting a day on this, I decided to create a [PR](https://github.com/godot-rust/book/pull/89) on Godot Rust Book so that others won't have to go through this.
There's no reason to repeat the errors I encountered here because no one reads this blog anyway.

Another issue I have with Godot web build is that if you put the game in an `iframe` inside a web-page, you cannot scroll the page if your cursor is on the `iframe`.
At first I tried embedding the game directly into this page but I couldn't find a way to solve this.


# AI

The currently available AI's in the game and their behavior:
- AI (Random)
  - Selects a random move from the list of available moves.
- AI (Easy)
  - [Negamax](https://en.wikipedia.org/wiki/Negamax) algorithm with alpha-beta pruning and depth = 1.
- AI (Hard)
  - [Negamax](https://en.wikipedia.org/wiki/Negamax) algorithm with alpha-beta pruning and depth = 2.

[Negamax](https://en.wikipedia.org/wiki/Negamax) is a variation of [minimax search](https://en.wikipedia.org/wiki/Minimax) algorithm condensed for 2-player case.

Depth = 2 means that the hardest AI will only consider the next 2 moves while making a decision.
Despite that, it is a tough opponent in single-island boards, i.e., maps in which players don't start in separate islands.
When there are multiple islands though, it's competence suffers greatly.
This is especially prominent on "6 Arrows" map, in which AI will focus on capturing all tiles on its starting island instead of colonizing other islands.

Random AI exhibits "exploring" behavior, i.e., jumping around the map without cloning or capturing any pieces, which is the biggest mistake in Hexxagon.
This is because there are more jumping moves than cloning moves for each piece since there are more tiles on ring-2 than ring-1.

Although most articles are specialized for chess, [Chess Programming Wiki](https://www.chessprogramming.org/) is a good resource for programming an AI like this.

## Board Representation

[This interactive Red Blob Games article](https://www.redblobgames.com/grids/hexagons/) is a definitive source for programming hexagonal grids.
Check out the cube coordinates section there and get ready to have your mind blown.

Godot tilemap uses offset coordinates for hexagons and a hash map for storing the tiles.
My GDNative Rust library converts offset coordinates to axial coordinates since algorithms are faster and easier to implement in axial coordinates.
Board is stored as a hash map on the Rust side as well, since it makes it trivial to store complex boards.
Although hash maps usually use more memory than arrays, arrays also waste space when storing arbitrarily-shaped hexagonal boards.
I don't think it's worth switching to arrays.

Currently I'm using the `HashMap` implementation in Rust standard library, but I will change it to `hashbrown`.

## Move Generation

Each move is represented as a pair of axial coordinates.
The first one denotes the location of the piece which performs the move, and the second one denotes the empty target tile.
For each piece, it's straightforward to generate all possible moves by iterating the hexagons on the rings 1 and 2, and checking whether they are empty.

This approach works great for representing the possible moves to the player in the UI.
However, for AI, it's not optimal.
Two neighboring hexagons share 2 neighbors, to which both of them can clone a piece.
See the figure below.

![Screenshot](/assets/hexxagon/same-cloning.webp)
<center><i>You can choose either the top or the bottom piece to clone a new piece to the green tiles.</i></center>

Note that the source piece from which we clone the new piece has no effect on the outcome as long as the target tile is the same.
By eliminating duplicate moves that lead to the same successor board state, we can improve the performance of minimax search.
Therefore, we don't have to explore both moves.

## Alpha-Beta Pruning

[Alpha-Beta Pruning](https://en.wikipedia.org/wiki/Alpha%E2%80%93beta_pruning) is treated like a must-have for a board game AI like this.
But surprisingly, I didn't receive any noticable performance improvement from it, presumably because
1. The depth is 2, which is not too high anyway.
2. There's no good move ordering heuristic. Alpha-beta pruning depends on this.

Currently, moves are ordered based on captured pieces, but this is a terrible heuristic in many cases.
That's why Easy AI can make terrible mistakes. It usually weakens its position and sacrifices many pieces in order to gain only a few pieces immediately.

I think I should optimize the search algorithm in another way.
Because alpha-beta pruning assumes that the game is a [zero-sum game](https://en.wikipedia.org/wiki/Zero-sum_game).
This property holds for 2-player Hexxagon, but it breaks when you have 3 players.
I don't want the AI to be limited to 2-player games.


# Graphics

I made the tiles and pieces with Blender.
I'm happy with how the game looks so far, but backgrounds will require some work.

