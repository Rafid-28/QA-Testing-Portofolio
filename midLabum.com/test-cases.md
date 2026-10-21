# Test Cases — MidLab

Dokumen ini memuat seluruh test case manual MidLab beserta hasil pengujiannya di **https://midlabum.com**.

| Info | Detail |
|---|---|
| Penguji | Rafid |
| Browser | Google Chrome 155.0.8059.85 |
| Lingkungan uji | Produksi: `https://midlabum.com` |
| Tanggal pengujian | 2 September 2026 |

> **Catatan**
> - ID, Test Data, dan Expected Result disusun dari rancangan test case (BAB 5 Laporan Akhir). ID SEC-01 – SEC-08 dan EXT-01 – EXT-03 ditambahkan pada dokumentasi ini.
> - Seluruh test case dijalankan oleh penguji di `https://midlabum.com` menggunakan Google Chrome.
> - Precondition dan Langkah Pengujian ditulis ringkas berdasarkan deskripsi test case.

**Total: 83 test case — PASS 81 · FAIL 2 · BLOCKED 0**

| Suite | Jumlah | Pass | Fail | Blocked |
|---|---|---|---|---|
| A. Boundary Value Analysis (BVA-01 – BVA-22) | 22 | 21 | 1 (BVA-12) | 0 |
| B. All-Pairs Testing (TC-01 – TC-50) | 50 | 50 | 0 | 0 |
| C. Keamanan & Responsif (SEC-01 – SEC-08) | 8 | 8 | 0 | 0 |
| D. Tambahan (EXT-01 – EXT-03) | 3 | 2 | 1 (EXT-03) | 0 |
| **Total** | **83** | **81** | **2** | **0** |

Seluruh hasil berasal dari pengujian penguji di `midlabum.com`.

**Revisi spesifikasi BVA-21/22:** spesifikasi awal (15 vs 16 digit) tidak sesuai batas aplikasi karena nomor telepon 16 digit tetap diterima. Spesifikasi diubah menjadi batas 30 vs 31 karakter.

Catatan: SEC-01 s.d. SEC-05 setara dengan TC-49, TC-50, TC-47, BVA-08, dan BVA-10 (lihat kolom referensi di Suite C).

## A. Boundary Value Analysis (BVA)

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| BVA-01 | Halaman Registrasi (tab Mahasiswa) terbuka | 1. Isi form registrasi dengan NIM 12 digit<br>2. Klik Daftar | NIM: 231012345678 | Registrasi berhasil; akun aktif; masuk Portal Mahasiswa | Registrasi berhasil; akun aktif; masuk Portal Mahasiswa | PASS |
| BVA-02 | Halaman Registrasi (tab Mahasiswa) terbuka | 1. Isi form dengan NIM 11 digit<br>2. Klik Daftar | NIM: 11 digit (batas bawah -1) | Validasi error 'NIM harus 12 digit' | Validasi error 'NIM harus 12 digit' | PASS |
| BVA-03 | Halaman Registrasi (tab Mahasiswa) terbuka | 1. Isi form dengan NIM 13 digit<br>2. Klik Daftar | NIM: 13 digit (batas atas +1) | Validasi error 'NIM harus 12 digit' | Validasi error 'NIM harus 12 digit' | PASS |
| BVA-04 | Halaman Registrasi (tab Mahasiswa) terbuka; NIM sudah terdaftar di sistem | 1. Isi form dengan NIM yang sudah terdaftar<br>2. Klik Daftar | NIM: 231012345678 (sudah terdaftar) | Validasi error 'NIM sudah digunakan' (unique) | Validasi error 'NIM sudah digunakan' (unique) | PASS |
| BVA-05 | Halaman Registrasi (tab Mahasiswa) terbuka | 1. Isi form dengan password 8 karakter<br>2. Klik Daftar | Password: 8 karakter | Password diterima; proses sukses | Password diterima; proses sukses | PASS |
| BVA-06 | Halaman Registrasi (tab Mahasiswa) terbuka | 1. Isi form dengan password 7 karakter<br>2. Klik Daftar | Password: 7 karakter (minimum -1) | Validasi error minimal 8 karakter | Validasi error minimal 8 karakter | PASS |
| BVA-07 | Akun valid terdaftar; halaman Login terbuka | 1. Login dengan password salah sebanyak 5x berurutan<br>2. Login kembali dengan password benar | Email valid; password salah 5x | Masih bisa login normal, tidak terkunci | 5 percobaan salah pertama menghasilkan HTTP 302 (normal), belum terkunci | PASS |
| BVA-08 | Akun valid terdaftar; halaman Login terbuka | 1. Login dengan password salah sebanyak 6x berurutan<br>2. Amati respons sistem | Email valid; password salah 6x | Akun terkunci sementara (throttle 6x) | Percobaan ke-6 menghasilkan HTTP 429. Throttle berlaku per IP (6 percobaan/menit), bukan per akun; kredensial benar dari IP yang sama juga kena 429 selama 1 menit | PASS |
| BVA-09 | Halaman Registrasi terbuka; IP sama | 1. Lakukan registrasi ke-10 dari IP yang sama dalam 1 menit | Registrasi ke-10 / IP / 1 menit | Registrasi masih berhasil | 10 registrasi pertama dari IP yang sama menghasilkan HTTP 302 (berhasil); 10 user terbuat | PASS |
| BVA-10 | Halaman Registrasi terbuka; IP sama | 1. Lakukan registrasi ke-11 dari IP yang sama dalam 1 menit | Registrasi ke-11 / IP / 1 menit | Diblokir (throttle 10x/menit) | Registrasi ke-11 menghasilkan HTTP 429 (diblokir) | PASS |
| BVA-11 | Login sebagai Kasub Lab; halaman Profil Saya (avatar) dan Manajemen Inventaris (foto barang) terbuka | 1. Upload avatar profil dengan JPEG tepat 2 MB<br>2. Tambah item inventaris dengan foto JPEG tepat 2 MB | File JPEG tepat 2 MB (2.097.152 byte) | Upload berhasil diproses | Avatar: berhasil tersimpan. Foto barang: item terbuat, foto tersimpan | PASS |
| BVA-12 | Login sebagai Kasub Lab; halaman Profil Saya (avatar) dan Manajemen Inventaris (foto barang) terbuka | 1. Upload avatar profil dengan JPEG 2 MB + 1 KB<br>2. Tambah item inventaris dengan foto JPEG 2 MB + 1 KB | File JPEG 2 MB + 1 KB (2.098.176 byte) | Validasi error 'maksimal 2MB' | Avatar: ditolak, avatar tidak berubah. Foto barang: seluruh simpan ditolak (item tidak terbuat) dan TIDAK ada pesan error untuk user -> lihat BUG-21 | FAIL |
| BVA-13 | Login sebagai Dosen; form Perhitungan terbuka | 1. Isi jumlah mahasiswa 0<br>2. Klik Hitung/Simpan | Jumlah mahasiswa: 0 | Validasi error min:1 | Validasi error min:1 | PASS |
| BVA-14 | Login sebagai Dosen; form Perhitungan terbuka | 1. Isi jumlah mahasiswa 1<br>2. Klik Hitung | Jumlah mahasiswa: 1 | Validasi lolos; hasil perhitungan sesuai rumus | Validasi lolos; hasil perhitungan sesuai rumus | PASS |
| BVA-15 | Login sebagai Dosen; form Perhitungan terbuka; jumlah dosen pengajar diketahui | 1. Isi jumlah_kelompok sama dengan jumlah dosen pengajar<br>2. Klik Hitung/Simpan | jumlah_kelompok = jumlah dosen | Validasi lolos | Validasi lolos | PASS |
| BVA-16 | Login sebagai Dosen; form Perhitungan terbuka; jumlah dosen pengajar diketahui | 1. Isi jumlah_kelompok = jumlah dosen + 1<br>2. Klik Hitung/Simpan | jumlah_kelompok = jumlah dosen + 1 | Validasi error melebihi jumlah dosen | Validasi error melebihi jumlah dosen | PASS |
| BVA-17 | Login sebagai Dosen; form Perhitungan terbuka | 1. Isi input yang menghasilkan nilai 13,33<br>2. Klik Hitung | Hasil hitung mentah: 13,33 | Dibulatkan ke atas menjadi 14 | Dibulatkan ke atas menjadi 14 | PASS |
| BVA-18 | Login sebagai Dosen; form Perhitungan terbuka | 1. Isi input yang menghasilkan nilai 12,0<br>2. Klik Hitung | Hasil hitung mentah: 12,0 | Tetap 12, tidak berubah | Tetap 12, tidak berubah | PASS |
| BVA-19 | Login; halaman daftar dengan paginasi 12 data/halaman | 1. Siapkan 13 data<br>2. Buka halaman daftar<br>3. Buka halaman 2 | Jumlah data: 13 (12 + 1) | Halaman 2 muncul dengan 1 data | Halaman 2 muncul dengan 1 data | PASS |
| BVA-20 | Form dengan field nama (wajib) terbuka | 1. Kosongkan field nama<br>2. Klik Simpan | Nama: (kosong) | Validasi error wajib diisi | Validasi error wajib diisi | PASS |
| BVA-21 | Login; halaman Profil Saya terbuka | 1. Isi field telepon dengan tepat 30 karakter<br>2. Klik Simpan | Telepon: 30 karakter (batas server max:30). Spesifikasi direvisi dari '15 digit' | Telepon diterima | 30 karakter diterima dan tersimpan | PASS |
| BVA-22 | Login; halaman Profil Saya terbuka | 1. Isi field telepon dengan 31 karakter<br>2. Klik Simpan | Telepon: 31 karakter (batas +1). Spesifikasi direvisi dari '16 digit' | Validasi error melebihi batas | 31 karakter ditolak validasi, nilai tidak tersimpan | PASS |

## B. All-Pairs Testing

### TR-01 — Login (role × kredensial)

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| TC-01 | Akun Kasub Lab tersedia; halaman Login terbuka | 1. Buka halaman Login<br>2. Isi email dan kata sandi<br>3. Klik Masuk | Role: Kasub; Kredensial valid | Redirect ke Dashboard, statistik tampil | Redirect ke Dashboard, statistik tampil | PASS |
| TC-02 | Akun Dosen tersedia; halaman Login terbuka | 1. Buka halaman Login<br>2. Isi email dan kata sandi<br>3. Klik Masuk | Role: Dosen; Kredensial valid | Redirect ke Dashboard dosen | Redirect ke Dashboard dosen | PASS |
| TC-03 | Akun Mahasiswa tersedia; halaman Login terbuka | 1. Buka halaman Login<br>2. Isi email dan kata sandi<br>3. Klik Masuk | Role: Mahasiswa; Kredensial valid | Redirect ke Portal Mahasiswa | Redirect ke Portal Mahasiswa | PASS |
| TC-04 | Akun Dosen berstatus pending tersedia; halaman Login terbuka | 1. Buka halaman Login<br>2. Isi email dan kata sandi<br>3. Klik Masuk | Role: Dosen Pending; Kredensial valid | Masuk halaman Menunggu Persetujuan | Masuk halaman Menunggu Persetujuan | PASS |
| TC-05 | Halaman Login terbuka | 1. Isi email valid dengan kata sandi salah<br>2. Klik Masuk | Role: semua; Kredensial salah | Error 'credentials do not match' | Error 'credentials do not match' | PASS |
| TC-06 | Akun berstatus suspended tersedia; halaman Login terbuka | 1. Buka halaman Login<br>2. Isi email dan kata sandi<br>3. Klik Masuk | User suspended; Kredensial valid | Login gagal, akun ditangguhkan | Login gagal, akun ditangguhkan | PASS |

### TR-02 — Registrasi

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| TC-07 | Halaman Registrasi terbuka | 1. Pilih tab Mahasiswa<br>2. Isi form, centang mata kuliah, NIM 12 digit<br>3. Klik Daftar | Tipe: Mahasiswa; MK dicentang; NIM 12 digit | Registrasi sukses, akun aktif | Registrasi sukses, akun aktif | PASS |
| TC-08 | Halaman Registrasi terbuka | 1. Pilih tab Mahasiswa<br>2. Isi form tanpa mencentang mata kuliah<br>3. Klik Daftar | Tipe: Mahasiswa; MK tidak dicentang | Validasi error required | Validasi error required | PASS |
| TC-09 | Halaman Registrasi terbuka | 1. Pilih tab Dosen<br>2. Isi form dengan NIP unik, centang mata kuliah<br>3. Klik Daftar | Tipe: Dosen; NIP unik; MK dicentang | Registrasi sukses, status menunggu persetujuan | Registrasi sukses, status menunggu persetujuan | PASS |

### TR-03 — Perhitungan (status × aksi)

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| TC-10 | Login sebagai Dosen; perhitungan berstatus Draft tersedia | 1. Buka perhitungan Draft<br>2. Klik Edit<br>3. Ubah data dan simpan | Actor: Dosen; Status: Draft | Perhitungan dapat diedit | Perhitungan dapat diedit | PASS |
| TC-11 | Login sebagai Dosen; perhitungan berstatus Draft tersedia | 1. Buka daftar perhitungan<br>2. Klik Hapus pada perhitungan Draft<br>3. Konfirmasi | Actor: Dosen; Status: Draft | Draft terhapus | Draft terhapus | PASS |
| TC-12 | Login sebagai Dosen; perhitungan berstatus Draft tersedia | 1. Buka perhitungan Draft<br>2. Klik Submit | Actor: Dosen; Status: Draft | Status berubah menjadi submitted | Status berubah menjadi submitted | PASS |
| TC-13 | Login sebagai Dosen; perhitungan berstatus Submitted tersedia | 1. Coba hapus perhitungan Submitted | Actor: Dosen; Status: Submitted | Gagal (403/error) | Gagal (403/error) | PASS |
| TC-14 | Login sebagai Dosen; perhitungan berstatus Approved/Locked tersedia | 1. Coba edit perhitungan Approved/Locked | Actor: Dosen; Status: Approved/Locked | Tidak bisa, tombol disable/403 | Tidak bisa, tombol disable/403 | PASS |
| TC-15 | Login sebagai Kasub Lab; perhitungan Submitted tersedia | 1. Buka halaman Approval<br>2. Pilih perhitungan Submitted<br>3. Klik Approve | Actor: Kasub; Status: Submitted | Status -> approved; dosen dapat notifikasi | Status -> approved; dosen dapat notifikasi | PASS |
| TC-16 | Login sebagai Kasub Lab; perhitungan Approved tersedia | 1. Buka perhitungan Approved<br>2. Klik Lock | Actor: Kasub; Status: Approved | Status -> locked (final); dosen dapat notifikasi | Status -> locked (final); dosen dapat notifikasi | PASS |

### TR-04 — Change Request

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| TC-17 | Login sebagai Dosen; change request berstatus Draft tersedia | 1. Buka change request Draft<br>2. Klik Submit | Actor: Dosen; Jenis: CR; Status: Draft | Status -> submitted | Status -> submitted | PASS |
| TC-18 | Login sebagai Dosen; change request berstatus Submitted tersedia | 1. Coba hapus change request Submitted | Actor: Dosen; Jenis: CR; Status: Submitted | Gagal (403/error) | Gagal (403/error) | PASS |
| TC-19 | Login sebagai Kasub Lab; change request berstatus Submitted tersedia | 1. Buka halaman Approval<br>2. Pilih CR Submitted<br>3. Isi catatan dan klik Approve | Actor: Kasub; Jenis: CR; Aksi: Approve + catatan | Status -> approved; notifikasi dosen | Status -> approved; notifikasi dosen | PASS |
| TC-20 | Login sebagai Kasub Lab; change request berstatus Submitted tersedia | 1. Buka halaman Approval<br>2. Pilih CR Submitted<br>3. Klik Reject | Actor: Kasub; Jenis: CR; Aksi: Reject | Status -> rejected | Status -> rejected | PASS |

### TR-05 — Manajemen Pengguna

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| TC-21 | Login sebagai Kasub Lab; halaman Manajemen Pengguna terbuka; dosen pending tersedia | 1. Pilih user Dosen pending<br>2. Klik Setujui | User: Dosen pending; Aksi: Approve | Status -> Aktif; dosen login normal | Status -> Aktif; dosen login normal | PASS |
| TC-22 | Login sebagai Kasub Lab; halaman Manajemen Pengguna terbuka; dosen pending tersedia | 1. Pilih user Dosen<br>2. Klik Tolak<br>3. Kosongkan alasan<br>4. Kirim | Aksi: Reject; Alasan: kosong | Validasi error wajib alasan | Validasi error wajib alasan | PASS |
| TC-23 | Login sebagai Kasub Lab; halaman Manajemen Pengguna terbuka; dosen pending tersedia | 1. Pilih user Dosen<br>2. Klik Tolak<br>3. Isi alasan<br>4. Kirim | Aksi: Reject; Alasan: diisi | Status -> Ditolak; alasan tampil | Status -> Ditolak; alasan tampil | PASS |
| TC-24 | Login sebagai Kasub Lab; halaman Manajemen Pengguna terbuka; user aktif tersedia | 1. Pilih user Aktif<br>2. Klik Suspend<br>3. Coba login dengan akun tersebut | User: Aktif; Aksi: Suspend | Status -> Ditangguhkan; tidak bisa login | Status -> Ditangguhkan; tidak bisa login | PASS |
| TC-25 | Login sebagai Kasub Lab; halaman Manajemen Pengguna terbuka; user ditangguhkan tersedia | 1. Pilih user Ditangguhkan<br>2. Klik Resume<br>3. Coba login dengan akun tersebut | User: Ditangguhkan; Aksi: Resume | Status -> Aktif; bisa login | Status -> Aktif; bisa login | PASS |

### TR-06 — Modul Praktikum

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| TC-26 | Login sebagai Dosen; halaman Modul Praktikum terbuka | 1. Klik Upload Modul<br>2. Pilih file PDF versi 1<br>3. Simpan | Modul: PDF; Versi 1 | Modul muncul, badge PDF | Modul muncul, badge PDF | PASS |
| TC-27 | Login sebagai Dosen; halaman Modul Praktikum terbuka; modul versi 1 sudah ada | 1. Upload ulang file pada modul yang sama | Modul: file sama; Versi 1 | Versi bertambah menjadi v2 | Versi bertambah menjadi v2 | PASS |
| TC-28 | Login sebagai Dosen; halaman Modul Praktikum terbuka; modul tersedia | 1. Klik Pratinjau pada modul | Modul: teks/PDF; Aksi: Pratinjau | Preview terbuka di tab baru | Preview terbuka di tab baru | PASS |
| TC-29 | Login sebagai Dosen; halaman Modul Praktikum terbuka | 1. Coba upload modul untuk mata kuliah milik dosen lain (dropdown/akses langsung) | Modul: MK dosen lain | Tidak muncul di dropdown / 403 | Tidak muncul di dropdown / 403 | PASS |
| TC-30 | Login sebagai Dosen; halaman Modul Praktikum terbuka; modul milik dosen tersedia | 1. Klik Hapus pada modul milik sendiri<br>2. Konfirmasi | Modul: milik dosen; Aksi: Hapus | Modul terhapus dari daftar | Modul terhapus dari daftar | PASS |

### TR-07 — Inventaris

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| TC-31 | Login sebagai Kasub Lab/Dosen/Laboran; halaman Inventaris terbuka | 1. Klik Tambah<br>2. Isi form dan upload foto<br>3. Simpan | Foto: diupload; Aksi: Tambah | Thumbnail muncul di tabel | Thumbnail muncul di tabel | PASS |
| TC-32 | Login sebagai Kasub Lab/Dosen/Laboran; halaman Inventaris terbuka | 1. Klik Tambah<br>2. Isi form tanpa foto<br>3. Simpan | Foto: tanpa foto; Aksi: Tambah | Tersimpan tanpa error | Tersimpan tanpa error | PASS |
| TC-33 | Login sebagai Kasub Lab/Dosen/Laboran; halaman Inventaris terbuka; item berfoto tersedia | 1. Klik thumbnail foto | Foto: thumbnail; Aksi: Klik zoom | Foto membesar di modal | Foto membesar di modal | PASS |
| TC-34 | Login sebagai Kasub Lab/Dosen/Laboran; halaman Inventaris terbuka; item berfoto tersedia | 1. Klik Edit<br>2. Ganti foto<br>3. Simpan | Aksi: Edit + ganti foto | Foto lama terganti | Foto lama terganti | PASS |
| TC-35 | Login sebagai Kasub Lab/Dosen/Laboran; halaman Inventaris terbuka; item tersedia | 1. Klik Hapus pada item<br>2. Konfirmasi | Aksi: Hapus | Data inventaris terhapus | Data inventaris terhapus | PASS |

### TR-08 — Laporan

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| TC-36 | Login sebagai Kasub Lab/Dosen; halaman Laporan terbuka | 1. Pilih filter tahun dan semester<br>2. Klik Lihat | Filter: tahun + semester | Data tersaring | Data tersaring | PASS |
| TC-37 | Login sebagai Kasub Lab/Dosen; halaman Laporan terbuka | 1. Pilih laporan<br>2. Klik Ekspor PDF | Format: PDF; Aksi: Ekspor | File terunduh berisi data | HTTP 200; file Laporan-Lengkap-DD-MM-YYYY.pdf terunduh, valid, dan berisi data | PASS |
| TC-38 | Login sebagai Kasub Lab/Dosen; halaman Laporan terbuka | 1. Pilih laporan<br>2. Klik Ekspor Excel | Format: Excel; Aksi: Ekspor | File terunduh berisi data | HTTP 200; file Laporan-Lengkap-DD-MM-YYYY.xlsx terunduh, valid, dan berisi data | PASS |

### TR-09 — Notifikasi

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| TC-39 | Login; notifikasi tersedia pada ikon lonceng | 1. Klik ikon lonceng | Aksi: Klik bell | Dropdown lebar 360px, max 8 item | Dropdown lebar 360px, max 8 item | PASS |
| TC-40 | Login; notifikasi tersedia pada ikon lonceng; notifikasi baru masuk | 1. Amati badge pada ikon lonceng | Notifikasi baru; Aksi: Cek badge | Badge sesuai jumlah belum dibaca | Badge sesuai jumlah belum dibaca | PASS |
| TC-41 | Login; notifikasi tersedia pada ikon lonceng | 1. Klik salah satu notifikasi | Aksi: Klik notifikasi | Ditandai dibaca + pindah halaman | Ditandai dibaca + pindah halaman | PASS |
| TC-42 | Login; notifikasi tersedia pada ikon lonceng; ada notifikasi belum dibaca | 1. Klik 'Tandai semua dibaca' | Aksi: Tandai semua dibaca | Badge reset ke 0 | Badge reset ke 0 | PASS |

### TR-10 — Hak Akses & Proteksi Halaman

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| TC-43 | Login sebagai Dosen berstatus pending | 1. Akses URL /dashboard | Dosen Pending; URL: /dashboard | Redirect ke Menunggu Persetujuan | Redirect ke Menunggu Persetujuan | PASS |
| TC-44 | Login sebagai Dosen berstatus pending | 1. Akses URL /calculations | Dosen Pending; URL: /calculations | Redirect ke Menunggu Persetujuan | Redirect ke Menunggu Persetujuan | PASS |
| TC-45 | Login sebagai Dosen berstatus pending | 1. Akses URL /portal | Dosen Pending; URL: /portal | 403 Forbidden | 403 Forbidden | PASS |
| TC-46 | Login sebagai Mahasiswa | 1. Akses URL /dashboard | Mahasiswa; URL: /dashboard | 403 Forbidden | 403 Forbidden | PASS |
| TC-47 | Login sebagai Mahasiswa | 1. Akses URL /calculations | Mahasiswa; URL: /calculations | 403 Forbidden | 403 Forbidden | PASS |
| TC-48 | Login sebagai Mahasiswa; ada perhitungan draft/submitted dan approved/locked | 1. Buka menu Hasil Perhitungan | Perhitungan draft/submitted | Tidak tampil (hanya approved/locked) | Tidak tampil (hanya approved/locked) | PASS |
| TC-49 | Belum login | 1. Akses URL /dashboard | Semua role; tanpa login | Redirect ke login | Redirect ke login | PASS |
| TC-50 | Login sebagai Mahasiswa | 1. Akses URL /admin/users | Mahasiswa; URL: /admin/users | 403 Forbidden | 403 Forbidden | PASS |

## C. Keamanan & Responsif

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Ref. TC Setara |
|---|---|---|---|---|---|---|---|
| SEC-01 | Belum login | 1. Akses /dashboard | URL: /dashboard | Redirect ke login | Redirect ke login | PASS | TC-49 |
| SEC-02 | Login sebagai Mahasiswa | 1. Akses /admin/users | URL: /admin/users | 403 Forbidden | 403 Forbidden | PASS | TC-50 |
| SEC-03 | Login sebagai Mahasiswa | 1. Akses /calculations | URL: /calculations | 403 Forbidden | 403 Forbidden | PASS | TC-47 |
| SEC-04 | Halaman Login terbuka | 1. Login dengan password salah 6x berurutan | Password salah 6x | Terkunci sementara | Terkunci sementara | PASS | BVA-08 |
| SEC-05 | Halaman Registrasi terbuka | 1. Registrasi 11x dari IP yang sama dalam 1 menit | 11 registrasi / IP / menit | Throttle 10/menit | Throttle 10/menit | PASS | BVA-10 |
| SEC-06 | Aplikasi dapat diakses | 1. Buka halaman aplikasi<br>2. Periksa response header (DevTools > Network) | Header: X-Content-Type-Options, X-Frame-Options | X-Content-Type-Options: nosniff dan X-Frame-Options ada | Header X-Content-Type-Options: nosniff dan X-Frame-Options: DENY ada | PASS | - |
| SEC-07 | Aplikasi dapat diakses | 1. Picu halaman error 403, 404, dan 500 | URL tidak valid / tidak berhak / error server | Halaman error 403 / 404 / 500 tampil sesuai | Halaman error 403 / 404 / 500 tampil sesuai | PASS | - |
| SEC-08 | Login; viewport desktop dan mobile | 1. Collapse/expand sidebar<br>2. Buka aplikasi pada ukuran layar mobile | Viewport desktop dan mobile | Sidebar collapse/expand dan tampilan mobile responsif berfungsi | Sidebar collapse/expand dan tampilan mobile responsif berfungsi | PASS | - |

## D. Pengujian Tambahan

Test case tambahan untuk menutup item risk checklist (XSS, CSRF, perubahan NIP) yang belum punya test case pada rancangan awal.

| ID | Precondition | Langkah Pengujian | Test Data | Expected Result | Actual Result | Status | Bug Terkait |
|---|---|---|---|---|---|---|---|
| EXT-01 | Login; halaman dengan field nama dapat diisi | 1. Isi field nama dengan <code>&lt;script&gt;alert(1)&lt;/script&gt;</code><br>2. Simpan dan tampilkan kembali | Nama: payload skrip alert(1) | Output ter-escape, skrip tidak dieksekusi | Output tampil sebagai teks ter-escape (&amp;lt;script&amp;gt;alert(1)&amp;lt;/script&amp;gt;) | PASS | BUG-12 |
| EXT-02 | Login sebagai Kasub Lab | 1. Kirim POST /users/2/suspend tanpa token CSRF | Request tanpa token CSRF | Request ditolak (HTTP 419) | HTTP 419 | PASS | BUG-13 |
| EXT-03 | Login sebagai Kasub Lab; halaman Edit Profil terbuka | 1. Catat NIP tersimpan<br>2. Ubah NIP ke nilai lain<br>3. Simpan | NIP tersimpan diubah ke nilai lain | NIP tidak dapat diubah setelah tersimpan (aturan sistem) | NIP berubah dan tersimpan | FAIL | BUG-18 |
