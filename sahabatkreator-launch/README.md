# Sahabat Kreator — Product Launch Video

Product launch video for [Sahabat Kreator](https://sahabatkreator.com) — SaaS manajemen
social media untuk kreator Indonesia. Dibuat sebagai [HyperFrames](https://hyperframes.heygen.com)
composition: HTML murni, tanpa React, tanpa build step.

## Spec

| Prop     | Value                              |
| -------- | ---------------------------------- |
| Durasi   | 30 detik                           |
| Resolusi | 1920×1080 (16:9)                   |
| FPS      | 30                                 |
| Adegan   | 5 (hero → problem → fitur → platform → CTA) |

## Struktur

Satu composition parent + 5 sub-composition (satu file per adegan):

```
index.html                        ← parent: mount 5 scene, atur timing global
compositions/scene-hero.html      ← 0–6s   (dark)
compositions/scene-problem.html   ← 6–13s  (light, chaos → solution)
compositions/scene-features.html  ← 13–21s (dark, 6 kartu fitur)
compositions/scene-platforms.html ← 21–26s (light, 8 platform badge)
compositions/scene-cta.html       ← 26–30s (dark, CTA)
```

Parent (`index.html`) tinggal host kosong per adegan:

```html
<div class="scene"
     data-composition-id="scene-hero"
     data-composition-src="compositions/scene-hero.html"
     data-start="0" data-duration="6" data-track-index="0"></div>
```

Setiap sub-composition punya `data-composition-id` sendiri, scoped `<style>` (selector
`#scene-*` jadi tidak bocor antar scene), dan **timeline lokal yang dimulai dari 0** —
waktu di file scene selalu relatif terhadap awal adegan itu, bukan waktu global.

Kontrak yang renderer seek per frame: setiap scene mendaftarkan satu paused GSAP
timeline ke `window.__timelines["<composition-id>"]`.

Brand token diambil dari design system asli (`apps/web/src/index.css` di `sahabatkreator-dev`):

- Brand navy `#0C1627`
- Primary blue `#08A5FC`
- Secondary orange `#FD9501`
- Warm background `#faf8f6`

## Render lokal

Butuh Node.js 22+ dan FFmpeg.

```bash
npx hyperframes browser ensure        # sekali, download chrome-headless-shell
npx hyperframes lint                  # static check, no browser
npx hyperframes check                 # browser gate: runtime, layout, WCAG contrast
npx hyperframes render --output renders/launch.mp4
```

Output: `renders/launch.mp4`.

Status terakhir: lint 0 errors / 0 warnings, check 0 errors / 0 warnings, contrast 30/30
pass WCAG AA.

## Render via CI

Workflow ada di `.github/workflows/render-launch-video.yml` (folder ini di-push ke GitHub
sebagai repo sendiri). Trigger:

- **Push ke `main`** yang menyentuh `sahabatkreator-launch/**` → render 1080p otomatis
- **`workflow_dispatch`** → render manual, bisa pilih `1080p` atau `4k`**

MP4 jadi artifact GitHub Actions (retensi 30 hari), tinggal download. Untuk repo public,
GitHub Actions gratis tanpa batas menit.

## Ganti rasio output

Asalnya 16:9 untuk YouTube/embed. Mau TikTok / Reels (9:16)? Lima tempat:

1. Parent `index.html`: `data-width="1920" data-height="1080"` → `1080` × `1920`, plus
   `html, body { width/height }`
2. Setiap `compositions/scene-*.html`: `data-width` / `data-height` + selector `#scene-*`
   di scoped style
3. Sesuaikan `font-size` headline (84px terlalu besar untuk portrait) dan padding grid fitur

`hyperframes render --aspect-ratio 9:16` hanya mendeteksi dari komposisi; ukuran fisik tetap
harus diubah di HTML.

## Edit copy

Semua teks ada langsung di HTML — tidak ada file terpisah. Cari string seperti
"Mulai Gratis Sekarang" atau "sahabatkreator.com" dan ganti. Timing animasi ada di blok
`<script>` di masing-masing file scene, waktu lokal per adegan.

## Ganti urutan / durasi adegan

Urutan dan durasi hanya diatur di parent `index.html` — `data-start` dan `data-duration`
per host `<div>`. Isi scene tidak perlu disentuh. Tapi ingat: timeline di dalam scene
pakai waktu lokal, jadi kalau adegan jadi 8 detik, animasi dalam scene mungkin perlu
disesuaikan.
