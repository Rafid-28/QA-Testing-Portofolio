# Dokumentasi Manual Testing — MidLab (Sistem Kebutuhan Laboratorium)

Dokumentasi pengujian untuk **MidLab**, sistem informasi pengelolaan kebutuhan laboratorium Program Studi D3 Kebidanan, Universitas Negeri Malang (2026). Isi disusun dari *Laporan Akhir – Sistem Kebutuhan Laboratorium (MidLab)*, BAB 5 Pengujian, dan hasil pengujian 22 Oktober 2026.

| Info | Detail |
|---|---|
| Penguji | Rafid |
| Browser | Google Chrome 155.0.8059.85 |
| Lingkungan uji | Pengembangan (lokal): `http://127.0.0.1:8000` |
| Tanggal pengujian | 22 Oktober 2026 |

## Ringkasan Proyek

MidLab adalah aplikasi web yang membantu dosen dan Kasub Lab menghitung kebutuhan alat dan bahan habis pakai (BHP) praktikum secara otomatis dengan pembulatan ke atas (ceil), serta mengelola mata kuliah, keterampilan praktikum, modul praktikum, inventaris, change request, approval, laporan, audit log, dan backup.

Role pengguna: **Kasub Lab (Admin)**, **Dosen**, **Mahasiswa**, dan **Laboran**.

## Website yang Diuji

| Lingkungan | URL | Keterangan |
|---|---|---|
| **Pengembangan (lokal)** | `http://127.0.0.1:8000` | Lingkungan pengujian (22 Oktober 2026) |
| Produksi | `https://midlabum.com` | Hostinger shared hosting; **tidak diuji secara fungsional**, hanya response header yang diperiksa terpisah |

## Tools & Teknologi

| Kategori | Detail |
|---|---|
| Aplikasi yang diuji | Laravel 12, PHP 8.2.12, MySQL, Bootstrap 5, Chart.js, Spatie Laravel Permission, DOMPDF, PhpSpreadsheet |
| Metode pengujian | Black-box functional testing: Boundary Value Analysis dan All-Pairs; bukti diambil dengan skrip browser (headless Chromium) |
| Browser | Google Chrome 155.0.8059.85 (desktop 1440x900 dan mobile 390x844) |
| Pencatatan hasil | Dokumen Markdown + screenshot pada folder `evidence/` |

## Scope

**Dalam scope:** registrasi dan login (validasi, throttling), persetujuan dan status akun pengguna, perhitungan kebutuhan (rumus, ceil, status draft/submitted/approved/locked), change request dan approval, modul praktikum (upload, versi, pratinjau, hapus), inventaris (foto, zoom), laporan (filter, ekspor PDF/Excel), notifikasi, hak akses per role, serta keamanan dasar dan responsivitas.

**Di luar scope:** pengujian performa/beban, pengujian otomatis (unit/integration), penetration test mendalam, pengujian fungsional pada produksi, serta fitur audit log dan backup secara fungsional.

## Hasil Singkat

| Total test case | PASS | FAIL | BLOCKED | Bug terkonfirmasi |
|---|---|---|---|---|
| 83 | 78 | 5 | 0 | 2 |

Pengujian menemukan 2 bug terkonfirmasi (BUG-21: upload foto barang >2MB gagal tanpa pesan yang terlihat; BUG-22: jumlah mahasiswa 0 diterima) dan 3 temuan yang perlu keputusan apakah bug atau perubahan spesifikasi (BUG-23 s.d. BUG-25), serta perbaikan spesifikasi BVA-21/22 (batas telepon 30 karakter, bukan 15/16 digit). Detail dan risiko yang belum terselesaikan ada di `test-summary.md`.

## Navigasi Dokumentasi

| Dokumen | Isi |
|---|---|
| [`MidLab-Manual-Testing-dengan-Evidence.pdf`](MidLab-Manual-Testing-dengan-Evidence.pdf) | Gabungan seluruh dokumen dalam satu PDF (71 halaman) dengan daftar isi dan lampiran 88 screenshot |
| [`evidence/`](evidence/) | Screenshot pengujian 22 Oktober 2026 dan log header respons produksi |
| [`test-plan.md`](test-plan.md) | Tujuan, scope, jenis testing, environment, dan pendekatan pengujian |
| [`test-cases.md`](test-cases.md) | 83 test case: precondition, langkah, test data, expected/actual result, status |
| [`bug-reports.md`](bug-reports.md) | Risk checklist BUG-01 s.d. BUG-20, temuan baru BUG-21 s.d. BUG-25, status, dan template bug baru |
| [`test-summary.md`](test-summary.md) | Rekap PASS/FAIL/BLOCKED, jumlah bug, hasil akhir, risiko tersisa |

## Struktur Folder

```
midlab-testing-docs/
├── README.md
├── MidLab-Manual-Testing-dengan-Evidence.pdf
├── test-plan.md
├── test-cases.md
├── bug-reports.md
├── test-summary.md
└── evidence/
    ├── SEC-06-headers-production.txt
    └── screenshots/       (88 berkas PNG)
```
