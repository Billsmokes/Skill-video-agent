---
name: build-67js-scene
description: Build, light, film and refine 3D scenes through the 67js MCP server (npm package @codepawlhq/67js). Use when an agent has the scene_* tools and must model parts, set materials, lights, HDRI, color looks, fog, lens and depth of field, animate a camera, lights, sun, sky and effects, bake light, fix z-fighting or recover from a tool error. Also for "làm cảnh 67js", "dựng cảnh 3D bằng 67js", "model bị răng cưa", "ánh sáng cảnh 67js", "đổ bóng", "sương mù 67js", "camera 67js". Image and video export is the render-67js-media skill.
license: MIT
---

# Build scenes with 67js over MCP

> **Installed 2026-11-13 (audit: CLEAN) — 67js 0.19.0, pinned.**
> npm: `@codepawlhq/67js@0.19.0`, integrity `sha512-wXZ/i3kW+hROsVyPCsVhbXa8+UDey2t7IA/Z3eIclVrilhh311aoGwLHQLFSZTjDVWdOrZElD09YWkWTBjig8Q==`, installed at `~/workspace/67js-work` (`package.json` + lockfile pin the exact version; never use `@latest` unpinned).
> **This VM: CPU-only, no GPU — always use `--backend cpu`.** Browser: the Muse Chromium at `/opt/meta-chromium/chrome` does NOT work here (it refuses to start: "Hatch WebGL domain policy is required") — use the wrapper `~/workspace/67js-work/chrome-nosandbox.sh` (Playwright Chromium 156 + `--no-sandbox`, needed because this VM runs as root) via `--browser ~/workspace/67js-work/chrome-nosandbox.sh`. ffmpeg is on PATH (`/usr/bin/ffmpeg`) so MP4 export works. Set `SIXTYSEVENJS_NO_UPDATE_CHECK=1` to disable the once-daily npm version check. Never use `--addons`/`--allow-addon` (loads arbitrary addon packages — by design, opt-in only, not needed here). Upstream repo (audit source): `~/workspace/67js` (tarball extract of github.com/codepawl/67.js, main @ 2026-11-13).
> `${CLAUDE_SKILL_DIR}` refs were replaced with relative `references/` paths for this harness.

You drive 67js through the `scene_*` tools of the `67js-mcp` server. No checkout is needed.
Start the server with `npx --package @codepawlhq/67js@0.19.0 67js-mcp --backend cpu --browser ~/workspace/67js-work/chrome-nosandbox.sh --assets <abs dir> --jobs <abs dir>`
(`--assets` for GLB, HDR and cube files, `--jobs` for render jobs, `--backend cpu` on this GPU-less VM).

## Loop

1. `scene_capabilities`: with no arguments it returns a short index of features, limits and topic names. Pass
   `topic` for the field list of one feature (`environment`, `materialFields`, `geometries`, `meshEdit.bevel`,
   `motion`) or `examples.scene_apply` for a complete call of a hard tool. Tool schemas list only common fields, so read the topic before a feature you have not used.
2. `scene_create` with a `recipe`: `{ "format": "67js.scene", "schemaVersion": 1, "geometries": [], "materials": [], "nodes": [] }`
   plus optional `camera`, `environment`, `effects`, `shadow`, `motion`, `bake`, `timeline`.
3. `scene_inspect`: node IDs, bounds, `diagnostics`, current `revision`.
4. `scene_look`: the PNG. Add `labels: true` to match node names to the image. Judge the image, not the lint.
5. `scene_apply` with the current `expectedRevision` and a small `operations` batch. A stale revision fails whole.
6. `scene_checkpoint` before a risky batch, `scene_restore` to undo, `scene_release_checkpoint` after. Without a checkpoint, `scene_restore` with `revision` goes back to one of the last 4 earlier revisions.
7. `scene_export_recipe` keeps the result. `scene_dispose` a scene you finished with.

`sessionId` may be left out: the last used scene is meant. With two scenes open, pass it on every call. The server keeps 4 scenes and closes the least recently used (`evictedSessions` in the create result), and 16 checkpoints per scene, oldest first (`evictedCheckpointIds`).
Variants: `scene_apply` with a `branch` name, compare in one `scene_review` (a `branch` per shot), keep one with `scene_restore`.

## Pick the recipe

| Task | Read |
|---|---|
| Sky, HDRI, `.cube` look, tone mapping, fog, exposure, bloom, AO, outlines | `references/look.md` |
| Sun, point, spot and area lights, caps, baked bounce light, flush faces | `references/light.md` |
| Lens, focal length, depth of field, shift, orthographic | `references/camera.md` |
| Motion tracks (camera, nodes, lights, sun, sky, effects), shots, timeline | `references/film-and-animation.md` |
| A call failed, a diagnostic appeared, the picture is wrong | `references/errors-and-recovery.md` |
| A playable game: animate a placed, retargeted character (idle, walk, run), web export | `references/game.md` |
| Polygon modeling: extrude, inset, bevel, booleans, mirror, array | `references/mesh-modeling.md` |
| Scene code against the npm library, not MCP | `references/scene-contract.md` |

## Defaults that save work

- Start from the default look: `environment.setup` `daylight`, AO off. `studio` suits products, `overcast` soft light,
  `sunset` warm low light. Do not hand-tune key, fill and ambient first.
- The preview draws a floor with a contact shadow under the lowest geometry. `environment.ground: false` removes it,
  so never add a floor node only for the shadow.
- Sizes: `box` is a unit cube; `cylinder` and `sphere` have radius 1, so node scale x/z (cylinder) or scale (sphere) is the
  radius, not the diameter. A 6-segment cylinder has flats on Z (1.732 x scale apart) and corners on X.
- Stack parts from numbers: write each part's bottom and top Y first, adding the Y of the surface it stands on (a height "above
  the slab" is measured from the slab's top, not from Y = 0). After building, compare `scene_inspect` bounds with the task's numbers.
- Reuse one geometry ID for repeated parts and move them with node transforms.
- Put parts against each other with node `relations` (`attach`, `gap`, `flush`, `align`, `centered`) so one `set-node`
  resizes the body and the handle follows.
- Surfaces without UVs: material `pattern` (wood, bricks, marble, metal, fabric). Glass: `transmission` with a nonzero
  `thickness`. Details are in `scene_capabilities`.
- Hide a node with `visible: false`; `castShadow`, `receiveShadow`, `holdout` and `shadowCatcher` are node flags too
  (the last two need a transparent canvas, so they matter for exported composites, not `scene_look`).
- Characters and creatures: recipe `armatures` plus `skin` per mesh node, `motion.ik`, imported GLBs through the
  `asset` node field and `scene_asset_catalog`. Physics, cloth, particles and hair are recipe sections.
  `scene_inspect` reports their results; read those instead of guessing from a picture.
- Third-party controller or camera kinds exist only when the server owner started it with `--addons` and `--allow-addon`:
  read `scene_capabilities` topic `addons` for the names, then set `game.player.controller` or `game.camera` to one.
  You cannot load an addon from a tool.
- A GLB for Blender: `scene_export_glb` (needs `--jobs`), it returns a path.

## Finish

Deliver the recipe, one `scene_look` image and any limit you hit. Say what you measured (counts, bounds, seconds)
apart from what you judged by eye. Do not claim Blender parity or general speed over three.js.
Changes, including unreleased and breaking ones: `https://67js.codepawl.com/changelog.json`.
A 67js bug: `gh issue create -R codepawl/67.js` with the steps, the recipe and a still; no local paths.
