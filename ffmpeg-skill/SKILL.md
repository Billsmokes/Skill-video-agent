---
name: ffmpeg-skill
description: "Local FFmpeg video editor for coding agents: 42 tools (probe, cut, fit, captions and karaoke, loudness, silence, delivery checks) with a probe, edit, check, verify workflow. Use when editing, trimming, measuring, or quality-checking a video or audio file with FFmpeg, or when delivering a finished file to a platform (Shorts, Reels, TikTok). No cloud, no API keys, Python standard library only."
---

# ffmpeg-skill

Local video editor for agents: 42 FFmpeg tools with a machine-readable contract.
Source: `kajisho5/ffmpeg-skill` (MIT, 7,023 stars), v3.10.1, main @ `f1c97cb`
(downloaded 2026-11-17 to `~/workspace/ffmpeg-skill-upstream/`).
Audit 17 Nov 2026: CLEAN — no network calls in scripts/ or mcp/ (zero urllib/
requests/socket/curl), no credential reads (only sets an HW-accel env var),
all ffmpeg via `subprocess.run([...list...])`, no shell, no eval/exec; `doctor()`
explicitly fetches nothing; installer only copies files + runs `ffmpeg -version`.
No dependencies (package.json: none). Python stdlib only.

## Layout (this install)

`SKILL.md` here; tools in `scripts/` NEXT TO it (upstream plugin layout puts them
two levels up — paths below are corrected for this layout). References in
`references/` (workflow, gotchas, flags, requests, timestamps, report, scripts,
devices, ci-platform-pitfalls). Contract: `docs/contract.md`.
Run: `python3 ~/workspace/custom-skills/ffmpeg-skill/scripts/<name>.py`
(`<skill-dir>` below = `~/workspace/custom-skills/ffmpeg-skill`).

## VM quirks

- ffmpeg **8.1.2**, ffprobe 8.1.2, python3 **3.12.3** — all on PATH.
- `doctor` (2026-11-17): **86 capabilities available, 0 required missing**.
  2 optional missing: `external:parakeet`, `external:whisper` — the ASR engines
  are NOT importable from system python3. faster-whisper lives in
  `~/workspace/payeng-venv` — use `~/workspace/payeng-venv/bin/python` for
  `--transcribe`, or keep using our own faster-whisper timings (karaoke stays
  in Remotion; the skill's caption karaoke is an alternative path, not the default).
- ASR is opt-in only: without an engine installed the tools REFUSE with install
  lines; no model is ever auto-downloaded by the skill's own code.
- MCP server exists (`python3 mcp/server.py`, stdio, stdlib only) but is NOT
  wired into our framework — the CLI tools are the interface. Do not `npx install`.

## Workflow (probe → plan → run → check → verify → look)

0. On failure only: `python3 scripts/_contract.py doctor`.
1. **Probe** what you plan from: `scripts/probe.py in.mp4 --json` (real duration,
   fps, resolution, codecs — never assume).
2. **Prefer lossless:** no re-encode when a cut/remux/audio-only change suffices.
3. **Plan** with `--dry-run --json`, then run. Trust `--json` numbers.
4. **Chain:** colour → cut → join → silence → fit → caption/overlay → sync →
   audio → loudness → export. 3+ steps: a `render.py` project.
5. **Check:** `scripts/check.py OUT --platform shorts|reels|youtube` — fix FAILs,
   mention WARNs. (See LOUDNESS RULE below for the loudness row.)
6. **Verify** from the writer's `--json` probe or `probe.py`: exit 0 + the
   requested numbers, or it failed.
7. **Keep originals:** never overwrite a source; `--overwrite` only for this
   job's files.
8. **Look** after any picture change: `scripts/look.py OUT --tiles 3x2`, view
   the PNG, name it. This replaces hand-rolled `ffmpeg -vf fps=...` frame grabs.

## ⚠️ LOUDNESS RULE — READ BEFORE USING loudness.py / check.py

The skill's standard is **−14 LUFS / −1 dBTP** (YouTube/Spotify). **That is NOT
our target. Do NOT normalize to −14 LUFS** unless the user explicitly orders it.
Our pipeline target is **mean ≈ −26.8 dB / peak ≈ −5.2 dB** (ffmpeg volumedetect
dBFS, set empirically across Ep 17–19 + reels). Measured 17 Nov 2026 on our
finished file `reels-listrik-mati-hujan.mp4` (mean −26.2 / peak −6.0):
**−24.0 LUFS integrated, TP −6.0 dBTP, LRA 2.7 LU** — i.e. our files sit ~10 LU
below the skill's −14 standard, and `check.py --platform shorts` FAILs only the
loudness row (11/12 pass) because of it. The skill itself notes the platform
auto-normalises anyway. Recommendation: keep our target; use `loudness.py
--measure-only` as a MEASUREMENT tool, and run `check.py --platform shorts
--lufs -24` when you want the check to judge against OUR target. If loudness
ever becomes a retention question, A/B-test −14 on ONE episode — never switch
the whole pipeline on the skill's default alone. Related standing rule: ffmpeg
`amix` defaults `normalize=1` (cuts ~6 dB) — always `normalize=0` in our mixes.

## What it updates in OUR workflow (adopt) vs what stays (keep)

ADOPT:
- **probe-first discipline** — `probe.py` before any edit (we used to assume).
- **`check.py` delivery check** — 12-row PASS/WARN/FAIL per platform (duration,
  aspect, fps, codec, colour, size, audio, loudness, true peak). Run on every
  finished Shorts/long-form file before delivery.
- **`look.py` contact sheet** — one 3x2 PNG with timestamps replaces manual
  frame extraction for QA (verified 17 Nov on the Listrik reel).
- **`loudness.py --measure-only`** — LUFS/TP/LRA measurement for the audio
  log (do not normalise with it by default — see rule above).
- **`silence.py`** — silence/jump-cut detection for future talking-head or
  Veo-clip trimming (our Remotion pipeline doesn't need it today).
- **`fit.py` / `cut.py --smart`** — reframing + frame-exact cuts with partial-GOP
  re-encode when a clip must be trimmed outside Remotion.
- **Platform safe zones** (from `_platforms.py`): reels top 8% / bottom 20% /
  left 5% / right 12%; shorts top 6% / bottom 18% — captions/titles must clear
  these (our karaoke band already sits low-centre; check against these numbers).
- **`sync.py` / xcorr** — drift correction as an alternative to our numpy xcorr
  check (xcorr stays the authoritative sync check for karaoke).

KEEP AS-IS (do not replace):
- **Remotion** renders all stickman / motion-graphics video (karaoke, scenes,
  titles). This skill is the ffmpeg FINISHING + QA layer, not the renderer.
- **Our loudness target** (−26.8 dB mean / −5.2 dB peak) — see rule above.
- **faster-whisper timings → KaraokeTrack** for karaoke (skill's caption
  karaoke is an alternative, not a replacement).
- **xcorr of clean VO vs rendered audio** as the authoritative sync check.

## Request → script (most-used)

| Need | Command |
|---|---|
| What's in this file? | `probe.py in.mp4` (`--analyze`, `--field duration`) |
| Is this OK to upload? | `check.py final.mp4 --platform shorts` (add `--lufs -24` for our target) |
| Show me what it looks like | `look.py out.mp4 --tiles 3x2` → view the PNG |
| Measure loudness only | `loudness.py in.mp4 --measure-only --json` |
| Cut / trim | `cut.py in.mp4 --start 1:20 --end 2:05` (`--smart` = frame-exact) |
| Make it 9:16 / 60 s | `fit.py in.mp4 --aspect 9:16 --fit pad` / `--duration 60` |
| Remove pauses | `silence.py in.mp4` |
| Burn captions from SRT | `caption.py in.mp4 --srt subs.srt` (`--animate pop --karaoke` with `--transcribe`) |
| Watermark / logo | `overlay.py in.mp4 --image logo.png --position top-right` |
| Export for a platform | `export.py in.mp4 --preset youtube|reels|shorts` |
| Whole edit as a project | `render.py --template reels|shorts|tiktok in.mp4 --cues cues.txt` |
| Batch a folder | `batch.py FOLDER --recipe batch.json --jobs auto` |
| Machine-readable contract | `python3 scripts/_contract.py --json` |
| Capability check | `python3 scripts/_contract.py doctor` |

Full routing table + variants: upstream `SKILL.md` in
`~/workspace/ffmpeg-skill-upstream/ffmpeg-skill-main/skills/ffmpeg-skill/SKILL.md`;
per-tool flags: `references/scripts.md` or `--help`. Never run raw `ffmpeg`/
`ffprobe` when one of the 42 scripts exposes the feature — say so and name the
closest option instead.
