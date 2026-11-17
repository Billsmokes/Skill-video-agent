---
name: shorts-upload-pack
description: "Produce one saved upload pack per short video: a canonical copy file (title, description, tags, pin comment, caption) the user can paste, plus a manual upload checklist. Use when a Short, Reel, or TikTok is finished and ready to upload."
---

# Shorts Upload Pack

## Save one canonical copy file

- Create one copy file per episode beside the finished video before you send anything.

- Name the file with the episode number and the word copy so you can find it again later.

- Put the title, the description, the pin comment, the call to action, the hashtags, the scheduled slot with its timezone, the thumbnail note, the related video note, and the download link expiry time in that file.

- Keep hashtags out of the title.

- Record the scheduled time in Asia/Jakarta and the link expiry time in the same file.

- Read the file back after you write it and use only its text when you answer later questions about the copy.

## Deliver copy the user can paste

- Send each piece of copy as its own separate message with nothing else in it.

- Send the title alone first, then the description alone, then the pin comment alone.

- Put explanations, links, and questions in separate messages outside the copy.

- Answer a later question about the copy by quoting the saved file instead of rewriting it from memory.

- Tell the user the file path and the link expiry time outside the copy when you deliver the links.

## Check the manual upload

- Check the scheduled time in YouTube Studio against the saved file before you confirm it.

- Confirm the category, the language, and the audience setting recorded in the copy file.

- Confirm visibility stays Private until the schedule is set and then changes to Schedule.

- Confirm the thumbnail, the related video choice, and the title spelling before you confirm the upload is ready.

- Warn the user when a download link expires less than one hour after the scheduled slot and ask for an earlier download.

- Stop and ask the user when a Studio field does not match the saved file instead of guessing the intended value.

## Upload the thumbnail and pin the comment in Studio

- Use a live browser task in YouTube Studio to upload the cover and do not use the YouTube thumbnail API for this channel.

- The API returned 404 on repeated tries for LF3 and LF4 and Ep 19 and Transparansi while the Studio upload worked.

- Upload a cover in Studio only when the video has no custom cover yet and record that it is a new cover in the copy file.

- Replacing a cover that already exists fails in the cloud browser because the Ubah button does not respond so leave a replacement to the user or to another browser and do not retry it in the cloud browser.

- The Studio mobile app cannot upload a custom Shorts thumbnail so use Studio on a computer or Chrome in desktop site mode and note that the YouTube app only lets the user pick a frame from the video.

- The YouTube API has no method to pin a comment so never promise an automatic pin and pin the comment by hand in Studio after the video airs.

- Deliver the pin text through the call to action reminder at airtime so the user can pin it in about ten seconds and record in the copy file that the pin is manual.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Websites

- [YouTube Studio](https://studio.youtube.com/) for uploading and scheduling a Short and for checking its title, description, audience, thumbnail, and scheduled time. Carry the exact copy from the saved file into the upload task and verify the scheduled time shows the intended time in Asia/Jakarta.

## Alternatives for better results

- Ask the user for the scheduled slot and the thumbnail choice before you render when either is still open, so the copy file is complete at delivery.

- Reuse the previous episode copy file as the template for the next episode and change only the episode details, so the format stays stable across the series.
