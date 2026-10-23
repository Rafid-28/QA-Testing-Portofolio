# Bug Reports — MidLab

## Ringkasan

Pengujian pada 22 Oktober 2026 (lingkungan lokal) menemukan **2 bug terkonfirmasi** (BUG-21, BUG-22) dan **3 temuan yang perlu keputusan** apakah termasuk bug atau perubahan spesifikasi (BUG-23, BUG-24, BUG-25).

`BUG-01` s.d. `BUG-20` berasal dari Tabel 5.1 laporan ("Daftar Kemungkinan Bug Sebelum Pengujian") dan diperlakukan sebagai **risk checklist**. `BUG-21` s.d. `BUG-25` adalah temuan baru di luar checklist.

| Hasil | Jumlah |
|---|---|
| Bug terkonfirmasi (Open) | **2** (BUG-21, BUG-22) |
| Temuan perlu keputusan (Open) | **3** (BUG-23, BUG-24, BUG-25) |
| Item checklist yang diuji | 18 (dari 20 item Tabel 5.1) |
| — Verified (tidak terjadi) | 18 |

> **Catatan**
> - **Severity** BUG-01 – BUG-20 diambil dari laporan; severity BUG-21 s.d. BUG-25 adalah *usulan* dokumentasi ini. **Priority** tidak ada di laporan; kolom ini diturunkan dari severity dengan pemetaan Critical → P1, High → P2, Medium → P3, Low → P4.
> - Bukti berupa screenshot pengujian 22 Oktober 2026 di `evidence/screenshots/`.
> - BUG-18: pada log pengujian awal 10 Oktober 2026 NIP masih dapat diubah; pada pengujian 22 Oktober 2026 NIP tidak dapat diubah, sehingga item ini berstatus Verified.
> - BUG-11 (backup saat `exec()` nonaktif) dan BUG-17 (stok vs status barang) ada di Tabel 5.1 laporan tetapi **dikeluarkan dari scope pengujian**, sehingga checklist ini berisi 18 dari 20 item.
> - Throttle login berlaku **per IP**, bukan per akun (BVA-08): kredensial benar dari IP yang sama juga ditolak (HTTP 429) selama window aktif. Ini tidak dicatat sebagai bug.

## Risk Checklist

| Bug ID | Judul | Severity | Priority | TC Terkait | Status |
|---|---|---|---|---|---|
| BUG-01 | Validasi NIM tidak menerima tepat 12 digit | High | P2 | BVA-01, BVA-02, BVA-03 | Closed — Tidak terjadi (Verified) |
| BUG-02 | Login throttling tidak aktif setelah 6x percobaan salah | High | P2 | BVA-07, BVA-08, SEC-04 | Closed — Tidak terjadi (Verified) |
| BUG-03 | Perhitungan pembulatan tidak selalu ceil | Critical | P1 | BVA-14, BVA-17, BVA-18 | Closed — Tidak terjadi (Verified) |
| BUG-04 | File upload >2MB tetap diproses | Medium | P3 | BVA-11, BVA-12 | Closed — Tidak terjadi (Verified) |
| BUG-05 | Hapus data yang sudah di-submit masih bisa dilakukan | High | P2 | TC-13, TC-18 | Closed — Tidak terjadi (Verified) |
| BUG-06 | Dosen dapat mengakses/upload modul mata kuliah dosen lain | Critical | P1 | TC-29 | Closed — Tidak terjadi (Verified) |
| BUG-07 | Mahasiswa dapat melihat perhitungan draft/submitted | High | P2 | TC-48 | Closed — Tidak terjadi (Verified) |
| BUG-08 | Permission role tidak konsisten | High | P2 | TC-43 s.d. TC-47, TC-49, TC-50, SEC-01 – SEC-03 | Closed — Tidak terjadi (Verified) |
| BUG-09 | Notifikasi tidak muncul / badge tidak ter-update | Medium | P3 | TC-15, TC-16, TC-19, TC-39 s.d. TC-42 | Closed — Tidak terjadi (Verified) |
| BUG-10 | Ekspor laporan PDF/Excel kosong atau rusak | Medium | P3 | TC-37, TC-38 | Closed — Tidak terjadi (Verified) |
| BUG-12 | Output HTML tidak ter-escape (XSS) | Critical | P1 | EXT-01 | Closed — Tidak terjadi (Verified) |
| BUG-13 | Form tanpa proteksi CSRF | Critical | P1 | EXT-02 | Closed — Tidak terjadi (Verified) |
| BUG-14 | Sidebar menampilkan menu operasional untuk user pending | High | P2 | TC-04, TC-43, TC-44, TC-45 (verifikasi tidak langsung) | Closed — Tidak terjadi (Verified) |
| BUG-15 | Responsive mobile gagal | Low | P4 | SEC-08 | Closed — Tidak terjadi (Verified) |
| BUG-16 | Throttle registrasi tidak berlaku | Medium | P3 | BVA-09, BVA-10, SEC-05 | Closed — Tidak terjadi (Verified) |
| BUG-18 | NIP dosen dapat diubah setelah tersimpan | Medium | P3 | EXT-03 | Closed — Tidak terjadi (Verified) |
| BUG-19 | Nama file ekspor tanpa tanggal unduh | Low | P4 | TC-37, TC-38 | Closed — Tidak terjadi (Verified) |
| BUG-20 | jumlah_kelompok > jumlah dosen tidak dicegah | Medium | P3 | BVA-15, BVA-16 | Closed — Tidak terjadi (Verified) |
| BUG-21 | Validasi senyap: upload foto barang >2MB gagal tanpa pesan yang terlihat (pesan hanya ada di modal yang tertutup) | Medium (usulan) | P3 | BVA-12 | Open — Terkonfirmasi (bug aktual) |
| BUG-22 | Jumlah mahasiswa 0 diterima (validasi min:0, bukan min:1) | Medium (usulan) | P3 | BVA-13 | Open — Terkonfirmasi (bug aktual) |
| BUG-23 | Form registrasi mahasiswa tidak memiliki pilihan mata kuliah | Low (usulan) | P4 | TC-08 | Open — Perlu keputusan (bug atau perubahan spesifikasi) |
| BUG-24 | Perhitungan Draft tidak dapat diedit (tidak ada form edit) | Medium (usulan) | P3 | TC-10 | Open — Perlu keputusan (bug atau perubahan spesifikasi) |
| BUG-25 | Form edit inventaris tidak menyediakan field foto | Low (usulan) | P4 | TC-34 | Open — Perlu keputusan (bug atau perubahan spesifikasi) |

## Detail per Item

### BUG-01 — Validasi NIM tidak menerima tepat 12 digit

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | High / P2 |
| Langkah Reproduksi / Verifikasi | 1. Registrasi NIM 12 digit<br>2. Registrasi NIM 11 dan 13 digit |
| Expected Result | 12 digit diterima; 11/13 digit ditolak dengan pesan 'NIM harus 12 digit' |
| Actual Result | Sesuai expected |
| Test Case Terkait | BVA-01, BVA-02, BVA-03 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-02 — Login throttling tidak aktif setelah 6x percobaan salah

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | High / P2 |
| Langkah Reproduksi / Verifikasi | 1. Login salah password 5x lalu login benar<br>2. Login salah password 6x |
| Expected Result | Percobaan ke-5 normal; ke-6 terkunci sementara |
| Actual Result | Sesuai expected |
| Test Case Terkait | BVA-07, BVA-08, SEC-04 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-03 — Perhitungan pembulatan tidak selalu ceil

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Critical / P1 |
| Langkah Reproduksi / Verifikasi | 1. Hitung dengan hasil 13,33<br>2. Hitung dengan hasil 12,0 |
| Expected Result | 13,33 menjadi 14; 12,0 tetap 12 |
| Actual Result | Sesuai expected |
| Test Case Terkait | BVA-14, BVA-17, BVA-18 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-04 — File upload >2MB tetap diproses

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Medium / P3 |
| Langkah Reproduksi / Verifikasi | 1. Upload file 2 MB<br>2. Upload file 2 MB + 1 KB |
| Expected Result | 2 MB diproses; >2 MB ditolak 'maksimal 2MB' |
| Actual Result | Sesuai expected |
| Test Case Terkait | BVA-11, BVA-12 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-05 — Hapus data yang sudah di-submit masih bisa dilakukan

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | High / P2 |
| Langkah Reproduksi / Verifikasi | 1. Login Dosen<br>2. Hapus perhitungan/CR berstatus Submitted |
| Expected Result | Gagal (403/error) |
| Actual Result | Sesuai expected |
| Test Case Terkait | TC-13, TC-18 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-06 — Dosen dapat mengakses/upload modul mata kuliah dosen lain

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Critical / P1 |
| Langkah Reproduksi / Verifikasi | 1. Login Dosen<br>2. Upload modul pada MK dosen lain |
| Expected Result | MK tidak muncul di dropdown / 403 |
| Actual Result | Sesuai expected |
| Test Case Terkait | TC-29 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-07 — Mahasiswa dapat melihat perhitungan draft/submitted

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | High / P2 |
| Langkah Reproduksi / Verifikasi | 1. Login Mahasiswa<br>2. Buka Hasil Perhitungan |
| Expected Result | Hanya perhitungan approved/locked tampil |
| Actual Result | Sesuai expected |
| Test Case Terkait | TC-48 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-08 — Permission role tidak konsisten

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | High / P2 |
| Langkah Reproduksi / Verifikasi | 1. Akses URL terlarang per role (mahasiswa, dosen pending, tanpa login) |
| Expected Result | Redirect login / halaman Menunggu Persetujuan / 403 sesuai role |
| Actual Result | Sesuai expected |
| Test Case Terkait | TC-43 s.d. TC-47, TC-49, TC-50, SEC-01 – SEC-03 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-09 — Notifikasi tidak muncul / badge tidak ter-update

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Medium / P3 |
| Langkah Reproduksi / Verifikasi | 1. Submit/approve/lock perhitungan<br>2. Cek badge, buka dropdown, tandai dibaca |
| Expected Result | Notifikasi muncul; badge sesuai jumlah belum dibaca; reset ke 0 |
| Actual Result | Sesuai expected |
| Test Case Terkait | TC-15, TC-16, TC-19, TC-39 s.d. TC-42 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-10 — Ekspor laporan PDF/Excel kosong atau rusak

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Medium / P3 |
| Langkah Reproduksi / Verifikasi | 1. Buka Laporan<br>2. Ekspor PDF dan Excel |
| Expected Result | File terunduh dan berisi data |
| Actual Result | Sesuai expected |
| Test Case Terkait | TC-37, TC-38 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-12 — Output HTML tidak ter-escape (XSS)

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Critical / P1 |
| Langkah Reproduksi / Verifikasi | 1. Input skrip HTML/JS pada field nama<br>2. Tampilkan kembali halaman terkait |
| Expected Result | Output ter-escape, skrip tidak dieksekusi |
| Actual Result | Output tampil sebagai teks ter-escape (&lt;script&gt;alert(1)&lt;/script&gt;); skrip tidak dijalankan |
| Test Case Terkait | EXT-01 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-13 — Form tanpa proteksi CSRF

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Critical / P1 |
| Langkah Reproduksi / Verifikasi | 1. Kirim POST /users/2/suspend tanpa token CSRF |
| Expected Result | Request ditolak (419) |
| Actual Result | HTTP 419 |
| Test Case Terkait | EXT-02 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-14 — Sidebar menampilkan menu operasional untuk user pending

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | High / P2 |
| Langkah Reproduksi / Verifikasi | 1. Login Dosen pending<br>2. Akses menu/halaman operasional |
| Expected Result | Diarahkan ke Menunggu Persetujuan; menu operasional tidak dapat diakses |
| Actual Result | Sesuai expected (halaman operasional tidak dapat diakses; tampilan sidebar tidak diperiksa terpisah) |
| Test Case Terkait | TC-04, TC-43, TC-44, TC-45 (verifikasi tidak langsung) |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-15 — Responsive mobile gagal

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Low / P4 |
| Langkah Reproduksi / Verifikasi | 1. Buka aplikasi pada ukuran layar mobile<br>2. Collapse/expand sidebar |
| Expected Result | Tampilan responsif dan fungsional |
| Actual Result | Sesuai expected |
| Test Case Terkait | SEC-08 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-16 — Throttle registrasi tidak berlaku

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Medium / P3 |
| Langkah Reproduksi / Verifikasi | 1. Registrasi ke-10 dan ke-11 dari IP sama dalam 1 menit |
| Expected Result | Ke-10 berhasil; ke-11 diblokir |
| Actual Result | Sesuai expected |
| Test Case Terkait | BVA-09, BVA-10, SEC-05 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-18 — NIP dosen dapat diubah setelah tersimpan

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Medium / P3 |
| Langkah Reproduksi / Verifikasi | 1. Login sebagai Kasub Lab, buka Edit Profil<br>2. Catat NIP tersimpan<br>3. Coba ubah NIP (lewat form dan POST langsung) |
| Expected Result | NIP tidak dapat diubah setelah tersimpan (aturan sistem) |
| Actual Result | NIP tidak berubah: field NIP disabled dan server mengabaikan perubahan. Catatan: pada log pengujian awal 10 Oktober 2026 NIP masih dapat diubah |
| Test Case Terkait | EXT-03 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-19 — Nama file ekspor tanpa tanggal unduh

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Low / P4 |
| Langkah Reproduksi / Verifikasi | 1. Ekspor laporan PDF/Excel<br>2. Periksa nama file hasil unduhan |
| Expected Result | Nama file memuat tanggal unduh |
| Actual Result | Nama file hasil ekspor (Laporan-Lengkap-DD-MM-YYYY.pdf / .xlsx) memuat tanggal |
| Test Case Terkait | TC-37, TC-38 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-20 — jumlah_kelompok > jumlah dosen tidak dicegah

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Medium / P3 |
| Langkah Reproduksi / Verifikasi | 1. Isi jumlah_kelompok = jumlah dosen<br>2. Isi jumlah dosen + 1 |
| Expected Result | Yang pertama lolos; yang kedua ditolak |
| Actual Result | Sesuai expected |
| Test Case Terkait | BVA-15, BVA-16 |
| Status | Closed — Tidak terjadi (Verified) |

### BUG-21 — Validasi senyap: upload foto barang >2MB gagal tanpa pesan yang terlihat (pesan hanya ada di modal yang tertutup)

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Medium (usulan) / P3 |
| Langkah Reproduksi / Verifikasi | 1. Login Kasub Lab, buka Manajemen Inventaris<br>2. Tambah item dengan foto JPEG 2 MB + 1 KB<br>3. Simpan |
| Expected Result | Pesan error ukuran file terlihat oleh user dan form dapat diperbaiki |
| Actual Result | Item tidak terbuat. Pesan 'The foto field must not be greater than 2048 kilobytes' hanya ada di dalam modal Tambah Barang yang tertutup setelah submit, jadi user tidak melihat pesan; pesan baru terlihat bila modal dibuka ulang. Pembanding: upload avatar menampilkan alert browser dan file dikosongkan |
| Test Case Terkait | BVA-12 |
| Status | Open — Terkonfirmasi (bug aktual) |

### BUG-22 — Jumlah mahasiswa 0 diterima (validasi min:0, bukan min:1)

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Medium (usulan) / P3 |
| Langkah Reproduksi / Verifikasi | 1. Login sebagai Dosen, buka form Perhitungan<br>2. Isi jumlah mahasiswa 0<br>3. Simpan |
| Expected Result | Validasi error: jumlah mahasiswa minimal 1 |
| Actual Result | Tidak ada error; perhitungan CAL-2026-0009 (draft) tersimpan dengan jumlah mahasiswa 0 (aturan validasi min:0) |
| Test Case Terkait | BVA-13 |
| Status | Open — Terkonfirmasi (bug aktual) |

### BUG-23 — Form registrasi mahasiswa tidak memiliki pilihan mata kuliah

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Low (usulan) / P4 |
| Langkah Reproduksi / Verifikasi | 1. Buka halaman Registrasi (tab Mahasiswa)<br>2. Isi form tanpa memilih mata kuliah<br>3. Klik Daftar |
| Expected Result | Validasi error: mata kuliah wajib dipilih (rancangan awal) |
| Actual Result | Form tidak memiliki field mata kuliah sehingga tidak ada validasi required; registrasi berhasil dan diarahkan ke /verify-email/code. Perlu keputusan: bug atau perubahan spesifikasi |
| Test Case Terkait | TC-08 |
| Status | Open — Perlu keputusan (bug atau perubahan spesifikasi) |

### BUG-24 — Perhitungan Draft tidak dapat diedit (tidak ada form edit)

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Medium (usulan) / P3 |
| Langkah Reproduksi / Verifikasi | 1. Login sebagai Dosen<br>2. Buka perhitungan berstatus Draft |
| Expected Result | Perhitungan Draft dapat diedit (rancangan awal) |
| Actual Result | Halaman detail Draft hanya menyediakan Hitung Ulang, Submit, dan Hapus; tidak ada form edit. Perlu keputusan: bug atau perubahan spesifikasi |
| Test Case Terkait | TC-10 |
| Status | Open — Perlu keputusan (bug atau perubahan spesifikasi) |

### BUG-25 — Form edit inventaris tidak menyediakan field foto

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Low (usulan) / P4 |
| Langkah Reproduksi / Verifikasi | 1. Buka halaman Inventaris<br>2. Klik Edit pada item berfoto |
| Expected Result | Foto dapat diganti pada form edit (rancangan awal) |
| Actual Result | Form edit inventaris tidak memiliki field foto; foto dikelola dari master barang. Perlu keputusan: bug atau perubahan spesifikasi |
| Test Case Terkait | TC-34 |
| Status | Open — Perlu keputusan (bug atau perubahan spesifikasi) |

## Template Bug Baru

Gunakan template berikut jika ditemukan bug pada pengujian ulang (regresi).

```markdown
### BUG-XX — <judul singkat>

| Field | Isi |
|---|---|
| Severity | Critical / High / Medium / Low |
| Priority | P1 / P2 / P3 / P4 |
| Environment | <URL, browser, OS> |
| Precondition | <kondisi awal> |
| Langkah Reproduksi | 1. ... <br> 2. ... |
| Expected Result | ... |
| Actual Result | ... |
| Test Case Terkait | <ID TC> |
| Bukti | <screenshot / log> |
| Status | New / Open / In Progress / Fixed / Retest / Closed |
```
