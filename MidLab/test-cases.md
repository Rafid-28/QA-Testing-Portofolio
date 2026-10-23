# Test Cases — MidLab

Dokumen ini memuat seluruh test case MidLab beserta hasil pengujiannya pada lingkungan lokal.

| Info | Detail |
|---|---|
| Penguji | Rafid |
| Browser | Google Chrome 155.0.8059.85 (screenshot diambil dengan headless Chromium) |
| Lingkungan uji | Pengembangan (lokal): `http://127.0.0.1:8000` |
| Tanggal pengujian | 22 Oktober 2026 |
| Screenshot | `evidence/screenshots/` |

> **Catatan**
> - ID, Test Data, dan Expected Result disusun dari rancangan test case (BAB 5 Laporan Akhir). ID SEC-01 – SEC-08 dan EXT-01 – EXT-03 ditambahkan pada dokumentasi ini.
> - Actual Result dan Status mengikuti hasil pengujian 22 Oktober 2026.
> - Pengujian dijalankan di lingkungan lokal; belum dilakukan di `midlabum.com`.
> - Precondition dan Langkah Pengujian ditulis ringkas berdasarkan deskripsi test case.
> - Bukti: screenshot di `evidence/screenshots/`; sebagian berupa gambar bukti dengan caption hasil.

**Total: 83 test case — PASS 78 · FAIL 5 · BLOCKED 0**

| Suite | Jumlah | Pass | Fail | Blocked |
|---|---|---|---|---|
| A. Boundary Value Analysis (BVA-01 – BVA-22) | 22 | 20 | 2 (BVA-12, BVA-13) | 0 |
| B. All-Pairs Testing (TC-01 – TC-50) | 50 | 47 | 3 (TC-08, TC-10, TC-34) | 0 |
| C. Keamanan & Responsif (SEC-01 – SEC-08) | 8 | 8 | 0 | 0 |
| D. Tambahan (EXT-01 – EXT-03) | 3 | 3 | 0 | 0 |
| **Total** | **83** | **78** | **5** | **0** |

**Kelima test case FAIL:** BVA-12 (BUG-21), BVA-13 (BUG-22), TC-08 (BUG-23), TC-10 (BUG-24), dan TC-34 (BUG-25). Pada TC-08, TC-10, dan TC-34, perilaku aplikasi berbeda dari rancangan awal; perlu keputusan apakah itu bug atau perubahan spesifikasi.

**Revisi spesifikasi BVA-21/22:** spesifikasi awal (15 vs 16 digit) tidak sesuai batas aplikasi karena nomor telepon 16 digit tetap diterima. Spesifikasi diubah menjadi batas 30 vs 31 karakter.

Catatan: SEC-01 s.d. SEC-05 setara dengan TC-49, TC-50, TC-47, BVA-08, dan BVA-10 (lihat kolom referensi di Suite C).

## A. Boundary Value Analysis (BVA)

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti |
|---|---|---|---|---|---|---|---|
| BVA-01 | Halaman Registrasi (tab Mahasiswa) terbuka | 1. Isi form registrasi dengan NIM 12 digit<br>2. Klik Daftar | NIM: 231012345678 | Registrasi berhasil; akun aktif; masuk Portal Mahasiswa | Registrasi berhasil dan akun aktif; diarahkan ke halaman Verifikasi Email (kode 6 digit) | PASS | <a href="evidence/screenshots/BVA-01.png"><img src="evidence/screenshots/BVA-01.png" width="220" alt="BVA-01.png"></a> |
| BVA-02 | Halaman Registrasi (tab Mahasiswa) terbuka | 1. Isi form dengan NIM 11 digit<br>2. Klik Daftar | NIM: 11 digit (batas bawah -1) | Validasi error 'NIM harus 12 digit' | Validasi error 'NIM harus 12 digit' | PASS | <a href="evidence/screenshots/BVA-02.png"><img src="evidence/screenshots/BVA-02.png" width="220" alt="BVA-02.png"></a> |
| BVA-03 | Halaman Registrasi (tab Mahasiswa) terbuka | 1. Isi form dengan NIM 13 digit<br>2. Klik Daftar | NIM: 13 digit (batas atas +1) | Validasi error 'NIM harus 12 digit' | Validasi error 'NIM harus 12 digit' | PASS | <a href="evidence/screenshots/BVA-03.png"><img src="evidence/screenshots/BVA-03.png" width="220" alt="BVA-03.png"></a> |
| BVA-04 | Halaman Registrasi (tab Mahasiswa) terbuka; NIM sudah terdaftar di sistem | 1. Isi form dengan NIM yang sudah terdaftar<br>2. Klik Daftar | NIM: 231012345678 (sudah terdaftar) | Validasi error 'NIM sudah digunakan' (unique) | Validasi error 'NIM sudah digunakan' (unique) | PASS | <a href="evidence/screenshots/BVA-04.png"><img src="evidence/screenshots/BVA-04.png" width="220" alt="BVA-04.png"></a> |
| BVA-05 | Halaman Registrasi (tab Mahasiswa) terbuka | 1. Isi form dengan password 8 karakter<br>2. Klik Daftar | Password: 8 karakter | Password diterima; proses sukses | Password diterima; proses sukses | PASS | <a href="evidence/screenshots/BVA-05.png"><img src="evidence/screenshots/BVA-05.png" width="220" alt="BVA-05.png"></a> |
| BVA-06 | Halaman Registrasi (tab Mahasiswa) terbuka | 1. Isi form dengan password 7 karakter<br>2. Klik Daftar | Password: 7 karakter (minimum -1) | Validasi error minimal 8 karakter | Validasi error minimal 8 karakter | PASS | <a href="evidence/screenshots/BVA-06.png"><img src="evidence/screenshots/BVA-06.png" width="220" alt="BVA-06.png"></a> |
| BVA-07 | Akun valid terdaftar; halaman Login terbuka | 1. Login dengan password salah sebanyak 5x berurutan<br>2. Login kembali dengan password benar | Email valid; password salah 5x | Masih bisa login normal, tidak terkunci | 5 percobaan salah menghasilkan HTTP 302 kembali ke /login dengan pesan 'These credentials do not match our records'; belum terkunci | PASS | <a href="evidence/screenshots/BVA-07.png"><img src="evidence/screenshots/BVA-07.png" width="220" alt="BVA-07.png"></a> |
| BVA-08 | Akun valid terdaftar; halaman Login terbuka | 1. Login dengan password salah sebanyak 6x berurutan<br>2. Amati respons sistem | Email valid; password salah 6x | Akun terkunci sementara (throttle 6x) | Percobaan ke-6 menghasilkan HTTP 429. Throttle 5 percobaan/menit per IP (bukan per akun); kredensial benar dari IP yang sama juga kena 429 selama window aktif | PASS | <a href="evidence/screenshots/BVA-08.png"><img src="evidence/screenshots/BVA-08.png" width="220" alt="BVA-08.png"></a> |
| BVA-09 | Halaman Registrasi terbuka; IP sama | 1. Lakukan registrasi ke-10 dari IP yang sama dalam 1 menit | Registrasi ke-10 / IP / 1 menit | Registrasi masih berhasil | 10 registrasi pertama menghasilkan HTTP 302 (berhasil, diarahkan ke verifikasi); 10 akun mahasiswa terbuat dan muncul di Manajemen Pengguna | PASS | <a href="evidence/screenshots/BVA-09.png"><img src="evidence/screenshots/BVA-09.png" width="220" alt="BVA-09.png"></a> |
| BVA-10 | Halaman Registrasi terbuka; IP sama | 1. Lakukan registrasi ke-11 dari IP yang sama dalam 1 menit | Registrasi ke-11 / IP / 1 menit | Diblokir (throttle 10x/menit) | Registrasi ke-11 menghasilkan HTTP 429 (middleware throttle 10/menit); tidak ada user ke-11 | PASS | <a href="evidence/screenshots/BVA-10.png"><img src="evidence/screenshots/BVA-10.png" width="220" alt="BVA-10.png"></a> |
| BVA-11 | Login sebagai Kasub Lab; halaman Profil Saya (avatar) dan Manajemen Inventaris (foto barang) terbuka | 1. Upload avatar profil dengan JPEG tepat 2 MB<br>2. Tambah item inventaris dengan foto JPEG tepat 2 MB | File JPEG tepat 2 MB (2.097.152 byte) | Upload berhasil diproses | Avatar: upload diterima, avatar tersimpan. Foto barang: item terbuat (BRG-ALATUTAMA-001), foto tampil di tabel | PASS | <a href="evidence/screenshots/BVA-11_avatar.png"><img src="evidence/screenshots/BVA-11_avatar.png" width="220" alt="BVA-11_avatar.png"></a><br><a href="evidence/screenshots/BVA-11_foto-barang.png"><img src="evidence/screenshots/BVA-11_foto-barang.png" width="220" alt="BVA-11_foto-barang.png"></a> |
| BVA-12 | Login sebagai Kasub Lab; halaman Profil Saya (avatar) dan Manajemen Inventaris (foto barang) terbuka | 1. Upload avatar profil dengan JPEG 2 MB + 1 KB<br>2. Tambah item inventaris dengan foto JPEG 2 MB + 1 KB | File JPEG 2 MB + 1 KB (2.098.176 byte) | Validasi error 'maksimal 2MB' | Avatar: alert browser 'Ukuran file melebihi 2 MB', file dikosongkan, avatar tidak berubah. Foto barang: item tidak terbuat; pesan 'The foto field must not be greater than 2048 kilobytes' hanya ada di dalam modal Tambah Barang yang tertutup, sehingga user tidak melihat pesan -> lihat BUG-21 | FAIL | <a href="evidence/screenshots/BVA-12_avatar.png"><img src="evidence/screenshots/BVA-12_avatar.png" width="220" alt="BVA-12_avatar.png"></a><br><a href="evidence/screenshots/BVA-12_foto-barang.png"><img src="evidence/screenshots/BVA-12_foto-barang.png" width="220" alt="BVA-12_foto-barang.png"></a> |
| BVA-13 | Login sebagai Dosen; form Perhitungan terbuka | 1. Isi jumlah mahasiswa 0<br>2. Klik Hitung/Simpan | Jumlah mahasiswa: 0 | Validasi error min:1 | Tidak ada error: perhitungan CAL-2026-0009 (draft) tersimpan dengan jumlah mahasiswa 0 (aturan validasi min:0, bukan min:1) -> lihat BUG-22 | FAIL | <a href="evidence/screenshots/BVA-13.png"><img src="evidence/screenshots/BVA-13.png" width="220" alt="BVA-13.png"></a> |
| BVA-14 | Login sebagai Dosen; form Perhitungan terbuka | 1. Isi jumlah mahasiswa 1<br>2. Klik Hitung | Jumlah mahasiswa: 1 | Validasi lolos; hasil perhitungan sesuai rumus | Validasi lolos; hasil sesuai rumus (CAL-2026-0010, total 1) | PASS | <a href="evidence/screenshots/BVA-14.png"><img src="evidence/screenshots/BVA-14.png" width="220" alt="BVA-14.png"></a> |
| BVA-15 | Login sebagai Dosen; form Perhitungan terbuka; jumlah dosen pengajar diketahui | 1. Isi jumlah_kelompok sama dengan jumlah dosen pengajar<br>2. Klik Hitung/Simpan | jumlah_kelompok = jumlah dosen | Validasi lolos | Validasi lolos | PASS | <a href="evidence/screenshots/BVA-15.png"><img src="evidence/screenshots/BVA-15.png" width="220" alt="BVA-15.png"></a> |
| BVA-16 | Login sebagai Dosen; form Perhitungan terbuka; jumlah dosen pengajar diketahui | 1. Isi jumlah_kelompok = jumlah dosen + 1<br>2. Klik Hitung/Simpan | jumlah_kelompok = jumlah dosen + 1 | Validasi error melebihi jumlah dosen | Pesan: 'Jumlah kelompok (3) melebihi jumlah dosen pengampu (2). Maksimal 2 kelompok.' | PASS | <a href="evidence/screenshots/BVA-16.png"><img src="evidence/screenshots/BVA-16.png" width="220" alt="BVA-16.png"></a> |
| BVA-17 | Login sebagai Dosen; form Perhitungan terbuka | 1. Isi input yang menghasilkan nilai 13,33<br>2. Klik Hitung | Hasil hitung mentah: 13,33 | Dibulatkan ke atas menjadi 14 | Nilai mentah 13,33 dibulatkan ke atas menjadi 14 (total kebutuhan); kolom pembelajaran menampilkan 13 tanpa desimal | PASS | <a href="evidence/screenshots/BVA-17.png"><img src="evidence/screenshots/BVA-17.png" width="220" alt="BVA-17.png"></a> |
| BVA-18 | Login sebagai Dosen; form Perhitungan terbuka | 1. Isi input yang menghasilkan nilai 12,0<br>2. Klik Hitung | Hasil hitung mentah: 12,0 | Tetap 12, tidak berubah | 12,0 tetap 12 | PASS | <a href="evidence/screenshots/BVA-18.png"><img src="evidence/screenshots/BVA-18.png" width="220" alt="BVA-18.png"></a> |
| BVA-19 | Login; halaman daftar dengan paginasi 12 data/halaman | 1. Siapkan 13 data<br>2. Buka halaman daftar<br>3. Buka halaman 2 | Jumlah data: 13 (12 + 1) | Halaman 2 muncul dengan 1 data | Halaman 2 muncul dengan 1 data | PASS | <a href="evidence/screenshots/BVA-19.png"><img src="evidence/screenshots/BVA-19.png" width="220" alt="BVA-19.png"></a> |
| BVA-20 | Form dengan field nama (wajib) terbuka | 1. Kosongkan field nama<br>2. Klik Simpan | Nama: (kosong) | Validasi error wajib diisi | Validasi error wajib diisi | PASS | <a href="evidence/screenshots/BVA-20.png"><img src="evidence/screenshots/BVA-20.png" width="220" alt="BVA-20.png"></a> |
| BVA-21 | Login; halaman Profil Saya terbuka | 1. Isi field telepon dengan tepat 30 karakter<br>2. Klik Simpan | Telepon: 30 karakter (batas server max:30). Spesifikasi direvisi dari '15 digit' | Telepon diterima | 30 karakter diterima dan tersimpan (validasi server max:30) | PASS | <a href="evidence/screenshots/BVA-21.png"><img src="evidence/screenshots/BVA-21.png" width="220" alt="BVA-21.png"></a> |
| BVA-22 | Login; halaman Profil Saya terbuka | 1. Isi field telepon dengan 31 karakter<br>2. Klik Simpan | Telepon: 31 karakter (batas +1). Spesifikasi direvisi dari '16 digit' | Validasi error melebihi batas | 31 karakter ditolak validasi, nilai tidak tersimpan | PASS | <a href="evidence/screenshots/BVA-22.png"><img src="evidence/screenshots/BVA-22.png" width="220" alt="BVA-22.png"></a> |

## B. All-Pairs Testing

### TR-01 — Login (role × kredensial)

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti |
|---|---|---|---|---|---|---|---|
| TC-01 | Akun Kasub Lab tersedia; halaman Login terbuka | 1. Buka halaman Login<br>2. Isi email dan kata sandi<br>3. Klik Masuk | Role: Kasub; Kredensial valid | Redirect ke Dashboard, statistik tampil | Redirect ke Dashboard, statistik tampil | PASS | <a href="evidence/screenshots/TC-01.png"><img src="evidence/screenshots/TC-01.png" width="220" alt="TC-01.png"></a> |
| TC-02 | Akun Dosen tersedia; halaman Login terbuka | 1. Buka halaman Login<br>2. Isi email dan kata sandi<br>3. Klik Masuk | Role: Dosen; Kredensial valid | Redirect ke Dashboard dosen | Redirect ke Dashboard dosen | PASS | <a href="evidence/screenshots/TC-02.png"><img src="evidence/screenshots/TC-02.png" width="220" alt="TC-02.png"></a> |
| TC-03 | Akun Mahasiswa tersedia; halaman Login terbuka | 1. Buka halaman Login<br>2. Isi email dan kata sandi<br>3. Klik Masuk | Role: Mahasiswa; Kredensial valid | Redirect ke Portal Mahasiswa | Kredensial valid, redirect ke /portal (Portal Mahasiswa) | PASS | <a href="evidence/screenshots/TC-03.png"><img src="evidence/screenshots/TC-03.png" width="220" alt="TC-03.png"></a> |
| TC-04 | Akun Dosen berstatus pending tersedia; halaman Login terbuka | 1. Buka halaman Login<br>2. Isi email dan kata sandi<br>3. Klik Masuk | Role: Dosen Pending; Kredensial valid | Masuk halaman Menunggu Persetujuan | Masuk halaman Menunggu Persetujuan | PASS | <a href="evidence/screenshots/TC-04.png"><img src="evidence/screenshots/TC-04.png" width="220" alt="TC-04.png"></a> |
| TC-05 | Halaman Login terbuka | 1. Isi email valid dengan kata sandi salah<br>2. Klik Masuk | Role: semua; Kredensial salah | Error 'credentials do not match' | Error 'credentials do not match' | PASS | <a href="evidence/screenshots/TC-05.png"><img src="evidence/screenshots/TC-05.png" width="220" alt="TC-05.png"></a> |
| TC-06 | Akun berstatus suspended tersedia; halaman Login terbuka | 1. Buka halaman Login<br>2. Isi email dan kata sandi<br>3. Klik Masuk | User suspended; Kredensial valid | Login gagal, akun ditangguhkan | Login gagal, akun ditangguhkan | PASS | <a href="evidence/screenshots/TC-06.png"><img src="evidence/screenshots/TC-06.png" width="220" alt="TC-06.png"></a> |

### TR-02 — Registrasi

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti |
|---|---|---|---|---|---|---|---|
| TC-07 | Halaman Registrasi terbuka | 1. Pilih tab Mahasiswa<br>2. Isi form, centang mata kuliah, NIM 12 digit<br>3. Klik Daftar | Tipe: Mahasiswa; MK dicentang; NIM 12 digit | Registrasi sukses, akun aktif | Registrasi berhasil, akun aktif; diarahkan ke halaman Verifikasi Email | PASS | <a href="evidence/screenshots/TC-07.png"><img src="evidence/screenshots/TC-07.png" width="220" alt="TC-07.png"></a> |
| TC-08 | Halaman Registrasi terbuka | 1. Pilih tab Mahasiswa<br>2. Isi form tanpa mencentang mata kuliah<br>3. Klik Daftar | Tipe: Mahasiswa; MK tidak dicentang | Validasi error required | Form registrasi tidak memiliki field mata kuliah sehingga tidak ada validasi required; registrasi tetap berhasil (diarahkan ke /verify-email/code) -> lihat BUG-23 | FAIL | <a href="evidence/screenshots/TC-08.png"><img src="evidence/screenshots/TC-08.png" width="220" alt="TC-08.png"></a> |
| TC-09 | Halaman Registrasi terbuka | 1. Pilih tab Dosen<br>2. Isi form dengan NIP unik, centang mata kuliah<br>3. Klik Daftar | Tipe: Dosen; NIP unik; MK dicentang | Registrasi sukses, status menunggu persetujuan | Registrasi sukses, status menunggu persetujuan | PASS | <a href="evidence/screenshots/TC-09.png"><img src="evidence/screenshots/TC-09.png" width="220" alt="TC-09.png"></a> |

### TR-03 — Perhitungan (status × aksi)

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti |
|---|---|---|---|---|---|---|---|
| TC-10 | Login sebagai Dosen; perhitungan berstatus Draft tersedia | 1. Buka perhitungan Draft<br>2. Klik Edit<br>3. Ubah data dan simpan | Actor: Dosen; Status: Draft | Perhitungan dapat diedit | Tidak ada form edit: halaman detail perhitungan Draft hanya menyediakan Hitung Ulang, Submit, dan Hapus -> lihat BUG-24 | FAIL | <a href="evidence/screenshots/TC-10.png"><img src="evidence/screenshots/TC-10.png" width="220" alt="TC-10.png"></a> |
| TC-11 | Login sebagai Dosen; perhitungan berstatus Draft tersedia | 1. Buka daftar perhitungan<br>2. Klik Hapus pada perhitungan Draft<br>3. Konfirmasi | Actor: Dosen; Status: Draft | Draft terhapus | Draft terhapus | PASS | <a href="evidence/screenshots/TC-11.png"><img src="evidence/screenshots/TC-11.png" width="220" alt="TC-11.png"></a> |
| TC-12 | Login sebagai Dosen; perhitungan berstatus Draft tersedia | 1. Buka perhitungan Draft<br>2. Klik Submit | Actor: Dosen; Status: Draft | Status berubah menjadi submitted | Status berubah dari draft menjadi submitted; badge Menunggu Persetujuan | PASS | <a href="evidence/screenshots/TC-12.png"><img src="evidence/screenshots/TC-12.png" width="220" alt="TC-12.png"></a> |
| TC-13 | Login sebagai Dosen; perhitungan berstatus Submitted tersedia | 1. Coba hapus perhitungan Submitted | Actor: Dosen; Status: Submitted | Gagal (403/error) | HTTP 403 Forbidden; perhitungan tidak dihapus | PASS | <a href="evidence/screenshots/TC-13.png"><img src="evidence/screenshots/TC-13.png" width="220" alt="TC-13.png"></a> |
| TC-14 | Login sebagai Dosen; perhitungan berstatus Approved/Locked tersedia | 1. Coba edit perhitungan Approved/Locked | Actor: Dosen; Status: Approved/Locked | Tidak bisa, tombol disable/403 | PATCH /calculations/2/recalculate dipaksa: 403 Forbidden; halaman perhitungan approved tidak menampilkan Edit/Hitung Ulang/Hapus | PASS | <a href="evidence/screenshots/TC-14.png"><img src="evidence/screenshots/TC-14.png" width="220" alt="TC-14.png"></a> |
| TC-15 | Login sebagai Kasub Lab; perhitungan Submitted tersedia | 1. Buka halaman Approval<br>2. Pilih perhitungan Submitted<br>3. Klik Approve | Actor: Kasub; Status: Submitted | Status -> approved; dosen dapat notifikasi | Status -> approved; dosen dapat notifikasi | PASS | <a href="evidence/screenshots/TC-15.png"><img src="evidence/screenshots/TC-15.png" width="220" alt="TC-15.png"></a> |
| TC-16 | Login sebagai Kasub Lab; perhitungan Approved tersedia | 1. Buka perhitungan Approved<br>2. Klik Lock | Actor: Kasub; Status: Approved | Status -> locked (final); dosen dapat notifikasi | Status approved -> locked; notifikasi dikirim ke pembuat | PASS | <a href="evidence/screenshots/TC-16.png"><img src="evidence/screenshots/TC-16.png" width="220" alt="TC-16.png"></a> |

### TR-04 — Change Request

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti |
|---|---|---|---|---|---|---|---|
| TC-17 | Login sebagai Dosen; change request berstatus Draft tersedia | 1. Buka change request Draft<br>2. Klik Submit | Actor: Dosen; Jenis: CR; Status: Draft | Status -> submitted | Status draft -> submitted; menunggu review Kasub Lab | PASS | <a href="evidence/screenshots/TC-17.png"><img src="evidence/screenshots/TC-17.png" width="220" alt="TC-17.png"></a> |
| TC-18 | Login sebagai Dosen; change request berstatus Submitted tersedia | 1. Coba hapus change request Submitted | Actor: Dosen; Jenis: CR; Status: Submitted | Gagal (403/error) | HTTP 403 Forbidden; change request tidak dihapus | PASS | <a href="evidence/screenshots/TC-18.png"><img src="evidence/screenshots/TC-18.png" width="220" alt="TC-18.png"></a> |
| TC-19 | Login sebagai Kasub Lab; change request berstatus Submitted tersedia | 1. Buka halaman Approval<br>2. Pilih CR Submitted<br>3. Isi catatan dan klik Approve | Actor: Kasub; Jenis: CR; Aksi: Approve + catatan | Status -> approved; notifikasi dosen | Status submitted -> approved; catatan review tersimpan dan notifikasi dikirim. Tidak ada halaman Approval terpisah; review dilakukan di detail change request | PASS | <a href="evidence/screenshots/TC-19.png"><img src="evidence/screenshots/TC-19.png" width="220" alt="TC-19.png"></a> |
| TC-20 | Login sebagai Kasub Lab; change request berstatus Submitted tersedia | 1. Buka halaman Approval<br>2. Pilih CR Submitted<br>3. Klik Reject | Actor: Kasub; Jenis: CR; Aksi: Reject | Status -> rejected | Status submitted -> rejected; notifikasi dikirim. Review dilakukan di detail change request | PASS | <a href="evidence/screenshots/TC-20.png"><img src="evidence/screenshots/TC-20.png" width="220" alt="TC-20.png"></a> |

### TR-05 — Manajemen Pengguna

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti |
|---|---|---|---|---|---|---|---|
| TC-21 | Login sebagai Kasub Lab; halaman Manajemen Pengguna terbuka; dosen pending tersedia | 1. Pilih user Dosen pending<br>2. Klik Setujui | User: Dosen pending; Aksi: Approve | Status -> Aktif; dosen login normal | Status -> Aktif; dosen login normal | PASS | <a href="evidence/screenshots/TC-21.png"><img src="evidence/screenshots/TC-21.png" width="220" alt="TC-21.png"></a> |
| TC-22 | Login sebagai Kasub Lab; halaman Manajemen Pengguna terbuka; dosen pending tersedia | 1. Pilih user Dosen<br>2. Klik Tolak<br>3. Kosongkan alasan<br>4. Kirim | Aksi: Reject; Alasan: kosong | Validasi error wajib alasan | Pesan validasi 'The Alasan penolakan field is required'; status user tetap pending | PASS | <a href="evidence/screenshots/TC-22.png"><img src="evidence/screenshots/TC-22.png" width="220" alt="TC-22.png"></a> |
| TC-23 | Login sebagai Kasub Lab; halaman Manajemen Pengguna terbuka; dosen pending tersedia | 1. Pilih user Dosen<br>2. Klik Tolak<br>3. Isi alasan<br>4. Kirim | Aksi: Reject; Alasan: diisi | Status -> Ditolak; alasan tampil | Status -> Ditolak; alasan tampil | PASS | <a href="evidence/screenshots/TC-23.png"><img src="evidence/screenshots/TC-23.png" width="220" alt="TC-23.png"></a> |
| TC-24 | Login sebagai Kasub Lab; halaman Manajemen Pengguna terbuka; user aktif tersedia | 1. Pilih user Aktif<br>2. Klik Suspend<br>3. Coba login dengan akun tersebut | User: Aktif; Aksi: Suspend | Status -> Ditangguhkan; tidak bisa login | Status -> Ditangguhkan; tidak bisa login | PASS | <a href="evidence/screenshots/TC-24.png"><img src="evidence/screenshots/TC-24.png" width="220" alt="TC-24.png"></a> |
| TC-25 | Login sebagai Kasub Lab; halaman Manajemen Pengguna terbuka; user ditangguhkan tersedia | 1. Pilih user Ditangguhkan<br>2. Klik Resume<br>3. Coba login dengan akun tersebut | User: Ditangguhkan; Aksi: Resume | Status -> Aktif; bisa login | Status -> Aktif; bisa login | PASS | <a href="evidence/screenshots/TC-25.png"><img src="evidence/screenshots/TC-25.png" width="220" alt="TC-25.png"></a> |

### TR-06 — Modul Praktikum

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti |
|---|---|---|---|---|---|---|---|
| TC-26 | Login sebagai Dosen; halaman Modul Praktikum terbuka | 1. Klik Upload Modul<br>2. Pilih file PDF versi 1<br>3. Simpan | Modul: PDF; Versi 1 | Modul muncul, badge PDF | Modul muncul, badge PDF | PASS | <a href="evidence/screenshots/TC-26.png"><img src="evidence/screenshots/TC-26.png" width="220" alt="TC-26.png"></a> |
| TC-27 | Login sebagai Dosen; halaman Modul Praktikum terbuka; modul versi 1 sudah ada | 1. Upload ulang file pada modul yang sama | Modul: file sama; Versi 1 | Versi bertambah menjadi v2 | Versi 1 -> 2; riwayat versi bertambah | PASS | <a href="evidence/screenshots/TC-27.png"><img src="evidence/screenshots/TC-27.png" width="220" alt="TC-27.png"></a> |
| TC-28 | Login sebagai Dosen; halaman Modul Praktikum terbuka; modul tersedia | 1. Klik Pratinjau pada modul | Modul: teks/PDF; Aksi: Pratinjau | Preview terbuka di tab baru | Pratinjau terbuka di tab baru (/practical-modules/6/preview); stream PDF HTTP 200 application/pdf | PASS | <a href="evidence/screenshots/TC-28.png"><img src="evidence/screenshots/TC-28.png" width="220" alt="TC-28.png"></a> |
| TC-29 | Login sebagai Dosen; halaman Modul Praktikum terbuka | 1. Coba upload modul untuk mata kuliah milik dosen lain (dropdown/akses langsung) | Modul: MK dosen lain | Tidak muncul di dropdown / 403 | POST upload modul mata kuliah lain: 403 Forbidden; mata kuliah tersebut tidak muncul di halaman modul dosen; tidak ada modul baru | PASS | <a href="evidence/screenshots/TC-29.png"><img src="evidence/screenshots/TC-29.png" width="220" alt="TC-29.png"></a> |
| TC-30 | Login sebagai Dosen; halaman Modul Praktikum terbuka; modul milik dosen tersedia | 1. Klik Hapus pada modul milik sendiri<br>2. Konfirmasi | Modul: milik dosen; Aksi: Hapus | Modul terhapus dari daftar | Modul terhapus dari daftar | PASS | <a href="evidence/screenshots/TC-30.png"><img src="evidence/screenshots/TC-30.png" width="220" alt="TC-30.png"></a> |

### TR-07 — Inventaris

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti |
|---|---|---|---|---|---|---|---|
| TC-31 | Login sebagai Kasub Lab/Dosen/Laboran; halaman Inventaris terbuka | 1. Klik Tambah<br>2. Isi form dan upload foto<br>3. Simpan | Foto: diupload; Aksi: Tambah | Thumbnail muncul di tabel | Thumbnail muncul di tabel | PASS | <a href="evidence/screenshots/TC-31.png"><img src="evidence/screenshots/TC-31.png" width="220" alt="TC-31.png"></a> |
| TC-32 | Login sebagai Kasub Lab/Dosen/Laboran; halaman Inventaris terbuka | 1. Klik Tambah<br>2. Isi form tanpa foto<br>3. Simpan | Foto: tanpa foto; Aksi: Tambah | Tersimpan tanpa error | Tersimpan tanpa error | PASS | <a href="evidence/screenshots/TC-32.png"><img src="evidence/screenshots/TC-32.png" width="220" alt="TC-32.png"></a> |
| TC-33 | Login sebagai Kasub Lab/Dosen/Laboran; halaman Inventaris terbuka; item berfoto tersedia | 1. Klik thumbnail foto | Foto: thumbnail; Aksi: Klik zoom | Foto membesar di modal | Klik thumbnail membuka modal #fotoModal berisi foto ukuran besar | PASS | <a href="evidence/screenshots/TC-33.png"><img src="evidence/screenshots/TC-33.png" width="220" alt="TC-33.png"></a> |
| TC-34 | Login sebagai Kasub Lab/Dosen/Laboran; halaman Inventaris terbuka; item berfoto tersedia | 1. Klik Edit<br>2. Ganti foto<br>3. Simpan | Aksi: Edit + ganti foto | Foto lama terganti | Form edit inventaris tidak menyediakan field foto; foto dikelola dari master barang -> lihat BUG-25 | FAIL | <a href="evidence/screenshots/TC-34.png"><img src="evidence/screenshots/TC-34.png" width="220" alt="TC-34.png"></a> |
| TC-35 | Login sebagai Kasub Lab/Dosen/Laboran; halaman Inventaris terbuka; item tersedia | 1. Klik Hapus pada item<br>2. Konfirmasi | Aksi: Hapus | Data inventaris terhapus | Data inventaris terhapus | PASS | <a href="evidence/screenshots/TC-35.png"><img src="evidence/screenshots/TC-35.png" width="220" alt="TC-35.png"></a> |

### TR-08 — Laporan

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti |
|---|---|---|---|---|---|---|---|
| TC-36 | Login sebagai Kasub Lab/Dosen; halaman Laporan terbuka | 1. Pilih filter tahun dan semester<br>2. Klik Lihat | Filter: tahun + semester | Data tersaring | Data tersaring | PASS | <a href="evidence/screenshots/TC-36.png"><img src="evidence/screenshots/TC-36.png" width="220" alt="TC-36.png"></a> |
| TC-37 | Login sebagai Kasub Lab/Dosen; halaman Laporan terbuka | 1. Pilih laporan<br>2. Klik Ekspor PDF | Format: PDF; Aksi: Ekspor | File terunduh berisi data | HTTP 200; file Laporan-Lengkap-22-10-2026.pdf (887.411 byte) terunduh, valid, dan berisi data | PASS | <a href="evidence/screenshots/TC-37_ekspor.png"><img src="evidence/screenshots/TC-37_ekspor.png" width="220" alt="TC-37_ekspor.png"></a><br><a href="evidence/screenshots/TC-37_file.png"><img src="evidence/screenshots/TC-37_file.png" width="220" alt="TC-37_file.png"></a> |
| TC-38 | Login sebagai Kasub Lab/Dosen; halaman Laporan terbuka | 1. Pilih laporan<br>2. Klik Ekspor Excel | Format: Excel; Aksi: Ekspor | File terunduh berisi data | HTTP 200; file Laporan-Lengkap-22-10-2026.xlsx (14.379 byte) terunduh, valid, dan berisi data | PASS | <a href="evidence/screenshots/TC-38_ekspor.png"><img src="evidence/screenshots/TC-38_ekspor.png" width="220" alt="TC-38_ekspor.png"></a><br><a href="evidence/screenshots/TC-38_file.png"><img src="evidence/screenshots/TC-38_file.png" width="220" alt="TC-38_file.png"></a> |

### TR-09 — Notifikasi

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti |
|---|---|---|---|---|---|---|---|
| TC-39 | Login; notifikasi tersedia pada ikon lonceng | 1. Klik ikon lonceng | Aksi: Klik bell | Dropdown lebar 360px, max 8 item | Dropdown lebar 360px menampilkan 8 item dari 9 notifikasi | PASS | <a href="evidence/screenshots/TC-39.png"><img src="evidence/screenshots/TC-39.png" width="220" alt="TC-39.png"></a> |
| TC-40 | Login; notifikasi tersedia pada ikon lonceng; notifikasi baru masuk | 1. Amati badge pada ikon lonceng | Notifikasi baru; Aksi: Cek badge | Badge sesuai jumlah belum dibaca | Badge menampilkan jumlah belum dibaca (9) | PASS | <a href="evidence/screenshots/TC-40.png"><img src="evidence/screenshots/TC-40.png" width="220" alt="TC-40.png"></a> |
| TC-41 | Login; notifikasi tersedia pada ikon lonceng | 1. Klik salah satu notifikasi | Aksi: Klik notifikasi | Ditandai dibaca + pindah halaman | Notifikasi ditandai dibaca (badge 9 -> 8) dan pindah ke /calculations/1 | PASS | <a href="evidence/screenshots/TC-41.png"><img src="evidence/screenshots/TC-41.png" width="220" alt="TC-41.png"></a> |
| TC-42 | Login; notifikasi tersedia pada ikon lonceng; ada notifikasi belum dibaca | 1. Klik 'Tandai semua dibaca' | Aksi: Tandai semua dibaca | Badge reset ke 0 | Badge reset ke 0 | PASS | <a href="evidence/screenshots/TC-42.png"><img src="evidence/screenshots/TC-42.png" width="220" alt="TC-42.png"></a> |

### TR-10 — Hak Akses & Proteksi Halaman

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti |
|---|---|---|---|---|---|---|---|
| TC-43 | Login sebagai Dosen berstatus pending | 1. Akses URL /dashboard | Dosen Pending; URL: /dashboard | Redirect ke Menunggu Persetujuan | Redirect ke Menunggu Persetujuan | PASS | <a href="evidence/screenshots/TC-43.png"><img src="evidence/screenshots/TC-43.png" width="220" alt="TC-43.png"></a> |
| TC-44 | Login sebagai Dosen berstatus pending | 1. Akses URL /calculations | Dosen Pending; URL: /calculations | Redirect ke Menunggu Persetujuan | Redirect ke Menunggu Persetujuan | PASS | <a href="evidence/screenshots/TC-44.png"><img src="evidence/screenshots/TC-44.png" width="220" alt="TC-44.png"></a> |
| TC-45 | Login sebagai Dosen berstatus pending | 1. Akses URL /portal | Dosen Pending; URL: /portal | 403 Forbidden | 403 Forbidden | PASS | <a href="evidence/screenshots/TC-45.png"><img src="evidence/screenshots/TC-45.png" width="220" alt="TC-45.png"></a> |
| TC-46 | Login sebagai Mahasiswa | 1. Akses URL /dashboard | Mahasiswa; URL: /dashboard | 403 Forbidden | 403 Forbidden | PASS | <a href="evidence/screenshots/TC-46.png"><img src="evidence/screenshots/TC-46.png" width="220" alt="TC-46.png"></a> |
| TC-47 | Login sebagai Mahasiswa | 1. Akses URL /calculations | Mahasiswa; URL: /calculations | 403 Forbidden | 403 Forbidden | PASS | <a href="evidence/screenshots/TC-47.png"><img src="evidence/screenshots/TC-47.png" width="220" alt="TC-47.png"></a> |
| TC-48 | Login sebagai Mahasiswa; ada perhitungan draft/submitted dan approved/locked | 1. Buka menu Hasil Perhitungan | Perhitungan draft/submitted | Tidak tampil (hanya approved/locked) | Tidak tampil (hanya approved/locked) | PASS | <a href="evidence/screenshots/TC-48.png"><img src="evidence/screenshots/TC-48.png" width="220" alt="TC-48.png"></a> |
| TC-49 | Belum login | 1. Akses URL /dashboard | Semua role; tanpa login | Redirect ke login | Redirect ke login | PASS | <a href="evidence/screenshots/TC-49.png"><img src="evidence/screenshots/TC-49.png" width="220" alt="TC-49.png"></a> |
| TC-50 | Login sebagai Mahasiswa | 1. Akses URL /admin/users | Mahasiswa; URL: /admin/users | 403 Forbidden | 403 Forbidden | PASS | <a href="evidence/screenshots/TC-50.png"><img src="evidence/screenshots/TC-50.png" width="220" alt="TC-50.png"></a> |

## C. Keamanan & Responsif

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti | Ref. TC Setara |
|---|---|---|---|---|---|---|---|---|
| SEC-01 | Belum login | 1. Akses /dashboard | URL: /dashboard | Redirect ke login | Redirect ke login | PASS | <a href="evidence/screenshots/SEC-01.png"><img src="evidence/screenshots/SEC-01.png" width="220" alt="SEC-01.png"></a> | TC-49 |
| SEC-02 | Login sebagai Mahasiswa | 1. Akses /admin/users | URL: /admin/users | 403 Forbidden | 403 Forbidden | PASS | <a href="evidence/screenshots/SEC-02.png"><img src="evidence/screenshots/SEC-02.png" width="220" alt="SEC-02.png"></a> | TC-50 |
| SEC-03 | Login sebagai Mahasiswa | 1. Akses /calculations | URL: /calculations | 403 Forbidden | 403 Forbidden | PASS | <a href="evidence/screenshots/SEC-03.png"><img src="evidence/screenshots/SEC-03.png" width="220" alt="SEC-03.png"></a> | TC-47 |
| SEC-04 | Halaman Login terbuka | 1. Login dengan password salah 6x berurutan | Password salah 6x | Terkunci sementara | Terkunci sementara | PASS | <a href="evidence/screenshots/SEC-04.png"><img src="evidence/screenshots/SEC-04.png" width="220" alt="SEC-04.png"></a> | BVA-08 |
| SEC-05 | Halaman Registrasi terbuka | 1. Registrasi 11x dari IP yang sama dalam 1 menit | 11 registrasi / IP / menit | Throttle 10/menit | Throttle 10/menit | PASS | <a href="evidence/screenshots/SEC-05.png"><img src="evidence/screenshots/SEC-05.png" width="220" alt="SEC-05.png"></a> | BVA-10 |
| SEC-06 | Aplikasi dapat diakses | 1. Buka halaman aplikasi<br>2. Periksa response header (DevTools > Network) | Header: X-Content-Type-Options, X-Frame-Options | X-Content-Type-Options: nosniff dan X-Frame-Options ada | Header X-Content-Type-Options: nosniff, X-Frame-Options: DENY, Referrer-Policy, dan Permissions-Policy ada. Strict-Transport-Security dan Content-Security-Policy tidak ada (lokal) | PASS | evidence/SEC-06-headers-production.txt<br><a href="evidence/screenshots/SEC-06.png"><img src="evidence/screenshots/SEC-06.png" width="220" alt="SEC-06.png"></a> | - |
| SEC-07 | Aplikasi dapat diakses | 1. Picu halaman error 403, 404, dan 500 | URL tidak valid / tidak berhak / error server | Halaman error 403 / 404 / 500 tampil sesuai | 404, 403, dan 500 tampil dengan halaman error aplikasi. Halaman 500 dipicu dari server terpisah (port 8001, APP_DEBUG=false) lewat unduhan backup yang filenya tidak ada | PASS | <a href="evidence/screenshots/SEC-07.png"><img src="evidence/screenshots/SEC-07.png" width="220" alt="SEC-07.png"></a><br><a href="evidence/screenshots/SEC-07-404.png"><img src="evidence/screenshots/SEC-07-404.png" width="220" alt="SEC-07-404.png"></a> | - |
| SEC-08 | Login; viewport desktop dan mobile | 1. Collapse/expand sidebar<br>2. Buka aplikasi pada ukuran layar mobile | Viewport desktop dan mobile | Sidebar collapse/expand dan tampilan mobile responsif berfungsi | Desktop 1440x900: sidebar terbuka 248px dan collapsed 64px. Mobile 390x844: sidebar tampil sebagai overlay dengan backdrop | PASS | <a href="evidence/screenshots/SEC-08.png"><img src="evidence/screenshots/SEC-08.png" width="220" alt="SEC-08.png"></a> | - |

## D. Pengujian Tambahan

Test case tambahan untuk menutup item risk checklist (XSS, CSRF, perubahan NIP) yang belum punya test case pada rancangan awal.

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bukti | Bug Terkait |
|---|---|---|---|---|---|---|---|---|
| EXT-01 | Login sebagai Kasub Lab; halaman Profil Pengguna terbuka | 1. Isi field Nama Lengkap dengan <code>&lt;script&gt;alert(1)&lt;/script&gt;</code><br>2. Simpan dan lihat kembali halaman | Nama: payload skrip alert(1) | Output ter-escape, skrip tidak dieksekusi | Teks tampil ter-escape (bukan elemen script); dialog alert yang muncul: 0 | PASS | <a href="evidence/screenshots/EXT-01.png"><img src="evidence/screenshots/EXT-01.png" width="220" alt="EXT-01.png"></a> | BUG-12 |
| EXT-02 | Login sebagai Kasub Lab | 1. Kirim POST /users/2/suspend tanpa token CSRF | Request tanpa token CSRF | Request ditolak (HTTP 419) | HTTP 419 Page Expired; status dosen tetap active | PASS | <a href="evidence/screenshots/EXT-02.png"><img src="evidence/screenshots/EXT-02.png" width="220" alt="EXT-02.png"></a> | BUG-13 |
| EXT-03 | Login sebagai Kasub Lab; halaman Edit Profil terbuka | 1. Catat NIP tersimpan<br>2. Coba ubah NIP (lewat form dan POST langsung)<br>3. Simpan | NIP tersimpan 198001012005011001; percobaan ubah ke 199901012005011009 | NIP tidak dapat diubah setelah tersimpan (aturan sistem) | NIP tidak berubah: field NIP disabled dan server mengabaikan perubahan | PASS | <a href="evidence/screenshots/EXT-03.png"><img src="evidence/screenshots/EXT-03.png" width="220" alt="EXT-03.png"></a> | BUG-18 |
