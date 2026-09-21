# Sahabat Kreator — Product Launch Video (Portrait 9:16)

Versi **portrait** dari video launch Sahabat Kreator — rasio 9:16 untuk TikTok, Instagram
Reels, dan YouTube Shorts. Isi dan durasi sama dengan versi landscape; hanya dimensi dan
tata letak yang beradaptasi.

Lihat `../sahabatkreator-launch/README.md` untuk branding, token warna, dan filosofi copy.

## Spec

| Prop     | Value                              |
| -------- | ---------------------------------- |
| Durasi   | 30 detik                           |
| Resolusi | 1080×1920 (9:16)                   |
| FPS      | 30                                 |
| Adegan   | 5 (hero → problem → fitur → platform → CTA) |

## Struktur

```
index.html                        ← parent: mount 5 scene
compositions/scene-hero.html      ← 0–6s
compositions/scene-problem.html   ← 6–13s  (badge SVG berubah ke logo brand)
compositions/scene-features.html  ← 13–21s (grid 2 kolom, bukan 3)
compositions/scene-platforms.html ← 21–26s (grid 4×2, badge 168px)
compositions/scene-cta.html       ← 26–30s
```

Setiap sub-composition punya timeline **lokal** (mulai dari 0) yang terdaftar di
`window.__timelines["<composition-id>"]`. Parent hanya mengatur `data-start` /
`data-duration` per host.

## Perbedaan dari versi landscape

- `data-width`/`data-height` di parent dan di setiap scene: 1080×1920
- Grid fitur: 3 kolom × 380px → 2 kolom × 460px (3 baris)
- Grid platform: 1 baris 8 badge → grid 4×2, badge 118px → 168px (lebih besar untuk layar HP)
- Font size headline sedikit lebih kecil, padding horizontal ditambah
- Ikon platform: **SVG brand glyph** (Instagram, TikTok, YouTube, Facebook, X, WhatsApp,
  Shopee, LinkedIn) — satu warna putih di badge warna brand, bukan singkatan teks

## Render lokal

```bash
npx hyperframes browser ensure
npx hyperframes lint
npx hyperframes check
npx hyperframes render --output renders/launch-portrait.mp4
```

Status terakhir: lint 0/0, check 0 errors / 0 warnings, contrast 22/22 pass WCAG AA.

## Render via CI

Workflow di `.github/workflows/render-launch-video.yml` merender **dua** proyek secara
paralel (matrix): landscape dan portrait. Artifact: `launch-video-landscape` dan
`launch-video-portrait`, retensi 30 hari.
