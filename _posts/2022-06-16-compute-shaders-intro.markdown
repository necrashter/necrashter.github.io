---
title:  "Introduction to Vulkan Compute Shaders"
date:   2022-06-16 21:02:00 +0300
permalink: /ceng469/project/compute-shaders-intro
tags:
- ceng469
categories:
- graphics
toc: true
sidebar:
  nav: "ceng469"
excerpt: "The basics of Vulkan compute shaders are explained in a video."
---

For the final project of CENG 469 Advanced Graphics Course, I wanted to do something that involves compute shaders.
Because the other project topics either felt too limited (e.g. implementing a single technique like SSR or SSAO) or I didn't have the required hardware (e.g. real-time ray tracing).
With compute shaders, there are infinite possibilities.

To get started with the compute shaders, I decided to implement a GPU particle system with them.
Since this is a broad area, I will have the opportunity to extend on it as much as I want after I implement the basics.
If you have read my previous blog posts about 469 homeworks, you know I always like experimentation and creative expression.


# Presentation

I created a presentation that explains the basics of compute shaders in Vulkan.
I originally gave this presentation in real life, but today I decided to record it.
Here's [the video](https://youtu.be/KN9nHo9kvZs):

{% include video id="KN9nHo9kvZs" provider="youtube" %}

I thought I should have asked someone to record me during the original lecture, because I found it harder to deliver this presentation in front of the computer.
But don't worry; I recorded some outtakes and edited the vidoe in order to make tt more bearable to watch.


## Resources

You can access [the slides here](https://docs.google.com/presentation/d/1KrDE7a3WT551gjLwa0OO09kgZ71lKJf-6CR6kVVeVAM/edit?usp=sharing).

Also, you can find the repository that contains the minimal Vulkan compute shader application [here](https://github.com/necrashter/minimal-vulkan-compute-shader).
It also features a CMake build system, but I only tested it on Linux.


## References

The Vulkan code is based on [this blog post](https://bakedbits.dev/posts/vulkan-compute-example/).
In there, you will also find the HLSL implementation of the shader I showed you.

Here's [a link to GPU Gems 3 book](https://developer.nvidia.com/gpugems/gpugems3/contributors) I mentioned in the presentation.

Some images has been taken from [Cem Yuksel's lecture about the same topic](https://www.youtube.com/watch?v=HH-9nfceXFw).
I recommend you to watch that lecture if you would like to learn more about the basics of compute shaders and their usage in OpenGL.
However, that lecture doesn't cover the Vulkan side.


Sources of other images:
- OpenGL vs Vulkan slide
	- [nVidia: Transitioning from OpenGL to Vulkan](https://developer.nvidia.com/transitioning-opengl-vulkan)
- Memory slide
	- [Video card icons created by prettycons - Flaticon](https://www.flaticon.com/free-icons/video-card)
	- [Cpu tower icons created by Good Ware - Flaticon](https://www.flaticon.com/free-icons/cpu-tower)
- Particle system slide
	- [Fire](https://realtimevfx.com/t/sketch-10-jordanov/4273)
	- [Smoke](https://giphy.com/gifs/blobbybarack-art-loop-blobby-barack-JUnLuAEjsbUcM)
	- [Electric zap from Solalien](https://necrashter.itch.io/solalien) (It's a link to the game I mentioned)


Further reading:
- [Compute Shader in OpenGL Wiki](https://www.khronos.org/opengl/wiki/Compute_Shader)
- [A presentation about compute shaders from OSU](https://www.khronos.org/assets/uploads/developers/library/2014-siggraph-bof/KITE-BOF_Aug14.pdf)
- [nVidia: Transitioning from OpenGL to Vulkan](https://developer.nvidia.com/transitioning-opengl-vulkan)
- [GPU Gems 3](https://developer.nvidia.com/gpugems/gpugems3/contributors)
	- [Chapter 29. Real-Time Rigid Body Simulation on GPUs](https://developer.nvidia.com/gpugems/gpugems3/part-v-physics-simulation/chapter-29-real-time-rigid-body-simulation-gpus)
	- [Chapter 30. Real-Time Simulation and Rendering of 3D Fluids](https://developer.nvidia.com/gpugems/gpugems3/part-v-physics-simulation/chapter-30-real-time-simulation-and-rendering-3d-fluids)
	- [Chapter 31. Fast N-Body Simulation with CUDA](https://developer.nvidia.com/gpugems/gpugems3/part-v-physics-simulation/chapter-31-fast-n-body-simulation-cuda)
