# Skill-video-agent

Koleksi skill untuk membuat video dengan AI agent — dari video pendek (Shorts/Reels/TikTok), animasi stickman, claymation, sampai suara & TTS. Tinggal salin folder skill yang dibutuhkan ke direktori skill agent kamu.

A collection of skills for making videos with an AI agent — short videos (Shorts/Reels/TikTok), stickman animation, claymation, sound & TTS. Just copy the skill folder you need into your agent's skills directory.

## Skills

| Skill | Untuk apa / What it does | Author / Source | License |
|---|---|---|---|
| `video-studio` | Skill induk video pendek 9:16 (Shorts/Reels/TikTok): Blender, AI video, Remotion, FFmpeg, TTS | Billsmoke (original) | MIT |
| `video` | Generate video umum: Veo, Sora, Runway, Kling, Remotion, Hyperframes | [coreyhaines31/marketingskills](https://github.com/coreyhaines31/marketingskills) | MIT |
| `video-shotcraft` | Video produk sinematik dari halaman web (Remotion, template Ink Press, shot cards) | [Vincentwei1021/video-shotcraft](https://github.com/Vincentwei1021/video-shotcraft) | Apache 2.0 |
| `stickman-video-director` | Video stickman 11 gaya (storytelling, explainer, dll.) | [kaomei/stickman-video-director](https://github.com/kaomei/stickman-video-director) | MIT |
| `kinetic-reel` | Reel kinetic typography (teks bergerak, motion graphics) | Derived from [John Heibel/ClaudeAnimationBase](https://github.com/JohnHeibel/ClaudeAnimationBase) (MIT) | MIT |
| `motion-design` | Prinsip motion design: timing, easing, koreografi, animasi UI | LottieFiles | MIT |
| `painted-animation` | Animasi kartun cat air & tinta (p5.js + p5.brush) | Adapted from [John Heibel/PDoomVideo](https://github.com/JohnHeibel/PDoomVideo) & [ClaudeAnimationBase](https://github.com/JohnHeibel/ClaudeAnimationBase) (MIT) | MIT |
| `blender-claymation` | Video claymation / stop-motion di Blender | [angrypenguinpng/blender-claymation-skill](https://github.com/angrypenguinpng/blender-claymation-skill) | MIT |
| `shorts_upload_pack` | Paket upload per Short: judul, deskripsi, pin comment, checklist manual | Billsmoke (original) | MIT |
| `image` | Generate/edit gambar: cover, thumbnail, mockup, aset visual | [coreyhaines31/marketingskills](https://github.com/coreyhaines31/marketingskills) | MIT |
| `earshot` | Menilai/memilih/motong/memverifikasi suara: SFX, musik, ambience (CLI + MCP, CPU) | [Leonxlnx/earshot](https://github.com/Leonxlnx/earshot) | MIT |
| `gemini-api-dev` | Coding dengan Gemini API — termasuk speech generation (TTS), voice design & replication, transkripsi, dan video generation | Billsmoke (original) | MIT |
| `remotion-captions` | ❌ Tidak disertakan (tanpa lisensi upstream) — install dari sumber resmi: `npx skills add remotion-dev/skills` | Remotion — [remotion-dev/skills](https://github.com/remotion-dev/skills) | ⚠️ See license note below |

## Audio & bahasa / Audio & language

- **Audio:** `earshot` (nilai & pilih SFX/musik/ambience), `gemini-api-dev` (TTS, voice design, transkripsi), `video-studio` (alur audio + subtitle).
- **Caption/subtitle:** `video-studio` (skill `remotion-captions` tidak disertakan — lihat catatan lisensi di bawah).
- **Voice, podcast & transcription** adalah kemampuan bawaan Muse (built-in), bukan skill di repo ini — jadi tidak disertakan. / Voice, podcast & transcription are built-in Muse capabilities, not skills — so they are not included here.
- **Tidak ada skill terjemahan khusus** di koleksi ini. / There is no dedicated translation skill in this collection.

## Cara install / How to install

Salin folder skill yang kamu butuhkan ke direktori skill agent kamu, contoh:

Copy the skill folder you need into your agent's skills directory, for example:

```bash
# Claude Code
cp -r video-studio ~/.claude/skills/

# Atau / or any agent using .agents/skills
cp -r video-studio .agents/skills/
```

Lalu panggil skill-nya dari agent seperti biasa (atau biarkan agent memilih sendiri berdasarkan deskripsi skill). / Then invoke the skill from your agent as usual (or let the agent pick it by its description).

## Catatan / Notes

- **Lisensi per skill tetap milik pembuat aslinya** (lihat tabel di atas dan file LICENSE di dalam tiap folder skill). Lisensi MIT repo ini hanya mencakup kurasi, README, dan skill original (`video-studio`, `shorts_upload_pack`, `gemini-api-dev`). / Each skill keeps its original author's license (see the table above and the LICENSE file inside each skill folder). This repo's MIT license covers only the curation, README, and the original skills.
- **Video demo besar dihapus dari `stickman-video-director/assets/readme/`** (21 file .mp4, ~200 MB) supaya repo tetap ringan — file .mp4 baseline yang dipakai skill untuk verifikasi gaya tetap disertakan. Untuk melihat demo lengkap, kunjungi repo aslinya di [kaomei/stickman-video-director](https://github.com/kaomei/stickman-video-director). / Large demo videos were removed from `stickman-video-director/assets/readme/` (21 .mp4 files, ~200 MB) to keep the repo light — the baseline .mp4 files the skill uses for style verification are still included. For full demos, see the upstream repo.
- **⚠️ `remotion-captions` — TIDAK disertakan:** repo upstream `remotion-dev/skills` tidak memiliki file LICENSE (GitHub default copyright: tidak ada izin redistribusi), jadi folder skill ini dihapus dari repo ini. Install langsung dari sumber resmi: `npx skills add remotion-dev/skills`. / The upstream `remotion-dev/skills` repo has no LICENSE file (GitHub default copyright: no redistribution right), so this skill is NOT included here. Install it from the official source: `npx skills add remotion-dev/skills`.
- **⚠️ `video-studio` & `gemini-api-dev` — asal tidak terdokumentasi:** kedua skill ini tidak memiliki file LICENSE atau catatan sumber di folder aslinya; keduanya ditulis/dikurasi untuk pemakaian pribadi dan dipublikasikan di sini di bawah MIT repo. Bila ternyata berasal dari pihak ketiga, beri atribusi yang benar atau hapus. / Both skills have no LICENSE file or source note in their original folders; they were written/curated for personal use and are published here under the repo MIT license. If either turns out to be third-party work, attribute it correctly or remove it.

## License

MIT © 2026 Billsmoke — see [LICENSE](LICENSE). Covers the curation, this README, and the original skills only. Third-party skills keep their own licenses (see table above).
