---
name: motion-design
description: Motion design principles for video animation - Disney 12 principles, timing and easing tables, choreography and quality checks, mapped to Remotion springs, Blender keyframe easing and HyperFrames beats for the vertical stickman series.
license: MIT
---

# Motion Design — timing, easing and choreography for video

> **Installed 2026-11-17 (audit: CLEAN) — LottieFiles/motion-design-skill, commit `f9a8a041b85185ee4881b3471d3415e939aac772`, MIT, 3,248 stars.**
> Pure text skill (17 Markdown files, 92 KB, 1,628 lines). No code, no npm packages, no install scripts, no network calls, no credential reads — grep of the whole tree for `fetch`, `eval`, `exec`, `curl`, `token`, `password`, `http` returns nothing except the repo's own links. Nothing to pin, no integrity hashes apply.
> Upstream files copied verbatim to `references/upstream/` (SHA-256 of upstream `SKILL.md` `8729ecbc7e5736c1295baaa8ba0384bb4b1aac9cf91b83fcdc64d9548a57bb19`). This file is the adaptation to our stack; the upstream text is the depth reference.
> **Note:** a verbatim copy of this skill already sat in this folder since 9 Nov 2026 with no header and no stack mapping. Reorganised 17 Nov to house style (adapted `SKILL.md` on top, verbatim upstream under `references/upstream/`).
> **Repo:** https://github.com/LottieFiles/motion-design-skill

## Why this exists here

Our stickman workflow already has measured character rules from claude-animated-shorts (walk on-ones, blink every 0.5–1.5 s, react 0–3 f before the word, anticipation 2–3 f + impact 1 f, camera creeping every hold). Those rules cover **character acting**. This skill covers everything around the character: **how long a move takes, what easing it uses, how elements enter and leave, how a multi-element beat is staged**. Use it when a scene feels flat, floaty, jerky or samey even though the characters are drawn right.

## When to use it

- A text entrance, title card, karaoke word pop or transition feels wrong and you cannot say why — check the duration and easing tables first.
- Choreographing a multi-element beat (character enters + text pops + camera pushes in + object reacts) — use the 8-step checklist and the stagger rules.
- Choosing a Remotion spring or a Blender easing for a move instead of guessing.
- Reviewing a finished scene against the quality checklist (`references/upstream/reference/quality-checklist.md`).

## When NOT to use it

- Character acting itself (walk, blink, gesture timing) — the claude-animated-shorts measured rules in the video-studio skill win there. Where the two disagree, the measured rules win for characters: upstream anticipation is 100–200 ms (3–6 f) but stickman anticipation is 2–3 f; upstream squash impact is 2–4 f, which does match.
- UI work (buttons, modals, loading spinners) — the upstream examples are UI. Translate them, do not copy them: a modal entrance is a scene entrance, a button press is a character poke, a loading state is a waiting beat.

## The 8-step checklist (video version)

Before animating any beat, answer all eight:

1. **Emotional target?** — joy, calm, urgency, elegance. Drives easing, timing, amplitude.
2. **Motion personality?** — one archetype per episode, applied consistently (see below).
3. **Primary property?** — position, scale, rotation, opacity. One property = direct, two = polished, three or more = overwhelming.
4. **Duration?** — from the frame table below.
5. **Easing family?** — entrance decelerates, exit accelerates, on-screen smooths both ends.
6. **Hero element?** — one thing moves per beat; everything else is secondary or still.
7. **Secondary + ambient layers?** — shadow, hair, cloth, background drift. Flat animation = missing layers.
8. **1/3 rules?** — motion distance and simultaneous elements stay inside budget (stagger total under 500 ms / 15 f).

## Duration table — UI milliseconds translated to frames at 30 fps

Upstream durations are for UI micro-interactions. A video beat runs longer: treat the table as **ratios**, then scale to the beat. 1 f = 33 ms.

| Upstream element | ms | Frames at 30 fps | Video equivalent |
|---|---|---|---|
| Micro-feedback | 80–120 | 2–4 f | Karaoke word pop, blink settle |
| Button / toggle | 120–180 | 4–5 f | A poke, a tap, a small reaction |
| Icon transition | 150–250 | 5–8 f | Expression change, prop swap |
| Card enter / exit | 200–350 | 6–11 f | Character or text entrance |
| Modal / dialog | 300–400 | 9–12 f | Scene entrance, panel slide |
| Page transition | 400–600 | 12–18 f | Scene change, camera move |
| Dramatic reveal | 600–1200 | 18–36 f | Reveal, punchline beat, hero shot |
| Ambient | 2000–20000 | 60–600 f | Idle sway, background life, breathing |

**Distance scales duration:** 100 px = base, 200 px = 1.3x, 400 px = 1.6x. A character crossing the frame takes longer than one stepping forward.
**Enter longer than exit:** entrances run 30–50% longer than exits. What appears matters more than what leaves.
**Stagger budget:** micro cascade 20–40 ms (1 f), standard 50–100 ms (2–3 f), dramatic 100–200 ms (3–6 f). Total stagger must stay under 500 ms (15 f). Direction: top-to-bottom for lists, left-to-right for horizontal, centre-out for a hero, reverse for exits.

## Easing — the directional rules (never break these)

- **Entrance** = decelerate (fast start, gentle landing): ease-out family.
- **Exit** = accelerate (gentle start, fast departure): ease-in family.
- **On-screen travel** = smooth both ends: ease-in-out family.
- **Looping ambient** = sine ease-in-out, seamless.
- **NEVER linear for spatial movement.** Linear is only for rotation, progress bars and timers.

### Industry curves (cubic-bezier values transfer directly to Remotion)

| Curve | Bezier | Use |
|---|---|---|
| Material Design 3 standard | (0.2, 0, 0, 1) | Default on-screen move |
| MD3 Emphasized | (0.05, 0.7, 0.1, 1) | Entrances, attention |
| MD3 Accelerate | (0.3, 0, 1, 1) | Exits, dismissals |
| Apple HIG | (0.25, 0.1, 0.25, 1) | Standard, neutral |
| Snappy | (0.2, 0, 0, 1) | Fast, decisive |
| Gentle float | (0.4, 0, 0.2, 1) | Ambient, background |
| Bounce settle | (0.175, 0.885, 0.32, 1.275) | Playful overshoot |
| Elastic snap | (0.68, -0.55, 0.265, 1.55) | Dramatic overshoot |

### Remotion mapping

- `interpolate()` with `easing: Easing.bezier(0.2, 0, 0, 1)` for the MD3 standard curve; swap in any row above.
- `spring()` configs from the upstream spring table (mass 1): standard `{ stiffness: 300, damping: 20 }`, bouncy `{ stiffness: 200, damping: 12 }`, very bouncy `{ stiffness: 150, damping: 8 }`, gentle `{ stiffness: 120, damping: 22 }`, very stiff `{ stiffness: 400, damping: 28 }`.
- Overshoot budget: success 5–10%, feedback 2–5%, celebration 15–25%, error 0%, premium 0%.

### Blender mapping

- Graph editor, interpolation BEZIER with easing **EASE_OUT** for entrances, **EASE_IN** for exits, **EASE_IN_OUT** for on-screen travel.
- Overshoot: interpolation mode **BACK** (small overshoot), **BOUNCE** or **ELASTIC** (playful/dramatic). Keep overshoot inside the budget above.
- Material feel: rigid (metal, stone) 1.2x duration, 0% overshoot; elastic (rubber) 0.8x, 15–25% overshoot; fluid (water) 1.5x, 5%; paper 1.0x, 3–5%; gas/smoke 2.0x, 0%.

## Motion personality — pick ONE per episode

| Archetype | Duration | Easing | Overshoot | Fits |
|---|---|---|---|---|
| Playful | 150–300 ms | ease-out-back | 10–20% | Stickman comedy, Desa Seblak default |
| Premium | 350–600 ms | cubic-bezier(0.4, 0, 0.2, 1) | 0% | Milestone / Retro TV openings |
| Corporate | 200–400 ms | cubic-bezier(0.2, 0, 0, 1) | 0–3% | Explainer / tutorial long-form |
| Energetic | 100–250 ms | ease-out-expo | 15–30% | Punchline beats, celebrations |

**Desa Seblak default: Playful.** A milestone or Retro TV opening may switch to Premium for that episode only. Define three constants per episode and keep them: signature easing (one curve for 80% of moves), duration palette (quick / standard / slow), entrance pattern (one consistent entry style).

## Disney's 12 principles — condensed for video

Full version: `references/upstream/director/disney-principles.md`. The six that matter most for stickman:

1. **Squash and stretch** — squash scale ~[1.2, 0.8], stretch ~[0.85, 1.15]; impact 2–4 f, recovery 4–8 f. Preserve volume. Skip for Premium episodes.
2. **Anticipation** — small motion opposite to the main direction first, 10–20% of the main action. For characters use the tighter 2–3 f measured rule; for objects and text use 3–6 f.
3. **Staging** — dim non-hero elements to 40–60% opacity; hero enters 100–200 ms (3–6 f) after supporting elements; one primary action per beat.
4. **Follow through / overlapping action** — child parts trail the parent by 50–150 ms (2–5 f); trailing elements stop 100–200 ms (3–6 f) after the parent. Lower spring stiffness = more trailing.
5. **Slow in and slow out** — see the directional rules above. Never linear for spatial movement.
6. **Arcs** — add a 10–20 px perpendicular offset at a path midpoint (5 px for Corporate, 20 px+ for Playful). Straight paths read as mechanical — use them only on purpose.
7. **Secondary action** — amplitude 30–50% of primary, 50–100 ms (2–3 f) after primary, different easing. Example: character lands, then hair/cloth settles, then shadow grows.
8. **Timing** — heavy = 400–800 ms (12–24 f), light = 100–250 ms (3–8 f), sad/serious = 600 ms+ (18 f+), happy/light = 200–400 ms (6–12 f), urgent = 100–200 ms (3–6 f).
9. **Exaggeration** — Playful 15–25%, Energetic 20–30%, Corporate 0–5%, Premium 0%. Scale overshoot 10–30% beyond target, rotation ±5–15°.
10. **Appeal** — smooth curves over sharp angles, consistent personality. Appeal killers: jerky motion, inconsistent timing, abrupt stops, uniform animation (everything moving the same way at the same time).

## Common recipes (translated from UI)

| Recipe | Principles | Video form |
|---|---|---|
| Button press | Anticipation + squash + follow-through + secondary + timing | Character poke / tap: anticipate 2 f, squash 3 f, overshoot settle 6 f, shadow shrinks |
| Card entrance | Anticipation + arcs + slow in/out + follow-through + staging | Character/text entrance: rise from 20 px below on a slight arc, ease-out, shadow arrives 2 f late |
| Success celebration | Exaggeration + secondary + timing + squash + appeal | Punchline win: scale pop with ease-out-back, confetti/secondary burst, 9–12 f total |
| Error shake | Timing + slow in/out + staging, no exaggeration | Wrong-answer beat: position oscillates 2–3 times, ±10–15 px, 9–12 f |
| Loading / waiting | Timing + slow in/out + appeal | Waiting beat: slow ambient loop, sine easing, nothing pops |

## Three motion layers

Every beat needs all three or it reads flat: **primary** (the action the viewer follows), **secondary** (shadow, hair, cloth, icon shift), **ambient** (background drift, breathing, light). Check the render stills for missing layers before calling a scene done.

## Quality checklist (before delivery)

From `references/upstream/reference/quality-checklist.md` — the short version:

- Every move has an emotional target and a personality that matches the episode.
- No linear spatial movement; no move without easing.
- Entrances longer than exits; stagger total under 15 f.
- One hero per beat; secondary layers present; frozen frame ≥ 12 f is a defect (same bar as the stickman workflow).
- Overshoot inside budget; no exaggeration in Premium/Corporate episodes.
- If a move feels wrong, check in this order: duration, easing, then staging — in that order, not all at once.

## References — upstream files

All under `references/upstream/` (verbatim LottieFiles, MIT):

- `SKILL.md` — the full skill: 8-step checklist, three pillars, personality, property selection, duration/easing tables, patterns.
- `director/` — `core-philosophy.md`, `decision-framework.md`, `disney-principles.md`, `motion-personality.md`, `emotion-mapping.md`, `choreography.md`, `narrative-structure.md`, `context-adaptation.md`.
- `patterns/` — `entrance-exit.md`, `state-feedback.md`, `ambient-continuous.md`, `multi-element.md`.
- `reference/` — `timing-easing-tables.md`, `property-selection.md`, `quality-checklist.md`, `troubleshooting.md`.

## Limits — honest

- **Upstream is UI-first.** Every example is a button, card or modal. The translation to video beats in this file is ours, not theirs — the frame conversions assume 30 fps and the personality default (Playful) is our choice for the series, not a LottieFiles recommendation.
- **Small, quiet repo.** 4 commits since creation in March 2026, no releases, no changelog activity since. That is fine for a finished principles document (it does not need maintenance the way code does), but do not expect updates.
- **Principles, not a renderer.** This skill tells you what timing and easing to use; it writes no Remotion or Blender code itself. Pair it with the video-studio skill and the claude-animated-shorts rules.
- **Durations are starting points.** The tables are heuristics from UI practice, not measured video data. If a beat reads wrong at the table value, trust the render stills over the table.

## Smoke test, verified 2026-11-17

- Frontmatter parses (name, description, license) and the description contains no unquoted colon.
- Upstream tree verified byte-identical to the repo tarball at commit `f9a8a041` (SHA-256 of `SKILL.md` matches above).
- Frame-conversion proof: the duration table converts cleanly to 30 fps frames (e.g. card enter 200–350 ms = 6–11 f, dramatic reveal 600–1200 ms = 18–36 f) and the spring table maps to valid Remotion `spring()` configs (standard 300/20, bouncy 200/12, gentle 120/22). See the tables above.
