---
name: manim
description: "Manim is a Python framework for precise mathematical and explanatory animation; use it when animating formulas, graphs, geometric constructions, or math-explainer video, or in any project that mentions Manim."
---

Manim (ManimCommunity/manim, MIT, ~42.5k stars) — community Python framework for creating precise mathematical animations (the engine behind 3Blue1Brown-style videos). Animates LaTeX formulas, graphs, geometry, and diagrams frame-perfectly, renders to MP4/GIF. Docs: https://docs.manim.community — repo: https://github.com/ManimCommunity/manim

Install:
```
pip install manim
```
Verified on PyPI: manim 0.22.1. Also needs ffmpeg on PATH; LaTeX is needed only for tex/text rendering. CPU rendering works; no account or GPU needed (GPU does not speed up Manim much).
When to use vs alternatives: pick Manim for math/formula explainer animation where precision matters. For code-driven 2D explainers with an editor use Motion Canvas; for web UI animation use Anime.js or Motion; for video rendering from React use Remotion.
