---
name: motion-canvas
description: "Motion Canvas is a TypeScript framework for programmatic 2D animation (explainers, code-driven video); use it when a video or animation should be built from code with a visual editor preview, for diagram/explainer-style animation, or in any project that depends on @motion-canvas packages or mentions Motion Canvas."
---

Motion Canvas (motion-canvas/motion-canvas, MIT, ~19.9k stars) — TypeScript framework for programmatic 2D animation. Write scenes in code, preview them in a visual editor, export to video. Best for explainer/diagram-style animation (the "3Blue1Brown-style, but code-first" lane). Docs: https://motioncanvas.io — repo: https://github.com/motion-canvas/motion-canvas

Install (per project, NOT global):
```
npm init @motion-canvas@latest
```
Core package: `@motion-canvas/core` (verified 3.17.2, MIT). Requires Node.js. No account or GPU needed.
When to use vs alternatives: pick Motion Canvas for code-driven explainers with an editor preview. For React video rendering use Remotion (`remotion-create` skill); for timeline/easing-only work use Anime.js, Motion, or Shifty; for math-formula animation use Manim.
