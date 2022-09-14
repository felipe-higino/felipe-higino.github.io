---
lng_pair: id_asmdefs1
title: Using Assembly Definitions in Unity
category: Unity
tags: [Unity, Tools, Architecture]
img: ":id_asmdefs1/asmdef-asmref-thumb.png"
date: 2022-09-10 00:24:44 +0000
---

Why you should use `Assembly Definitions` in your next Unity project? And why you shouldn't? 

For the ones who don't have enough time to read (sadly 🙁), here is the key answers:

## TL;DR

### Why
- guarantees project features granularity
- better dependency management (better code architecture)
- better testability (unit tests integration)
- reduce compile times (exponentially decreases iteration times)

### Why not
- overusing can increase runtime memory usage
- team members familiarity
    - can cause merge conflicts
    - not undestanding how dll dependencies works

### WIP