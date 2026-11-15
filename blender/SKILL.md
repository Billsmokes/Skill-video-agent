---
name: blender
description: "Blender is the free 3D creation suite (modeling, rigging, animation, rendering); use it when building 3D scenes, character animation, or rendered video, or in any project that mentions Blender."
---

Blender (blender/blender, GPL-2.0-or-later, ~22.0k stars on the official mirror) — free and open-source 3D creation suite: modeling, rigging, animation, simulation, rendering (EEVEE/Cycles), video editing. Repo (official mirror): https://github.com/blender/blender — site: https://www.blender.org

ALREADY INSTALLED on this computer — do NOT reinstall:
- Working render path: `~/workspace/blender-4.0.2-linux-x64/blender` (Blender 4.0.2, EEVEE). Render at 540x960 then upscale to 1080x1920 with ffmpeg — see `~/AGENTS.md` for the exact settings.
- Also present: `~/workspace/blender-5.3/blender` (5.3.0) — its EEVEE is unusable on this CPU-only VM (13+ min/frame); do NOT use it for EEVEE renders.
- If EEVEE fails with `Couldn't open libEGL.so.1`, reinstall via apt: `libegl1 libgl1 libglx-mesa0 libegl-mesa0` (see `~/AGENTS.md`).
- Rig rule: parent parts to an UNSCALED empty as rig root; never parent a pivot/empty to a scaled mesh.

For other animation styles on this machine, see `blender-claymation` (claymation look) and the `video-studio` skill. Do NOT clone the Blender repo (huge); download releases from blender.org if a newer version is ever needed.
