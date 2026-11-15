---
name: lume
description: "LUME is a toolkit for GPU-powered 3D HTML elements; use it when building 3D scenes with plain HTML/CSS-like markup (custom elements such as <lume-box>), or in any project that depends on the lume package."
---

LUME (lume/lume, MIT, ~1.5k stars) — GPU-powered 3D HTML. Write 3D scenes as HTML custom elements (`<lume-box>`, `<lume-sphere>`, …) with CSS-style transforms; it renders with WebGL underneath. Repo: https://github.com/lume/lume

Install:
```
npm install lume
```
Verified package `lume` 0.3.0-alpha.46, MIT. Note: still alpha — APIs can change. Works in any modern browser; no account needed. A GPU helps (it is WebGL-based) but it runs on CPU fallback too.
When to use vs alternatives: pick LUME for quick 3D scenes written as HTML, e.g. product showcases or simple 3D web toys. For full 3D engine work use Three.js or Babylon.js; for agent-driven 3D scenes use the 67.js engine already installed here (`build-67js-scene` skill); for 3D animation film work use Blender.
