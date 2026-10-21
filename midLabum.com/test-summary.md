# Test Summary — MidLab

Penguji: Rafid · Browser: Google Chrome 155.0.8059.85 · Lingkungan uji: produksi `https://midlabum.com` · Tanggal pengujian: 2 September 2026.

## 1. Hasil Eksekusi Test Case

| Suite | Total | PASS | FAIL | BLOCKED |
|---|---|---|---|---|
| A. BVA (BVA-01 – BVA-22) | 22 | 21 | 1 (BVA-12) | 0 |
| B. All-Pairs (TC-01 – TC-50) | 50 | 50 | 0 | 0 |
| C. Keamanan & Responsif (SEC-01 – SEC-08) | 8 | 8 | 0 | 0 |
| D. Tambahan (EXT-01 – EXT-03) | 3 | 2 | 1 (EXT-03) | 0 |
| **Total** | **83** | **81** | **2** | **0** |

- **Pass rate: 97,6% (81/83).**
- Seluruh hasil berasal dari pengujian penguji di `midlabum.com`.
- **Revisi BVA-21/22:** spesifikasi awal (15 vs 16 digit) tidak sesuai batas aplikasi karena 16 digit tetap diterima. Spesifikasi diubah menjadi 30 vs 31 karakter.

### Rincian All-Pairs per Kelompok

| Kelompok | Area | Total | PASS | FAIL | BLOCKED |
|---|---|---|---|---|---|
| TR-01 | Login | 6 | 6 | 0 | 0 |
| TR-02 | Registrasi | 3 | 3 | 0 | 0 |
| TR-03 | Perhitungan | 7 | 7 | 0 | 0 |
| TR-04 | Change Request | 4 | 4 | 0 | 0 |
| TR-05 | Manajemen Pengguna | 5 | 5 | 0 | 0 |
| TR-06 | Modul Praktikum | 5 | 5 | 0 | 0 |
| TR-07 | Inventaris | 5 | 5 | 0 | 0 |
| TR-08 | Laporan | 3 | 3 | 0 | 0 |
| TR-09 | Notifikasi | 4 | 4 | 0 | 0 |
| TR-10 | Hak Akses | 8 | 8 | 0 | 0 |
| | **Total** | **50** | **50** | **0** | **0** |

## 2. Jumlah Bug

| Kategori | Jumlah |
|---|---|
| **Bug aktual (Open)** | **2** — BUG-18 (NIP dapat diubah), BUG-21 (upload foto barang >2MB gagal tanpa pesan) |
| Item risk checklist yang diuji | 18 (dari 20 item Tabel 5.1; BUG-11 dan BUG-17 dikeluarkan dari scope) |
| — Verified (tidak terjadi) | 17 |
| — Terkonfirmasi (terjadi) | 1 (BUG-18) |

| Severity | Total | Verified | Terkonfirmasi |
|---|---|---|---|
| Critical | 4 | 4 | 0 |
| High | 6 | 6 | 0 |
| Medium | 6 | 5 | 1 (BUG-18) |
| Low | 2 | 2 | 0 |
| **Total** | **18** | **17** | **1** |

BUG-21 adalah temuan baru di luar checklist (severity usulan: Medium).

## 3. Hasil Akhir

Seluruh item **Critical dan High terverifikasi tidak terjadi**, fungsi utama berjalan, dan kontrol keamanan dasar (akses, throttling, CSRF, XSS, header) terbukti. Namun ada **2 bug Medium yang masih Open**: BUG-18 dan BUG-21.

**Kesimpulan: layak dengan catatan** — BUG-18 dan BUG-21 perlu diperbaiki atau diterima secara formal sebelum rilis.

## 4. Risiko yang Belum Terselesaikan

| No | Risiko | Severity | Keterangan | Rekomendasi |
|---|---|---|---|---|
| 1 | **BUG-18:** NIP dapat diubah setelah tersimpan | Medium | Terkonfirmasi pada role Kasub Lab, halaman Edit Profil. Aturan sistem: NIP tidak boleh diubah | Kunci field NIP (read-only dan tolak di sisi server) |
| 2 | **BUG-21:** upload foto barang >2MB gagal tanpa pesan | Medium (usulan) | Item tidak tersimpan dan user tidak diberi tahu | Tambah blok `@error` dan/atau cek ukuran di JS |
| 3 | Audit Log dan Backup tidak punya test case fungsional | Low | Modul ada di BAB 4; BUG-11 dan BUG-17 dikeluarkan dari scope | Tambah test case bila scope diperluas |

### Observasi (bukan bug, perlu dipertimbangkan)

- **Throttle login per IP:** 6 percobaan gagal/menit dari satu IP juga menolak login berkredensial benar dari IP itu selama 1 menit. Pada jaringan bersama, pengguna lain berpotensi ikut terkunci.
- **Header `X-Powered-By`:** versi PHP (8.3.35) terlihat pada header respons produksi. Disarankan disembunyikan (Low).
- **Kop hasil ekspor:** PDF/Excel menampilkan "S1 Kebidanan" dan kontak yang tampak placeholder (`021-5550001`, `lab@univ.ac.id`), sedangkan laporan menyebut prodi D3 Kebidanan.
