---
title:  "Workaround for the Choppy Music in Bevy Web Builds"
date:   2023-12-27 17:29:00 +0300
permalink: /bevy-choppy-music-workaround
excerpt: "Using Web Audio API to fix the choppy audio problems on the web builds of Bevy game engine."
categories:
- gamedev
---

If you have used the Bevy game engine to create a web game, you may have noticed the choppy audio bug, especially during performance-heavy parts. This happens because there is no separate thread for the audio (or any separate thread at all) on the web builds, at least for now (2023-12-27).

In the recent [Bevy Jam #4](/bevy-jam-4), I came up with a quick workaround for this problem.
The basic idea is as follows:
1. Disable the music on the Rust side for web builds using the `cfg` directives. All music-related code should be marked with `#[cfg(not(feature = "web"))]` so that it doesn't run on web.
2. Add a JavaScript code to the game's web page to start the music using the Web Audio API.

Unfortunately, this doesn't solve the issue completely; the sound effects will still be played by the Rust code, therefore they will remain choppy.
However, I think it's worth it because the choppy audio affects the music the most.

Here's the script you can add to your `index.html` to start the music after the first click on the game:
```js
function playMusic() {
  // Check if the browser supports the Web Audio API
  if (window.AudioContext || window.webkitAudioContext) {
    // Create an AudioContext
    const audioContext = new (window.AudioContext || window.webkitAudioContext)();

    // Load the audio file
    // TODO: You will probably need to update the file name:
    const audioFile = './assets/music/ingame.ogg';
    const request = new XMLHttpRequest();
    request.open('GET', audioFile, true);
    request.responseType = 'arraybuffer';

    request.onload = function () {
      // Decode the audio data
      audioContext.decodeAudioData(request.response, function (buffer) {
        // Create a buffer source node
        const source = audioContext.createBufferSource();
        source.buffer = buffer;

        // Loop the audio
        source.loop = true;

        // Connect the source to the audio context's destination (output)
        source.connect(audioContext.destination);

        // Start playing the audio
        source.start(0);
      }, function (error) {
        console.error('Error decoding audio file', error);
      });
    };

    request.send();
  } else {
    console.error('Web Audio API is not supported in this browser');
  }

  // Remove the click event listener after the first click
  document.removeEventListener('click', playMusic);
}

// Add an event listener for the DOMContentLoaded event
document.addEventListener('DOMContentLoaded', function () {
  // Add a click event listener to the document
  // to trigger playMusic on the first click
  document.addEventListener('click', playMusic);
});
```

You can add this code directly in a `<script>` tag, or create a new JavaScript file and add it to `index.html`.
Also keep in mind that you will probably need to update the audio file name.

That's the gist of it.
In our Bevy Jam #4 submission, [Bevy Jam Simulator](https://pyrious.itch.io/bevy-jam-simulator), we improved this script by [changing the volume](https://github.com/benfrankel/bevy_jam4/commit/d066d43b0b014ea1ad335edf308664b823ee623e#diff-8f62b6ced28d3396b501d2e89a2e7cb761d16cd7dc977aebece03d4a5da5c24e) and [making the music start after the second click](https://github.com/benfrankel/bevy_jam4/commit/9e93d44263a18f3358da56425aa02664fe52aa4a#diff-8f62b6ced28d3396b501d2e89a2e7cb761d16cd7dc977aebece03d4a5da5c24e).
You can see how these changes were made in the given commit links.

This approach can be extended by communicating between the Rust and JavaScript code in order to control the music.
This code will play the same song in a loop, which was good enough for our game, but it might be severely limited for your use-case.

A more rigorous solution would be to implement a new library that uses the Web Audio API as an audio backend on the web, but that would require a lot of work.
Also, there doesn't seem to be enough interest in this topic in the Bevy community.

**Update 2024-01-02:**
gak shared an example of accessing the Web Audio API from Rust using `bindgen` and `websys` in [this GitHub Gist](https://gist.github.com/gak/1dcc8b8f53ca5e8980311da6c4a56e6e).
{: .notice--info}
