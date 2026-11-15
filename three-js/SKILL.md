---
name: three-js
description: "Three.js is the standard JavaScript 3D library; use it when building 3D scenes, WebGL experiences, or 3D animation for the web, or in any project that depends on three or mentions Three.js."
---

Three.js (mrdoob/three.js, MIT, ~118.5k stars) — the standard JavaScript 3D library. Scenes, cameras, lights, materials, loaders, animation; renders with WebGL. Docs: https://threejs.org — repo: https://github.com/mrdoob/three.js

Install:
```
npm install three
```
Verified package `three` 0.187.0, MIT. No account needed. A GPU helps (WebGL), but it runs on CPU fallback; on this CPU-only VM prefer headless/offline rendering or keep scenes light.
ALREADY COVERED on this computer for agent-driven 3D: the 67.js engine (`~/workspace/67js-work`, `@codepawlhq/67js@0.19.0`) — use the `build-67js-scene` and `render-67js-media` skills for agent-built 3D scenes instead of hand-writing Three.js. Use raw Three.js when you need full control or a web deliverable.
When to use vs alternatives: pick Three.js for full-control 3D web work. For simpler 3D-in-HTML use LUME; for a game engine use Babylon.js or Godot; for offline film-quality 3D use Blender.
