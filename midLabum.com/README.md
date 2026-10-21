# Dokumentasi Manual Testing — MidLab (Sistem Kebutuhan Laboratorium)

Dokumentasi pengujian manual untuk **MidLab**, sistem informasi pengelolaan kebutuhan laboratorium Program Studi D3 Kebidanan, Universitas Negeri Malang (2026). Seluruh isi disusun dari *Laporan Akhir – Sistem Kebutuhan Laboratorium (MidLab)*, BAB 5 Pengujian.

> Seluruh pengujian dijalankan di **https://midlabum.com**.

| Info | Detail |
|---|---|
| Penguji | Rafid |
| Browser | Google Chrome 155.0.8059.85 |
| Lingkungan uji | Produksi: `https://midlabum.com` |
| Tanggal pengujian | 2 September 2026 |

## Ringkasan Proyek

MidLab adalah aplikasi web yang membantu dosen dan Kasub Lab menghitung kebutuhan alat dan bahan habis pakai (BHP) praktikum secara otomatis dengan pembulatan ke atas (ceil), serta mengelola mata kuliah, keterampilan praktikum, modul praktikum, inventaris, change request, approval, laporan, audit log, dan backup.

Role pengguna: **Kasub Lab (Admin)**, **Dosen**, **Mahasiswa**, dan **Laboran**.

## Website yang Diuji

| Lingkungan | URL | Keterangan |
|---|---|---|
| **Produksi** | `https://midlabum.com` | Hostinger shared hosting; seluruh test case dijalankan di sini |

## Tools & Teknologi

| Kategori | Detail |
|---|---|
| Aplikasi yang diuji | Laravel 12, PHP 8.2.12, MySQL, Bootstrap 5, Chart.js, Spatie Laravel Permission, DOMPDF, PhpSpreadsheet |
| Metode pengujian | Black-box manual testing: Boundary Value Analysis dan All-Pairs Testing |
| Browser & alat bantu | Google Chrome 155.0.8059.85 (desktop + mobile view DevTools) |
| Pencatatan hasil | Dokumen Markdown |

## Scope

**Dalam scope:** registrasi dan login (validasi, throttling), persetujuan dan status akun pengguna, perhitungan kebutuhan (rumus, ceil, status draft/submitted/approved/locked), change request dan approval, modul praktikum (upload, versi, pratinjau, hapus), inventaris (foto, zoom), laporan (filter, ekspor PDF/Excel), notifikasi, hak akses per role, serta keamanan dasar dan responsivitas.

**Di luar scope:** pengujian performa/beban, pengujian otomatis, penetration test mendalam, serta fitur audit log dan backup secara fungsional.

## Hasil Singkat

| Total test case | PASS | FAIL | BLOCKED | Bug aktual |
|---|---|---|---|---|
| 83 | 81 | 2 | 0 | 2 |

Pengujian menemukan 2 bug aktual (BUG-18: NIP dapat diubah; BUG-21: upload foto barang >2MB gagal tanpa pesan) serta perbaikan spesifikasi BVA-21/22 (batas telepon 30 karakter, bukan 15/16 digit). Detail dan risiko yang belum terselesaikan ada di [`test-summary.md`](test-summary.md).

## Navigasi Dokumentasi

| Dokumen | Isi |
|---|---|
| [`MidLab-Manual-Testing.pdf`](MidLab-Manual-Testing.pdf) | Gabungan seluruh dokumen dalam satu PDF (26 halaman) dengan daftar isi |
| [`test-plan.md`](test-plan.md) | Tujuan, scope, jenis testing, environment, dan pendekatan pengujian |
| [`test-cases.md`](test-cases.md) | 83 test case: precondition, langkah, test data, expected/actual result, status |
| [`bug-reports.md`](bug-reports.md) | Risk checklist BUG-01 s.d. BUG-20, temuan baru BUG-21, status, dan template bug baru |
| [`test-summary.md`](test-summary.md) | Rekap PASS/FAIL/BLOCKED, jumlah bug, hasil akhir, risiko tersisa |

## Struktur Folder

```
midlab-testing-docs/
├── README.md
├── MidLab-Manual-Testing.pdf
├── test-plan.md
├── test-cases.md
├── bug-reports.md
└── test-summary.md
```
