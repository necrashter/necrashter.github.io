---
title:  "Tackling the Generative Learning Trilemma with Denoising Diffusion GANs"
date:   2023-05-26 18:54:00 +0300
permalink: /denoising-diffusion-gans-presentation
excerpt: "The presentation I made about this ICLR 2022 Spotlight paper."
---

I prepared a presentation about this ICLR 2022 Spotlight paper for [METU CENG 796 Deep Generative Models](https://user.ceng.metu.edu.tr/~gcinbis/courses/Spring23/CENG796/index.html) course in Spring 2023.
Since I noticed that there are almost no videos about this paper on the internet, I decided to record it and publish it.

Here's [the video](https://youtu.be/0OD9Dx2CADU):

{% include video id="0OD9Dx2CADU" provider="youtube" %}

I don't like the white background in the presentation, but I didn't bother to change it since all figures from the paper had white backgrounds.


## Resources & References

- [Presentation slides](https://drive.google.com/file/d/1wpWD5B3B5_XbsUQrCq_tXs-VQsaLCVgR/view?usp=share_link)
- Zhisheng Xiao, Karsten Kreis, Arash Vahdat (2021). [Tackling the Generative Learning Trilemma with Denoising Diffusion GANs](https://arxiv.org/pdf/2112.07804.pdf).
- [Project page of the paper](https://nvlabs.github.io/denoising-diffusion-gan/)
- [GitHub repository of the paper](https://github.com/NVlabs/denoising-diffusion-gan)
- Jonathan Ho, Ajay Jain, Pieter Abbeel (2020). [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239).
- Ian J. Goodfellow, Jean Pouget-Abadie, Mehdi Mirza, Bing Xu, David Warde-Farley, Sherjil Ozair, Aaron Courville, Yoshua Bengio (2014). [Generative Adversarial Networks](https://arxiv.org/abs/1406.2661).


## Comments

The paper and the reported results are quite impressive.
However, to the best of my knowledge, there are no real-life applications that use the method proposed in this paper.
Diffusion models was booming in the last year; we have seen great applications such as Stable Diffusion, Midjourney, and so on.
Despite the reported benefits of this method, none of the diffusion applications used this method.

The [GitHub repo](https://github.com/NVlabs/denoising-diffusion-gan) of the paper is basically abandoned: only 3 commits, and the latest commit dates back to the last year.
It's a shame that they didn't improve it further.
One of the possible reasons, as hypothesized by our course instructor [Gökberk Cinbiş](https://user.ceng.metu.edu.tr/~gcinbis/), is that people didn't want to work on this line of research because GANs are notoriously hard to train.
