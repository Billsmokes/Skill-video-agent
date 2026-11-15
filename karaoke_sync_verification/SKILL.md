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

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Alternatives for better results

- Keep the existing frame QA for visual problems and add this audio diff for timing problems, since a frame can look correct while the text runs ahead of the voice.

- Reuse the ground truth transcription for the next re-render check instead of trusting the timing file that just failed.
