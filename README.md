# Linux Kernel - easy build using LXD/LXC

## Overview

One simple [Taskfile](https://taskfile.dev/) that makes it very easy to build the Linux Kernel for multiple architectures.  

By design, this build system maintains separate caches for each architecture making each build fast.

## Requirements

- Install Task -> https://taskfile.dev/  
It is a pretty nice Makefile alternative written in Go.  
We use this a sort of orchestrator/builder for our lxc containers.

- Install LXD  -> https://ubuntu.com/lxd  
Make sure everything works by first testing it out with a test container before going any further.  
`lxc launch ubuntu:20.04 ubuntu-container`


## How it works

The general concept is that the script takes a folder with the linux kernel source and then it maps it inside a container where it rsyncs it into an internal build folder (this makes it real easy to know if anything needs rebuilding at all) and builds with the default config for the architecture chosen.

After the build is finished, the container is stopped automatically (but not deleted).

Cross-compilation for other architectures is possible.

## How to use

First you should get a kernel via a tag:

```
TAG=v6.0-rc2 task getKernel
```

This will get you the kernel with the v6.0-rc2 tag.

### Build for x86

This will build the kernel with the default config:

```
TAG=v6.0-rc2 task buildKernel
```

### Build for arm64 by crosscompiling inside lxc

```
CROSS_COMPILE=aarch64-linux-gnu- TAG=v6.0-rc2 task buildKernel
```