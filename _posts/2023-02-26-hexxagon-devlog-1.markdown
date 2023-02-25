---
title:  "Hexxagon Devlog: WebRTC, UI"
date:   2023-02-26 00:16:00 +0300
permalink: /hexxagon/devlog/1
categories:
- gamedev
toc: false
---

I updated my Hexxagon implementation.
See [previous devlog](/hexxagon/devlog/0).

I don't plan to have a separate release for each version on this website.
The following link is the latest version, which may be updated after this post.

<center><a class="btn btn--primary btn--large" href="/hexxagon/latest/">Play Hexxagon on Web</a></center>

- Updated UI.
- Added new level "Radioactive".
- Implemented WebRTC multiplayer.
- Implemented "end-game fill": when one of the players is eliminated, the other player captures all remaining tiles automatically.

# WebRTC

I have been working on adding online multiplayer support to Hexxagon using WebRTC.
I already had a [WebRTC chat demo without signaling server](/webrtc-chat-demo), I decided to do the same thing since it doesn't require a server.
Well, almost.
I found out that in many cases you need a TURN server.
However, I thought this problem would be fixed in the future after widespread IPv6 adaptation. 

Godot's implementation of WebRTC is simpler and does not feel feature-complete but it gets the job done.
I couldn't find a way to wait until all ICE candidates are completed.
But it doesn't seem to be a problem.

I'm not happy with WebRTC.
First of all, copying & pasting signaling data is quite inconvenient.
More importantly, clipboard doesn't work correctly in all browsers.
It worked well on the same machine while testing but it's extremely fragile in real-life use cases.
Finally, needing a TURN server defeats the purpose, since the goal of using WebRTC was peer-to-peer multiplayer.

The time and effort I spent for developing the WebRTC multiplayer feels wasted now.
