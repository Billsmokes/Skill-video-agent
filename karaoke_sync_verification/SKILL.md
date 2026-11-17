---
name: karaoke-sync-verification
description: "Verify karaoke subtitle sync in a rendered video: re-transcribe the rendered audio with Whisper, compare the word timings against the karaoke track, and confirm sync with cross-correlation of the clean voice-over against the rendered audio. Use after rendering any video with karaoke captions, before delivering it."
---

# Karaoke Sync Verification

## Re-transcribe the rendered audio

- Extract the audio from the final rendered video file and transcribe that audio with the Whisper tool already used for the project.

- Treat the new transcription as ground truth and keep its word timings in a file beside the video.

- Run this check after every re-voice, re-time, or re-render and before you deliver the video or upload it.

## Compare timings segment by segment

- Diff the karaoke timing file against the ground truth timings for every segment, not only for a few sample frames.

- Flag any word that appears more than 1 second before or after it is spoken.

- Report the worst drift per segment with its time range and the affected words, so the fix targets the right segment.

- Do not deliver, upload, or schedule the video while any segment still shows drift over 1 second.

## Fix the timing source

- Rebuild the karaoke timing file from the ground truth timings when the diff shows drift.

- Re-render the affected video and run the diff again from the new rendered audio.

- Inspect the alignment step that produced the drift when a rebuild still disagrees with the ground truth, and do not patch single words by hand to hide a shifted block.

## Cross-check long music-bedded audio with cross-correlation

- Use cross-correlation as the authoritative sync check for long-form audio that has a music bed.

- Keep the Whisper diff as the check for Shorts and for clean voice audio without a music bed.

- On LF5 the Whisper checker gave contradictory fail results on the same timeline while cross-correlation of the clean voice files against the rendered audio gave zero offset on all six scenes.

- When cross-correlation shows no offset and the Whisper diff still flags drift, trust cross-correlation and do not rebuild or re-render on the Whisper result alone.

- Re-run the Whisper check on a fresh transcription before treating its flag as real and record which check decided the verdict.

- Keep the one second drift limit for both checks and do not deliver while cross-correlation shows drift over one second.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Alternatives for better results

- Keep the existing frame QA for visual problems and add this audio diff for timing problems, since a frame can look correct while the text runs ahead of the voice.

- Reuse the ground truth transcription for the next re-render check instead of trusting the timing file that just failed.
