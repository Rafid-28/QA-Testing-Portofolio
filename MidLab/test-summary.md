# Test Summary — MidLab

Penguji: Rafid · Browser: Google Chrome 155.0.8059.85 · Lingkungan uji: lokal `http://127.0.0.1:8000` · Tanggal pengujian: 22 Oktober 2026.

## 1. Hasil Eksekusi Test Case

| Suite | Total | PASS | FAIL | BLOCKED |
|---|---|---|---|---|
| A. BVA (BVA-01 – BVA-22) | 22 | 20 | 2 (BVA-12, BVA-13) | 0 |
| B. All-Pairs (TC-01 – TC-50) | 50 | 47 | 3 (TC-08, TC-10, TC-34) | 0 |
| C. Keamanan & Responsif (SEC-01 – SEC-08) | 8 | 8 | 0 | 0 |
| D. Tambahan (EXT-01 – EXT-03) | 3 | 3 | 0 | 0 |
| **Total** | **83** | **78** | **5** | **0** |

- **Pass rate: 94,0% (78/83).**
- **Revisi BVA-21/22:** spesifikasi awal (15 vs 16 digit) tidak sesuai batas aplikasi karena 16 digit tetap diterima. Spesifikasi diubah menjadi 30 vs 31 karakter.

### Rincian All-Pairs per Kelompok

| Kelompok | Area | Total | PASS | FAIL | BLOCKED |
|---|---|---|---|---|---|
| TR-01 | Login | 6 | 6 | 0 | 0 |
| TR-02 | Registrasi | 3 | 2 | 1 (TC-08) | 0 |
| TR-03 | Perhitungan | 7 | 6 | 1 (TC-10) | 0 |
| TR-04 | Change Request | 4 | 4 | 0 | 0 |
| TR-05 | Manajemen Pengguna | 5 | 5 | 0 | 0 |
| TR-06 | Modul Praktikum | 5 | 5 | 0 | 0 |
| TR-07 | Inventaris | 5 | 4 | 1 (TC-34) | 0 |
| TR-08 | Laporan | 3 | 3 | 0 | 0 |
| TR-09 | Notifikasi | 4 | 4 | 0 | 0 |
| TR-10 | Hak Akses | 8 | 8 | 0 | 0 |
| | **Total** | **50** | **47** | **3** | **0** |

## 2. Jumlah Bug

| Kategori | Jumlah |
|---|---|
| **Bug terkonfirmasi (Open)** | **2** — BUG-21 (upload foto barang >2MB gagal tanpa pesan yang terlihat), BUG-22 (jumlah mahasiswa 0 diterima) |
| **Temuan perlu keputusan (Open)** | **3** — BUG-23 (registrasi tanpa pilihan mata kuliah), BUG-24 (perhitungan Draft tidak dapat diedit), BUG-25 (form edit inventaris tanpa field foto) |
| Item risk checklist yang diuji | 18 (dari 20 item Tabel 5.1; BUG-11 dan BUG-17 dikeluarkan dari scope) |
| — Verified (tidak terjadi) | 18 |

| Severity | Total | Verified |
|---|---|---|
| Critical | 4 | 4 |
| High | 6 | 6 |
| Medium | 6 | 6 |
| Low | 2 | 2 |
| **Total** | **18** | **18** |

BUG-18 (NIP dapat diubah) tidak terjadi pada pengujian 22 Oktober 2026; pada log pengujian awal 10 Oktober 2026 NIP masih dapat diubah. BUG-21 s.d. BUG-25 adalah temuan baru di luar checklist (severity usulan).

## 3. Hasil Akhir

Seluruh 18 item risk checklist, termasuk semua item **Critical dan High**, terverifikasi tidak terjadi, dan kontrol keamanan dasar (akses, throttling, CSRF, XSS, header) berfungsi. Namun ada **5 test case FAIL**: 2 bug terkonfirmasi (BUG-21, BUG-22) dan 3 temuan yang perilakunya berbeda dari rancangan awal (BUG-23, BUG-24, BUG-25).

**Kesimpulan: layak dengan catatan** — BUG-21 dan BUG-22 perlu diperbaiki, dan BUG-23 s.d. BUG-25 perlu diputuskan sebagai bug atau perubahan spesifikasi sebelum rilis.

## 4. Risiko yang Belum Terselesaikan

| No | Risiko | Severity | Keterangan | Rekomendasi |
|---|---|---|---|---|
| 1 | **BUG-21:** upload foto barang >2MB gagal tanpa pesan yang terlihat | Medium (usulan) | Item tidak tersimpan; pesan error hanya ada di modal yang tertutup | Tampilkan pesan di luar modal atau jaga modal tetap terbuka saat error |
| 2 | **BUG-22:** jumlah mahasiswa 0 diterima | Medium (usulan) | Validasi memakai min:0, sehingga perhitungan dengan 0 mahasiswa tersimpan | Ubah aturan validasi menjadi min:1 |
| 3 | **BUG-23 s.d. BUG-25** perlu keputusan | Medium/Low (usulan) | Registrasi tanpa pilihan mata kuliah, perhitungan Draft tanpa form edit, form edit inventaris tanpa foto | Putuskan bug atau perubahan spesifikasi; perbarui test case atau aplikasi |
| 4 | Produksi (`midlabum.com`) belum diuji secara fungsional | Medium | Konfigurasi berbeda (`exec()` nonaktif, PHP 8.3.35, header HSTS/CSP) berpotensi memengaruhi perilaku | Smoke test di produksi |
| 5 | Audit Log dan Backup tidak punya test case fungsional | Low | Modul ada di BAB 4; BUG-11 dan BUG-17 dikeluarkan dari scope | Tambah test case bila scope diperluas |

### Observasi (bukan bug, perlu dipertimbangkan)

- **Throttle login per IP:** 5 percobaan gagal/menit dari satu IP, dan percobaan ke-6 mendapat HTTP 429, juga menolak login berkredensial benar dari IP itu selama window aktif. Pada jaringan bersama, pengguna lain berpotensi ikut terkunci.
- **Header respons:** HSTS dan CSP tidak ada pada lingkungan lokal. Pemeriksaan header produksi terpisah menunjukkan `X-Powered-By: PHP/8.3.35` terlihat (disarankan disembunyikan, Low).
- **Kop hasil ekspor:** PDF/Excel menampilkan "S1 Kebidanan" dan kontak yang tampak placeholder (`021-5550001`, `lab@univ.ac.id`), sedangkan laporan menyebut prodi D3 Kebidanan.
- **Halaman Approval:** persetujuan change request dilakukan di halaman detail change request, tidak ada halaman Approval terpisah (TC-19, TC-20).
- **Metodologi bukti:** eksekusi dan pengambilan bukti dibantu skrip (headless Chromium). Area pratinjau PDF tampak kosong pada headless Chromium (TC-28) karena plugin PDF tidak dirender; ini bukan bug aplikasi.
