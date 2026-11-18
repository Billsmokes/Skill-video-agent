---
name: video-studio
description: "Skill induk untuk membuat video pendek (Shorts/Reels/TikTok, 9:16) memakai tool apa pun yang tersedia: Blender (3D), model AI video lewat konektor (mis. fal.ai, Veo, Runway, Kling), animasi berbasis kode (Remotion, HTML/Canvas, MoviePy, Manim), FFmpeg, TTS, Whisper, dan generator gambar. Gunakan skill ini SETIAP KALI pengguna meminta video, animasi, klip, reel, iklan pendek, konten Shorts, voice-over, atau subtitle, meskipun tidak menyebut tool tertentu. Skill ini memeriksa tool yang tersedia, memilih jalur terbaik, merencanakan, membuat, memeriksa kualitas sendiri, dan melarang hasil bergaya low-poly atau kasar."
---

# Video Studio

Satu skill untuk seluruh alur pembuatan video, apa pun tool yang dimiliki agent.
Alur: **cek tool -> pilih jalur -> rencana -> buat -> rakit -> QA -> serahkan.**

## Aturan emas
1. Jangan mengklaim sudah merender atau membuat video jika belum benar-benar menghasilkan file.
   Jika tool tidak ada, katakan jujur dan beri alternatif (lihat Langkah 0).
2. Selalu lihat hasil sendiri (frame sampel) sebelum menyerahkan.
3. Dilarang tampilan low-poly, flat shading, geometri berfaset, tepi bergerigi, warna polos
   tanpa material, atau latar abu polos, kecuali pengguna memang memintanya.
4. Minta persetujuan pengguna sebelum memakai layanan berbayar atau mengunggah data pribadi.
   Jangan menampilkan atau menyimpan API key di output.
5. Hormati hak cipta dan orang nyata: tidak memakai musik/gambar berhak cipta tanpa izin,
   tidak membuat video menyesatkan yang meniru wajah atau suara orang nyata.

## Karakter hirup (stickman/kartun 2D — wajib untuk video karakter)
Aturan ini dari repo `mromidev/claude-animated-shorts` (MIT, dicek 16 Nov 2026; laporan ukur
disimpan di `~/workspace/imagine_media/references/claude-animated-shorts/`). Karakter 2D
terasa hidup bukan karena dunia 3D, tapi karena sistem gerak yang diukur angkanya:
- **Jalan on-ones**: karakter yang bergerak berubah tiap frame (tanpa pose ditahan 2-3 frame).
  Langkah 9-12 frame; badan naik-turun (bob) 3-7% dari tinggi, terendah pas kaki menapak;
  condong (lean) 1-7° ke depan seirama bob; paha ayun ±35-45°; **kaki yang menapak TIDAK
  menggeser** (planted) + bayangan kontak lembut di bawah kaki.
- **Kedip tiap 0,5-1,5 detik** (3-4 frame); kedip menutupi momen ganti ekspresi.
- **Reaksi 0-3 frame SEBELUM kata** diucapkan; ganti ekspresi instan (1 frame).
- **Antisipasi + impact**: lompat = jongkok dulu 2-3 frame; pukulan/tujahan = tahan ~6 frame
  lalu dorong 1 frame; dampak (flash/ring) tepat di kata yang bersangkutan.
- **Idle = diam itu benar**: karakter yang menunggu cukup beku (jangan goyang-goyang tanpa
  alasan); hanya idle yang termotivasi (mis. kepala terkulai pelan 10-15°).
- **Kamera tidak pernah diam**: tiap hold merayap (push-in pelan 0,15-0,35%/frame); gerak
  glide smootherstep 18-40 frame; potongan hanya untuk lompatan besar; roll 4-5°; prop
  foreground di ~1/3 shot; DOF fokus ke mata di close-up.
- **TANAH (aturan user 18 Nov 2026, berlaku untuk Shorts + Reels)**: di scene ngobrol/berdiri,
  garis tanah/horizon harus TETAP DATAR dan DIAM — kamera creep hanya push-in (scale) atau
  glide lateral yang menjaga horizon level; DILARANG roll atau bob vertikal yang membuat
  tanah terlihat miring seperti tanjakan/turunan. Permukaan tanah BERVARIASI tiap scene
  sesuai tempatnya — batu bata, aspal, rumput, tanah — jangan tanah (dirt) melulu.
- **LANGIT (aturan user 18 Nov 2026, berlaku untuk Shorts + Reels)**: langit harus SESUAI
  waktu di scenenya — siang/sore/malam (warna, matahari/bulan, cahaya) — termasuk ada
  tidaknya burung di langit; jangan langit siang generik di scene malam, atau sebaliknya.
- **PROPERTI/DETAIL (aturan user 18 Nov 2026, berlaku untuk Shorts + Reels)**: tiap scene
  kudu aya komponen detail nu SESUAI UKURAN jeung karakterna — lampu jalan, korsi, meja,
  pager, tangkal, jsb. — supaya karasa dunya kartun beneran, lain latar kosong; skala
  properti kudu konsisten jeung tinggi karakter di scene nu sarua. Mun hiji properti teu
  bisa dijieun, paké nu PALING MIRIP (closest similar) — ulah ngosongkeun scenena.
- **FRAMING/SHOT (aturan user 18 Nov 2026, berlaku untuk Shorts + Reels)**: shot ulah
  deukeut teuing — jaga jarak framing nu pas supaya karakter jeung scenena kabaca jelas
  (medium/wide saluyu jeung kebutuhan scene, lain close-up nu motong konteks).
- **CLOSE-UP (aturan user 18 Nov 2026, berlaku untuk Shorts + Reels)**: close-up HANYA di
  momen berdampak — reaksi lucu, momen "sadar", ketegangan, detail penting, keputusan/ide,
  emosi cerita, hook pembuka — 1-2 detik tiap close-up, MAKS 1-3 close-up per video 30-45
  detik. Pola: wide → close-up → wide. Kepala 45-60% lebar frame, mata di sepertiga atas,
  latar polos/blur, SATU elemen pendukung (? / keringat / air mata / garis kejut). JANGAN
  close-up saat penonton perlu tahu lokasi/langkah (tutorial), saat banyak karakter
  berinteraksi, atau berturut-turut lebih dari 2x.
- **ULAH NGARUBAH NU GES ALUS (aturan user 18 Nov 2026, berlaku untuk semua kerja)**:
  saat memperbaiki/meningkatkan, ubah HANYA bagian yang cacat — yang sudah bagus jangan
  diutak-atik; jangan merusak yang sudah jalan demi perbaikan di tempat lain. KECUALI
  perubahannya jelas upgrade yang bikin makin alus — itu boleh.
- **Muka**: sistem ekspresi (kami punya 9 varian di TukangBase — kembangkan ke arah 14);
  konstruksi kepala "bean": taper + asimetris + miring, bukan bulet simetris; outline
  tebal-tipis (lebih tebal di sisi bayangan/bawah); muka harus terang (luma ≥0,85).
- **Yang DITOLAK dari repo itu**: dunia 3D three.js (render berat, ~4 fps di 2K), aturan
  "no captions" dan "English only" (kami: karaoke penuh + Bahasa Indonesia + CTA subscribe).
  Frame referensi repo hanya untuk studi gaya — jangan contoh karakter/cerita/shot-nya.

## Langkah 0: Cek kemampuan (lakukan sekali di awal)
Uji, jangan menebak. Catat hasilnya sebagai "inventaris tool".

| Kemampuan | Cara cek | Dipakai untuk |
|---|---|---|
| Terminal/sandbox | jalankan `echo ok` | semua jalur kode |
| Blender | `blender --version` (atau `python -c "import bpy"`) | video 3D |
| FFmpeg | `ffmpeg -version` dan `ffprobe -version` | merakit, konversi, QA |
| Python / Node | `python3 --version`, `node --version` | animasi kode |
| Konektor AI video/gambar/suara | lihat daftar tool/konektor yang aktif | klip AI, aset, voice-over |
| Vision (membaca gambar) | coba lihat satu frame | QA visual |
| Internet | coba akses satu URL | unduh library/aset |

Jika sebagian tidak ada, jangan berhenti. Pakai tabel fallback:

| Kondisi | Yang tetap bisa dilakukan |
|---|---|
| Tanpa terminal dan tanpa konektor video | Serahkan paket produksi: naskah, storyboard, prompt per shot (siap tempel ke Veo/Runway/Kling), file subtitle .srt, dan daftar aset |
| Konektor AI video saja | Jalur B (AI video), rakit manual lewat instruksi untuk pengguna |
| Terminal tanpa Blender | Jalur C (animasi kode) atau B; coba `pip install bpy` bila ada internet dan versi Python cocok |
| Tanpa vision | Verifikasi lewat metadata (ffprobe), histogram/kecerahan frame, dan minta pengguna memeriksa frame |

## Langkah 1: Pilih jalur

| Jenis video | Jalur utama | Alasan |
|---|---|---|
| Produk 3D, objek berputar, scene sinematik, abstrak 3D, penjelasan visual 3D | **A. Blender** | kontrol penuh, konsisten |
| Adegan realistis (orang, hewan, alam), b-roll sinematik | **B. AI video** | Blender sulit mencapai realisme foto |
| Teks kinetik, infografis, data, penjelasan edukasi 2D, kutipan, listicle | **C. Animasi kode** | tajam, cepat, presisi teks |
| Gabungan klip, potong, dubbing, subtitle, ubah format | **D. FFmpeg** | cepat dan deterministik |
| Video dengan narasi | tambah **E. Audio dan subtitle** | hampir semua Shorts butuh |

Satu video boleh menggabungkan jalur (mis. klip AI + teks kinetik kode + voice-over TTS + rakit FFmpeg).
Pilih jalur paling sederhana yang memenuhi kebutuhan. Jika pengguna menyebut tool, ikuti.

## Langkah 2: Rencana (wajib sebelum membuat)
Klarifikasi maksimal 3 pertanyaan hanya untuk hal yang belum jelas; sisanya nyatakan sebagai asumsi.
Buat:
- **Spesifikasi**: 1080x1920, 30 fps, durasi (default 15-45 dtk), bahasa, ada/tidak narasi, musik, teks.
- **Style bible** (satu paragraf): gaya visual, palet 2-3 warna + 1 aksen, font, mood cahaya, gaya kamera.
  Pakai ini di SEMUA scene agar konsisten. Jika ada video referensi, deskripsikan gayanya di sini.
- **Struktur**: Hook (0-2 dtk) -> 3-5 beat isi (3-8 dtk, satu ide per beat) -> penutup/CTA.
  Ganti visual atau sudut tiap 3-5 detik.
- **Shot list** (tabel): nomor, durasi, visual, teks di layar, narasi, jalur/tool.

Untuk video panjang atau mahal (banyak klip AI), tampilkan rencana ke pengguna dulu.

## Jalur A: Blender
Jalankan headless: `blender -b -P script.py`. Cek versi dulu karena API berbeda antar versi.

**Setup**
```python
import bpy
bpy.ops.wm.read_factory_settings(use_empty=True)
s = bpy.context.scene
s.render.resolution_x, s.render.resolution_y = 1080, 1920
s.render.fps = 30
s.frame_start, s.frame_end = 1, 30 * DURASI_DETIK
```

**Anti low-poly (wajib)**
- Subdivision Surface (viewport 2, render 3), shade smooth / smooth by angle, Bevel (3-4 segmen) di tepi keras
- Bola/silinder segmen >= 64
- Material Principled BSDF dengan roughness disengaja; variasikan dengan Noise/ColorRamp + bump kecil
  (logam: metallic 1, roughness 0.2-0.4; kaca: transmission 1, IOR 1.45; plastik: roughness 0.3-0.5 + coat)
- Lingkungan berdetail: gradien/sky World, lantai reflektif, kabut tipis, objek pendukung; bukan latar polos
- Tanpa aset eksternal: buat prosedural (modifier, Geometry Nodes, shader nodes), bungkus dalam fungsi

**Cahaya dan kamera**
- Tiga titik: key (area light besar), fill lemah, rim/back light. View Transform AgX (atau Filmic di versi lama)
- Kamera 35-85 mm, DoF aktif (f/1.8-4) dengan focus object, gerak lambat bermotif (push-in, orbit, dolly);
  parent kamera ke Empty dan animasikan Empty
- Subjek di tengah, sisakan safe zone atas/bawah ~12% untuk UI dan teks

**Animasi**
- Interpolasi BEZIER dengan easing; tambah anticipation, overshoot kecil, follow-through. Hindari LINEAR
- Aktifkan motion blur

**Render**
- Cycles (64-128 samples + denoiser OpenImageDenoise) untuk kualitas, EEVEE untuk cepat
  (nama engine: `BLENDER_EEVEE_NEXT` di 4.2+, `BLENDER_EEVEE` di versi lama)
- Render ke PNG sequence, lalu rakit dengan FFmpeg (lebih aman jika render terputus)
- Uji dulu: 1 still + preview resolusi 50% sebelum render penuh
- Compositor untuk finishing: Glare tipis, color balance, vignette, grain 3-6%

## Jalur B: AI video (lewat konektor)
Berlaku untuk Veo, Sora, Runway, Kling, Luma, Pika, Wan, dan layanan sejenis (mis. via fal.ai).
Cek model apa yang benar-benar tersedia di konektor; jangan mengarang nama model atau parameter.

**Formula prompt**: subjek + aksi + lingkungan + kamera + cahaya + gaya + kualitas.
> "Seekor rubah oranye berlari di hutan berkabut saat matahari terbit, tracking shot perlahan,
> cahaya emas lembut menembus pohon, detail bulu realistis, depth of field sempit, sinematik, 4K."

**Aturan**
- Negative prompt bila didukung: `low poly, blocky, pixelated, flat shading, cartoonish, low resolution, jittery, deformed, text artifacts`
- Klip AI biasanya pendek (sekitar 4-10 dtk). Rencanakan video sebagai beberapa klip lalu rakit dengan FFmpeg
- Konsistensi karakter/gaya: buat satu gambar acuan per tokoh/lokasi (generator gambar), lalu gunakan
  image-to-video; ulangi deskripsi style bible yang sama di setiap prompt
- Satu aksi dan satu gerak kamera per klip; prompt panjang dengan banyak aksi sering gagal
- Teks di dalam video AI sering rusak: tambahkan teks belakangan lewat Jalur C atau FFmpeg
- Generate 1 klip uji dulu (biaya rendah), cek, baru sisanya. Laporkan estimasi biaya/jumlah panggilan
  ke pengguna sebelum generate massal
- Orang nyata: jangan meniru wajah/suara tokoh nyata atau membuat konten menyesatkan

## Jalur C: Animasi berbasis kode
Pilih sesuai tool yang tersedia:
- **Remotion (React)**: bagus untuk teks kinetik dan infografis. `npx remotion render` -> MP4
- **HTML/CSS/Canvas + headless browser**: render frame demi frame dengan Puppeteer/Playwright, rakit dengan FFmpeg
- **MoviePy (Python)**: gabung klip, teks, gambar, dan audio sederhana
- **Manim**: animasi matematika dan sains
- **Pillow/matplotlib + FFmpeg**: infografis dan grafik data sederhana

**Aturan desain**
- Font tebal, ukuran besar (judul 90-140 px pada kanvas 1080 lebar), kontras tinggi, maksimal 6-8 kata per layar
- Muncul dan hilang dengan easing (ease-out masuk, ease-in keluar), bukan tiba-tiba
- Satu palet dan satu keluarga font; tidak lebih dari 2 font
- Safe zone: jauhkan elemen penting dari 12% atas dan 18% bawah (area UI platform)
- Latar punya kedalaman (gradien halus, bentuk bergerak pelan), bukan warna datar

## Jalur D: FFmpeg (perakit dan penyelamat)
```bash
# Frame PNG -> MP4
ffmpeg -framerate 30 -i frames/%04d.png -c:v libx264 -pix_fmt yuv420p -crf 18 out.mp4

# Crop ke 9:16 dan skala 1080x1920 (dari video lebar)
ffmpeg -i in.mp4 -vf "crop=ih*9/16:ih,scale=1080:1920" -c:a copy out.mp4

# Gabung klip (format sama)
printf "file 'a.mp4'\nfile 'b.mp4'\n" > list.txt
ffmpeg -f concat -safe 0 -i list.txt -c copy joined.mp4
# Jika format berbeda: re-encode seragam
ffmpeg -i a.mp4 -vf "scale=1080:1920,fps=30,format=yuv420p" -c:v libx264 -crf 18 -an a_norm.mp4

# Fade in/out video (durasi N detik: ganti N-0.5)
ffmpeg -i in.mp4 -vf "fade=t=in:st=0:d=0.5,fade=t=out:st=N-0.5:d=0.5" out.mp4

# Campur voice-over + musik (musik dikecilkan)
ffmpeg -i v.mp4 -i voice.wav -i music.mp3 \
  -filter_complex "[2:a]volume=0.25[m];[1:a][m]amix=inputs=2:duration=first[a]" \
  -map 0:v -map "[a]" -c:v copy -c:a aac -shortest out.mp4

# Bakar subtitle (butuh libass)
ffmpeg -i in.mp4 -vf "subtitles=subs.srt:force_style='FontSize=18,Outline=2,Alignment=2'" out.mp4

# Normalisasi loudness
ffmpeg -i in.mp4 -af loudnorm -c:v copy out.mp4

# Cek spesifikasi, ekstrak frame QA
ffprobe -v error -show_entries stream=width,height,r_frame_rate,duration -of default=nw=1 out.mp4
ffmpeg -ss 00:00:01 -i out.mp4 -frames:v 1 qa_01.png
```
Selalu `-pix_fmt yuv420p` dan H.264 agar kompatibel dengan platform.

## Jalur E: Audio, narasi, subtitle
- **Naskah narasi**: ~2,5 kata per detik (bahasa Indonesia ~2-2,5). Hitung agar pas dengan durasi
- **TTS**: pakai konektor/tool yang tersedia (mis. ElevenLabs, Piper, Kokoro, edge-tts). Pilih suara
  yang jelas, tempo wajar; jangan meniru suara orang nyata tanpa izin
- **Subtitle**: jika ada Whisper, transkripsi untuk timing per kalimat/kata -> buat `.srt`.
  Jika tidak, hitung timing dari naskah dan durasi audio. Pecah per frasa pendek (maks ~2 baris)
- **Musik/efek suara**: hanya file dari pengguna atau sumber bebas royalti/CC0 yang lisensinya jelas;
  sebutkan sumber. Musik latar volume 0,2-0,35 bila ada voice-over, fade in/out 0,5-1 dtk
- Tidak ada audio sama sekali? Ekspor tanpa audio dan beri tahu pengguna

## Langkah 3: Rakit dan finishing
1. Samakan semua klip: 1080x1920, 30 fps, yuv420p
2. Susun urutan sesuai shot list; potong di beat narasi/musik
3. Tambahkan teks, subtitle, audio; satu gaya transisi konsisten (cut tegas atau crossfade pendek)
4. Ekspor MP4 H.264 + AAC; target < 100 MB untuk 60 detik

## Langkah 4: QA mandiri (wajib, maksimal 3 putaran perbaikan)
Ekstrak minimal 3 frame (awal, tengah, akhir) plus 1 frame di setiap pergantian scene, lihat langsung:
- [ ] Tidak ada tampilan low-poly, faset, bergerigi, wajah/tangan cacat (untuk video AI), artefak aneh
- [ ] Pencahayaan jelas, tidak gosong atau terlalu gelap; latar tidak polos
- [ ] Gaya, palet, dan font konsisten antar scene
- [ ] Teks terbaca, tidak terpotong, di dalam safe zone
- [ ] Gerakan halus; tidak ada loncatan/objek menembus
- [ ] **Karakter hirup**: tidak ada frame beku ≥12 frame berturut-turut pada karakter yang
  seharusnya bergerak (cek selisih piksel antar frame); kedip terlihat; kaki menapak tidak
  menggeser; kamera merayap di setiap hold (bukan diam total)
- [ ] Audio sinkron, tidak clipping, volume musik tidak menutupi narasi
- [ ] Spesifikasi benar (ffprobe): 1080x1920, 30 fps, durasi sesuai, audio ada bila seharusnya
Perbaiki PENYEBAB (skrip/prompt), render ulang bagian bermasalah saja, cek lagi.
Jika setelah 3 putaran masih ada masalah, serahkan dengan laporan jujur tentang sisa masalahnya.

## Diagnosis cepat
| Gejala | Penyebab umum | Perbaikan |
|---|---|---|
| Low-poly / berfaset | tanpa subdivision/smooth | Subsurf + smooth + bevel |
| Tampak plastik/datar | material satu warna | variasi roughness + bump + ColorRamp |
| Video AI wajah/tangan cacat | prompt terlalu rumit | sederhanakan, jarak kamera lebih jauh, regenerate |
| Karakter berubah antar klip | tanpa acuan | gambar acuan + image-to-video + style bible sama |
| Teks AI rusak | model lemah teks | tambah teks lewat Jalur C/FFmpeg |
| Klip tidak bisa digabung | codec/fps beda | normalisasi dulu (Jalur D) |
| Render lama | samples/resolusi tinggi | turunkan samples, EEVEE, preview 50% |
| Gelap/pucat | cahaya lemah/View Transform | tambah key, cek AgX/exposure |
| Subtitle tidak sinkron | timing asumsi | transkripsi ulang dengan Whisper atau hitung ulang |
| Video tidak bisa diputar di platform | pixel format/codec | `-pix_fmt yuv420p`, H.264 + AAC |

## Langkah 5: Serah terima
Berikan: file MP4 akhir, file sumber (skrip/prompt/rencana) agar bisa diulang, subtitle `.srt` bila ada,
serta ringkasan 2-4 kalimat: apa yang dibuat, tool yang dipakai, asumsi, dan masalah tersisa (jika ada).
Sertakan juga judul, deskripsi, dan hashtag usulan bila pengguna menyebut akan mengunggah ke platform.
