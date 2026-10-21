# Bug Reports — MidLab

## Ringkasan

Pengujian di `midlabum.com` menemukan **2 bug aktual**: BUG-18 (NIP dapat diubah) dan BUG-21 (validasi senyap pada upload foto barang).

`BUG-01` s.d. `BUG-20` berasal dari Tabel 5.1 laporan ("Daftar Kemungkinan Bug Sebelum Pengujian") dan diperlakukan sebagai **risk checklist**. `BUG-21` adalah temuan baru di luar checklist.

| Hasil | Jumlah |
|---|---|
| Bug aktual (Open — Terkonfirmasi) | **2** (BUG-18, BUG-21) |
| Item checklist — **Verified** (tidak terjadi) | 17 |
| Item checklist — **Terkonfirmasi** (terjadi) | 1 (BUG-18) |
| Total item checklist | 18 |

> **Catatan**
> - **Severity** BUG-01 – BUG-20 diambil dari laporan; severity BUG-21 adalah *usulan* dokumentasi ini. **Priority** tidak ada di laporan; kolom ini diturunkan dari severity dengan pemetaan Critical → P1, High → P2, Medium → P3, Low → P4.
> - Aturan sistem: NIP dosen **tidak boleh diubah** (konfirmasi penguji), sehingga BUG-18 berstatus bug aktual.
> - BUG-11 (backup saat `exec()` nonaktif) dan BUG-17 (stok vs status barang) ada di Tabel 5.1 laporan tetapi **dikeluarkan dari scope pengujian**, sehingga checklist ini berisi 18 dari 20 item.
> - Perbedaan perilaku BVA-08: throttle login berlaku **per IP**, bukan per akun. Ini tidak dicatat sebagai bug, tetapi perlu diketahui karena kredensial benar dari IP yang sama juga ditolak (HTTP 429) selama 1 menit.

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
| BUG-18 | NIP dosen dapat diubah setelah tersimpan | Medium | P3 | EXT-03 | Open — Terkonfirmasi (bug aktual) |
| BUG-19 | Nama file ekspor tanpa tanggal unduh | Low | P4 | TC-37, TC-38 | Closed — Tidak terjadi (Verified) |
| BUG-20 | jumlah_kelompok > jumlah dosen tidak dicegah | Medium | P3 | BVA-15, BVA-16 | Closed — Tidak terjadi (Verified) |
| BUG-21 | Validasi senyap: upload foto barang >2MB gagal tanpa pesan error (temuan baru, di luar checklist) | Medium (usulan) | P3 | BVA-12 | Open — Terkonfirmasi (bug aktual) |

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
| Langkah Reproduksi / Verifikasi | 1. Login sebagai Kasub Lab, buka Edit Profil<br>2. Catat NIP tersimpan<br>3. Ubah NIP ke nilai lain dan simpan |
| Expected Result | NIP tidak dapat diubah setelah tersimpan (aturan sistem) |
| Actual Result | NIP berubah dan tersimpan. Role: Kasub Lab; halaman: Edit Profil |
| Test Case Terkait | EXT-03 |
| Status | Open — Terkonfirmasi (bug aktual) |

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

### BUG-21 — Validasi senyap: upload foto barang >2MB gagal tanpa pesan error (temuan baru, di luar checklist)

| Field | Isi |
|---|---|
| Penguji | Rafid |
| Severity / Priority | Medium (usulan) / P3 |
| Langkah Reproduksi / Verifikasi | 1. Login Kasub Lab, buka Manajemen Inventaris<br>2. Tambah item dengan foto JPEG 2 MB + 1 KB<br>3. Simpan |
| Expected Result | Muncul pesan error 'maksimal 2MB' dan form dapat diperbaiki |
| Actual Result | Seluruh simpan ditolak (item tidak terbuat) dan user tidak melihat pesan error apa pun. Pembanding: pada upload avatar profil, avatar tidak berubah. |
| Test Case Terkait | BVA-12 |
| Status | Open — Terkonfirmasi (bug aktual) |

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
| Status | New / Open / In Progress / Fixed / Retest / Closed |
```
