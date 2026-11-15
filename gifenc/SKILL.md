---
name: gifenc
description: "gifenc is a fast GIF encoder for JavaScript; use it when encoding animated GIFs from raw frames or canvas output in the browser or Node, or in any project that depends on gifenc."
---

gifenc (mattdesl/gifenc, MIT, ~361 stars) — fast GIF encoding for JavaScript. Encodes RGBA frame data to GIF with good speed and small size; works in browser and Node. By Matt DesLauriers (well-known creative-coding author). Repo: https://github.com/mattdesl/gifenc

Install:
```
npm install gifenc
```
Verified package `gifenc` 1.0.3, MIT. No account or GPU needed.
When to use vs alternatives: pick gifenc when you need to EXPORT animation as a GIF (e.g. from a canvas animation or a Remotion/Motion Canvas frame sequence). It only encodes — it does not animate. For creating the animation itself use Motion Canvas, Remotion, or Anime.js; for playback of vector animation use Lottie Web.
