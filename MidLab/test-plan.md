# Test Plan — MidLab

> Penguji: Rafid · Browser: Google Chrome 155.0.8059.85 · Lingkungan uji: `http://127.0.0.1:8000` (lokal) · Tanggal: 22 Oktober 2026.

## 1. Tujuan Pengujian

1. Memastikan fungsi-fungsi utama MidLab berjalan sesuai kebutuhan, terutama perhitungan kebutuhan alat/BHP dengan pembulatan ke atas (ceil).
2. Memverifikasi validasi input pada nilai batas (NIM, password, ukuran file, jumlah mahasiswa, jumlah kelompok, nomor telepon, paginasi).
3. Memverifikasi kombinasi role, status, dan aksi (login, registrasi, perhitungan, change request, manajemen user, modul, inventaris, laporan, notifikasi, hak akses).
4. Memastikan kontrol akses dan proteksi dasar (redirect, 403, throttling, header keamanan, halaman error, responsif) bekerja.
5. Menelusuri 18 kemungkinan bug dari daftar BUG-01 s.d. BUG-20 yang diidentifikasi sebelum pengujian (BUG-11 dan BUG-17 dikeluarkan dari scope).

## 2. Scope

### Dalam Scope

| Area | Cakupan |
|---|---|
| Autentikasi & registrasi | Login semua role, akun pending/suspended, registrasi mahasiswa/dosen, validasi NIM/NIP, throttling |
| Manajemen pengguna | Setujui, tolak (dengan alasan), suspend, resume |
| Perhitungan | Rumus BHP/alat, ceil, draft/submit/approve/lock, hak edit/hapus per status |
| Change request & approval | Submit, approve (dengan catatan), reject, hapus |
| Modul praktikum | Upload PDF, versi, pratinjau, hapus, pembatasan per mata kuliah |
| Inventaris | Tambah/edit/hapus, foto, lightbox |
| Laporan | Filter tahun/semester, ekspor PDF/Excel |
| Notifikasi | Dropdown, badge, tandai dibaca |
| Hak akses | Pembatasan halaman per role dan status akun |
| Keamanan & responsif | Header keamanan, halaman error, sidebar, tampilan mobile, XSS, CSRF, perubahan NIP |

### Di Luar Scope

- Pengujian performa, beban, dan stres
- Pengujian otomatis (unit/integration/E2E)
- Penetration test dan scan kerentanan mendalam (XSS dan CSRF hanya diuji dasar pada EXT-01 dan EXT-02)
- Pengujian fungsional pada produksi (`midlabum.com`)
- Pengujian fungsional audit log dan backup database
- Backup database saat `exec()` nonaktif (BUG-11) dan perilaku stok terhadap status aktif/inaktif barang (BUG-17); keduanya ada di Tabel 5.1 laporan tetapi dikeluarkan dari scope
- Kompatibilitas lintas browser/perangkat secara menyeluruh

## 3. Jenis Testing

| Jenis | Teknik | Cakupan |
|---|---|---|
| Functional testing (black-box) | Boundary Value Analysis (BVA) | 22 test case (BVA-01 – BVA-22) |
| Functional testing (black-box) | All-Pairs Testing | 50 test case (TC-01 – TC-50), dikelompokkan TR-01 – TR-10 |
| Security testing (dasar) | Pemeriksaan akses, throttling, header | 8 pemeriksaan (SEC-01 – SEC-08) |
| Responsive/UI testing (dasar) | Sidebar collapse/expand, mobile view | SEC-08 |
| Pengujian tambahan | XSS, CSRF, perubahan NIP | 3 test case (EXT-01 – EXT-03) |
| Risk-based review | Daftar kemungkinan bug Tabel 5.1 | BUG-01 – BUG-20 |

## 4. Environment Pengujian

| Aspek | Detail |
|---|---|
| Lingkungan uji | Pengembangan (lokal), `http://127.0.0.1:8000` |
| PHP / Framework | PHP 8.2.12 / Laravel 12 |
| Database | MySQL |
| Browser | Google Chrome 155.0.8059.85; viewport desktop 1440x900 dan mobile 390x844 |
| Bukti | Screenshot pengujian 22 Oktober 2026 (`evidence/screenshots/`) |
| Lingkungan produksi (tidak diuji) | `https://midlabum.com`, Hostinger; `exec()` nonaktif, backup memakai fallback PDO |

**Akun uji:** Kasub Lab, Dosen (aktif), Dosen (pending), Dosen (suspended), dan Mahasiswa, semuanya berdomain `lab.test`.

**Data uji:** mata kuliah dan keterampilan praktikum beserta item kebutuhan, perhitungan pada status draft/submitted/approved/locked, change request, file JPEG uji berukuran tepat 2 MB dan 2 MB + 1 KB, serta data berawalan "QA" yang dibuat selama pengujian.

## 5. Pendekatan Pengujian

1. **Identifikasi risiko.** Sebelum eksekusi, tim menyusun 20 kemungkinan bug berikut severity-nya (Tabel 5.1).
2. **Desain test case.** BVA untuk input bernilai batas; All-Pairs untuk kombinasi role × status × aksi.
3. **Eksekusi.** Setiap test case dijalankan pada lingkungan lokal tanggal 22 Oktober 2026, lalu Actual Result dibandingkan dengan Expected Result. Pengambilan bukti memakai skrip browser (headless Chromium).
4. **Penentuan status.**
   - **PASS:** actual sesuai expected.
   - **FAIL:** actual tidak sesuai expected → dicatat di `bug-reports.md` (sebagai bug, atau sebagai temuan yang perlu keputusan bila perilaku berbeda dari rancangan awal).
   - **BLOCKED:** test tidak dapat dijalankan karena prasyarat/lingkungan terhambat.
5. **Pelaporan.** Hasil dicatat pada `test-cases.md`, bug pada `bug-reports.md`, dan rekap pada `test-summary.md`.

### Kriteria Keberhasilan

- Seluruh test case berstatus PASS, atau setiap FAIL telah memiliki bug report.
- Tidak ada bug berstatus Open dengan severity Critical/High.

## 6. Peran & Jadwal

| Item | Keterangan |
|---|---|
| Penguji | Rafid |
| Tanggal pengujian | 22 Oktober 2026 |

## 7. Deliverable

`test-plan.md`, `test-cases.md`, `bug-reports.md`, `test-summary.md`, `README.md`, dan folder `evidence/`.
