# Drama AI Gratis — Wan 2.2 di Google Colab (Wan2GP)

Bikin drama AI gaya Seedance / Higgsfield, tapi **gratis, tanpa watermark, dan boleh komersial**
(lisensi Apache-2.0). Modelnya Wan 2.2 open-source dari Alibaba.

Repo ini khusus **Google Colab** — GPU gratis T4 16 GB.
Untuk Kaggle, lihat repo terpisah: [`ghozali25/drama-kaggle`](https://github.com/ghozali25/drama-kaggle).

> Status: template. Langkah-langkahnya disusun dari dokumentasi resmi Colab dan
> notebook komunitas Wan2GP. **Belum diuji live di T4 oleh penulis** (sandbox penulis
> tidak punya GPU). Kalau ada cell yang error, kirim screenshot error-nya.

## Kenapa Colab?

- Sandbox Muse: tanpa GPU, RAM 7,7 GB.
- InstaBox (`friday` / `jarvis` / `vision`): 4 vCPU / 8 GB RAM / 20 GB disk, tanpa GPU,
  plus auto-suspend 30 menit idle dan max-run 480 menit di Free.
- Wan 2.2 TI2V-5B butuh 6–8 GB VRAM, dan RAM sistem minimal 24 GB.
- Colab Free memberi **T4 16 GB VRAM, sesi maks 12 jam, kuota 15–30 jam/minggu**
  (angka tidak dipublish Google, dan pemakaian berat bisa diblokir sehari).

Yang muat di T4 16 GB:

- ✅ **Wan 2.2 TI2V-5B / FastWan (Text+Image-to-Video), 480p, 5 detik** — ini yang dipakai di sini.
  1 klip 5 detik sekitar 15–25 menit di T4.
- ⚠️ Wan 2.2 14B — hanya GGUF Q4/Q5, kalau tidak VRAM habis.
- ❌ 14B FP16 720p — butuh 24 GB+ VRAM, bukan untuk free tier.

Colab paling enak buat **tes cepat**. Buat produksi batch harian, Kaggle lebih bisa
diprediksi (kuota tertulis ~30 jam/minggu, RAM 29 GB).

## Alur drama

1. **Naskah dulu.** 1 episode = 6 scene, tiap scene 5 detik.
   Contoh lengkap: [`prompts/mamang-ai/episode-01-balas-dendam.md`](prompts/mamang-ai/episode-01-balas-dendam.md).
2. **Kunci karakter.** Bikin 1 foto referensi karakter, lalu semua scene pakai
   **image-to-video** dari foto itu. Jangan text-to-video terus — mukanya ganti-ganti.
3. **Render per scene di Colab**, download sebelum sesi habis.
4. **Edit di CapCut desktop**: susun 6 klip, tambah subtitle bakar, musik, SFX.

## Langkah demi langkah

### 0. Siapkan akun
Login Google, buka Colab. Tidak perlu kartu kredit untuk free tier.

### 1. Buka notebook
File: [`wan2gp_colab.ipynb`](wan2gp_colab.ipynb)

Upload file itu ke Colab (**File > Upload notebook**), atau buka dari GitHub.
Pastikan **Runtime > Change runtime type > GPU (T4)**.

### 2. Jalankan cell dari atas ke bawah
1. **Mount Google Drive** — model disimpan di `/content/drive/MyDrive/Wan2GP_Models`
   supaya tidak download ulang tiap sesi.
2. **Clone Wan2GP** — `deepbeepmeep/Wan2GP`.
3. **Symlink folder models ke Drive** — `models` diarahkan ke folder Drive di atas.
4. **Install requirements** — `pip install -r requirements.txt`.
5. **Launch Wan2GP** — `python wgp.py --share --profile 5`, klik link Gradio publik.

Sesi pertama cuma butuh Cell 1 + Cell 5 setelah model sudah ada di Drive —
sisanya sudah tersimpan.

### 3. Generate di Gradio
1. Pilih model **Wan 2.2 TI2V-5B FastWan**, **480p**, **5 detik**.
2. Prompt + gambar referensi dari `prompts/mamang-ai/episode-01-balas-dendam.md`.
3. Render **1 scene dulu** untuk tes, baru batch 6 scene dalam sesi yang sama.
4. Download hasil sebelum sesi putus. Colab putus kalau idle ~90 menit,
   dan sesi maks 12 jam.

## File di repo ini

```
.
├── README.md
├── wan2gp_colab.ipynb            # notebook utama — upload ke Colab
├── prompts/
│   └── mamang-ai/
│       ├── episode-01-balas-dendam.md
│       └── episode-02-pernikahan-palsu.md
├── scripts/
│   └── cek_gpu.py                 # cek GPU + torch
├── LICENSE                        # Apache-2.0
└── .gitignore
```

## Troubleshooting

| Gejala | Perbaiki |
|---|---|
| Runtime cuma CPU / tidak ada GPU | Runtime → Change runtime type → GPU (T4), tunggu |
| `ModuleNotFoundError: No module named 'models.wan'` | Pastikan cell symlink models ke Drive sudah jalan sebelum install |
| `CUDA out of memory` | Ganti ke TI2V-5B, 480p, profile 5 — jangan 14B FP16 |
| Sesi putus di tengah render | Jangan idle 90 menit, render batch dalam 1 sesi |
| Muka karakter beda tiap scene | Pakai image-to-video + foto referensi yang sama |

## Kredit

- Wan 2.2 — Alibaba, Apache-2.0.
- Wan2GP — `deepbeepmeep/Wan2GP`.
- Pola Colab + Drive — terinspirasi `Square-Zero-Labs/Wan2GP-on-Colab`.

Repo ini bukan afiliasi resmi Wan, Wan2GP, atau Google.
Gunakan sesuai Terms of Service Colab (free tier untuk personal use).
