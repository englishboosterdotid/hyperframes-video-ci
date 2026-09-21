# hyperframes-video

Kumpulan video yang dirender dengan [HyperFrames](https://hyperframes.heygen.com). Setiap
folder adalah satu proyek video (satu `index.html`), siap dirender lokal atau via CI.

## Isi

| Folder                          | Video                                      | Status |
| ------------------------------- | ------------------------------------------ | ------ |
| `sahabatkreator-launch`         | Product launch 30 detik, **16:9** (1920×1080) | Komposisi selesai, siap render |
| `sahabatkreator-launch-portrait` | Product launch 30 detik, **9:16** (1080×1920) | Komposisi selesai, siap render |

## Render lokal

```bash
cd sahabatkreator-launch
npx hyperframes browser ensure
npx hyperframes lint
npx hyperframes render --output renders/launch.mp4
```

Butuh Node.js 22+ dan FFmpeg.

## Render via CI

`.github/workflows/render-launch-video.yml` merender di GitHub-hosted runner (Chrome +
FFmpeg di-install di dalam job). Repo public → GitHub Actions gratis tanpa batas menit;
repo private → 2.000 menit/bulan di paket Free.

Setiap video folder mendapatkan entry-nya sendiri di workflow, atau tambahkan matrix job
kalau sudah banyak.

## Konvensi folder

```
hyperframes-video/
├── .github/workflows/               # CI render (matrix: landscape + portrait)
├── sahabatkreator-launch/           # 16:9 landscape
│   ├── index.html                   # parent composition
│   ├── compositions/                # sub-composition per adegan
│   ├── renders/                     # output MP4 (gitignored)
│   └── README.md
├── sahabatkreator-launch-portrait/  # 9:16 portrait (TikTok/Reels/Shorts)
│   ├── index.html
│   ├── compositions/
│   ├── renders/
│   └── README.md
└── README.md
```

Output render (`renders/`) jangan di-commit — artifact CI yang jadi sumber MP4.
