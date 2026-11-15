---
name: ola
description: "Ola is a tiny library for smoothly interpolating numbers over time; use it when a value (position, opacity, price, counter) should glide to its target instead of jumping, or in any project that depends on the ola package."
---

Ola (franciscop/ola, MIT, ~1.8k stars) — tiny (~1 KB) library that smoothly interpolates numbers toward a target value. Give it a number, change the target, and it glides there with configurable smoothing. Repo: https://github.com/franciscop/ola

Install:
```
npm install ola
```
Verified package `ola` 1.2.2, MIT. No account or GPU needed.
When to use vs alternatives: pick Ola only when you need smooth number interpolation (live counters, sliders, sensor values). For full animation (timelines, easing, transforms) use Motion, Anime.js, or Shifty instead — Ola does one thing and nothing else.
