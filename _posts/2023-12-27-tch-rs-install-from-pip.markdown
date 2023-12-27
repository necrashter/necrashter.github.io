---
title:  "Tch-rs Installation from PyTorch"
date:   2023-12-27 19:27:00 +0300
permalink: /tch-rs-install-from-pytorch
excerpt: "A short guide on how to install the Rust bindings of libtorch using a PyTorch installation."
---

Recently, I have been working on implementing deep reinforcement learning support for my [PowerRAFT](https://github.com/necrashter/PowerRAFT) project, which is written Rust.
Naturally, the first step was to install [Tch-rs](https://github.com/LaurentMazare/tch-rs), the Rust bindings of PyTorch, which depends on `libtorch`.
Tch-rs can install `libtorch` automatically using the `download-libtorch` feature.
However, due to the large file size of `libtorch`, it's preferable to use the same `libtorch` installation in both Python and Rust.
This also avoids installing `libtorch` for each project that depends on Tch-rs.
Besides, it seems like you have to install `libtorch` manually if you want CUDA 12.1 support with Tch-rs.

## Install PyTorch

First, you need to make sure that the correct version of PyTorch is installed.
As of 2023-12-27, Tch-rs v0.14.0 supports `libtorch` **v2.1.0** according to its README file.
The following command will install this version using PIP with **CUDA 12.1** support:
```sh
pip install --upgrade torch==2.1.0 torchvision==0.16.0 torchaudio==2.1.0
```

It's also possible to install PyTorch using Conda and use its `libtorch`.

PyTorch v2.1.1 also seems to work with Tch-rs, although I didn't test it thoroughly.
If you want to use a different version like that, you need to set the `LIBTORCH_BYPASS_VERSION_CHECK` environment variable.
{: .notice--info}

## Determine the Library Path

We need to figure out where the dynamic library files of `libtorch` are located.
To do that, start an interactive Python command shell and run the following commands:
```python
>>> import torch
>>> torch.__file__
'/home/ilker/miniconda3/lib/python3.10/site-packages/torch/__init__.py'
```
You will get the library path when you replace the `__init__.py` part with `lib`. In this example, the path we need to add to `LD_LIBRARY_PATH` is:
```
/home/ilker/miniconda3/lib/python3.10/site-packages/torch/lib
```

Note that if you are using a Conda environment, the same Conda environment must be active when you run your Rust project.

## Configure Environment Variables

Next, append the following to your `.bashrc`:
```bash
export LIBTORCH_USE_PYTORCH=1
export LD_LIBRARY_PATH="${LD_LIBRARY_PATH}:/home/ilker/miniconda3/lib/python3.10/site-packages/torch/lib"
```
The path in the second line must be the one that you found in the previous section.

The first line tells Tch-rs to use the Python installation of `libtorch`.
The second line is required so that your Rust program can find the `libtorch`'s dynamic libraries during runtime.

Don't forget to **restart your shell**.
{: .notice--warning}

After this, you should be able to add `tch` as a dependency to your Rust project normally.
Cargo will take care of the rest.

I hope this post was helpful.
