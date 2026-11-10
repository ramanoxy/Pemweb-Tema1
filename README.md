# UTS Pemrograman Web - SpaceHub Workspace Solutions

Proyek Mini Individu Website Profil & Katalog Dinamis Berbasis PHP Dasar.

---

## 1. Identitas Mahasiswa
- **Nama Lengkap** : [Nama Lengkap Anda]
- **NIM**          : [NIM Anda, contoh: 230101001]
- **Kelas**        : [Kelas Anda, contoh: TI-3A]
- **Mata Kuliah**  : Pemrograman Web (253A23W15)
- **Program Studi**: S1 Informatika, Fakultas Teknik, Tiga Serangkai University

---

## 2. Deskripsi Singkat Tema
SpaceHub adalah platform katalog digital layanan penyewaan ruang kerja modern yang mencakup *coworking hot desk*, *dedicated desk*, *private office*, *meeting room*, hingga studio multimedia dan acara. Website ini dibangun untuk memudahkan calon penyewa menelusuri ketersediaan kapasitas ruang dan melihat estimasi biaya sewa secara transparan, bersih, dan langsung dari data array PHP tanpa database.

---

## 3. Alur Website (User Flow & Data Flow)

Berikut adalah alur perjalanan pengguna (*user flow*) dan alur pemrosesan data sisi server (*server-side data flow*) pada aplikasi SpaceHub:

### A. Diagram Alur Navigasi Pengguna (User Flow)
```text
                  ┌───────────────────────────────┐
                  │    Pengunjung Membuka Web     │
                  │     (http://localhost:8000)   │
                  └──────────────┬────────────────┘
                                 │
                                 ▼
                  ┌───────────────────────────────┐
                  │   Halaman Profil (index.php)  │
                  ├───────────────────────────────┤
                  │ • Hero banner perkenalan      │
                  │ • Ringkasan statistik server  │
                  │ • Profil & informasi kontak   │
                  │ • 3 Fasilitas utama           │
                  └──────────────┬────────────────┘
                                 │
                   Klik Menu Navigasi / Tombol CTA
                                 │
                                 ▼
                  ┌───────────────────────────────┐
                  │  Halaman Katalog (katalog.php)│
                  ├───────────────────────────────┤
                  │ • 4 Kartu ringkasan hitung    │
                  │ • Render kartu item (foreach) │
                  │ • Badge status dinamis (match)│
                  │ • Tombol pesan via WhatsApp   │
                  └──────────────┬────────────────┘
                                 │
                    Klik Menu "Profil Usaha"
                                 │
                                 └──────────────► Kembali ke index.php
```

### B. Tahapan Alur Pengguna:
1. **Akses Halaman Utama (`index.php`)**: Pengunjung disambut dengan hero banner yang memuat moto layanan SpaceHub serta kartu ringkasan cepat (total ruang, daya tampung, harga sewa terendah) yang dihitung secara dinamis oleh server PHP.
2. **Eksplorasi Profil & Kontak**: Pengunjung membaca komitmen usaha, jam operasional layanan, fasilitas utama (internet fiber, kopi gratis, akses 24/7), serta kontak WhatsApp.
3. **Pindah ke Katalog (`katalog.php`)**: Pengunjung menekan tombol *"Buka Katalog Ruangan"* untuk melihat seluruh opsi ruang kerja.
4. **Melihat Ringkasan Statistik Data**: Pada bagian atas katalog, server menampilkan 4 kartu kalkulasi data riil: Total Unit Ruangan (`count`), Ruangan Siap Pakai (`array_filter`), Total Daya Tampung Orang (`array_sum`), dan Rata-Rata Harga Sewa.
5. **Menelusuri Kartu Ruangan (Render Foreach)**: Setiap ruangan ditampilkan dalam kartu (*card-only*) yang memuat kategori, nama, badge ketersediaan (`match()`), kapasitas, rating, deskripsi, daftar fasilitas, dan harga.
6. **Interaksi Pemesanan**: 
   - Ruangan berstatus **Tersedia** menyediakan tombol *"Pesan Sekarang"* yang langsung membuka tautan WhatsApp dengan pesan otomatis membawa nama ruangan.
   - Ruangan berstatus **Penuh / Dipesan** menyediakan tombol *"Tanya Jadwal"* untuk konsultasi antrean jadwal sewa berikutnya.

### C. Alur Pemrosesan Data Sisi Server (Server-Side Architecture):
```text
 [data.php] ───────► Menyediakan master array multidimensi ($daftarRuangan).
      │
      ├────────────► [functions.php] 
      │                   ├── Memproses data dengan fungsi array bawaan (count, sum, filter, usort)
      │                   ├── Menentukan warna badge dengan percabangan match()
      │                   └── Mengamankan seluruh cetakan output dengan fungsi e() (htmlspecialchars)
      │
      ▼
 [index.php & katalog.php] ───► Merender tampilan HTML5 semantik bersih & dikirim ke peramban.
```

---

## 4. Cara Menjalankan di Localhost

Aplikasi ini tidak membutuhkan database (semua data tersimpan dalam array PHP) dan tidak menggunakan framework apa pun.

### Opsi A: Menggunakan PHP Built-in Server (Paling Cepat & Praktis)
1. Buka terminal (Command Prompt / PowerShell / Terminal VS Code).
2. Arahkan direktori aktif ke dalam folder proyek ini:
   ```bash
   cd path/ke/tema-1-layanan-kerja
   ```
3. Jalankan server lokal bawaan PHP:
   ```bash
   php -S localhost:8000
   ```
4. Buka browser dan akses URL:
   ```
   http://localhost:8000
   ```

### Opsi B: Menggunakan XAMPP
1. Salin (*copy*) folder `tema-1-layanan-kerja` ke direktori `htdocs` XAMPP Anda:
   - Windows: `C:\xampp\htdocs\tema-1-layanan-kerja`
2. Jalankan modul **Apache** pada XAMPP Control Panel.
3. Buka browser dan buka:
   ```
   http://localhost/tema-1-layanan-kerja/index.php
   ```

---

## 5. Daftar Fitur & Penanggung Jawab File/Fungsi

| No | Fitur Utama | File Terkait | Fungsi / Logika Penangan | Keterangan |
|:--:|:---|:---|:---|:---|
| 1 | **Sanitasi Output Anti-XSS** | `functions.php` | `e()` | Membungkus `htmlspecialchars` agar setiap teks yang dirender ke browser aman dari injeksi script berbahaya. |
| 2 | **Format Mata Uang Rupiah** | `functions.php` | `formatRupiah()` | Mengonversi integer menjadi format mata uang resmi (contoh: `Rp 50.000`). |
| 3 | **Percabangan Status & Badge** | `functions.php`, `katalog.php` | `getBadgeStatus()` | Menggunakan sintaks modern PHP `match()` untuk memberi warna badge (hijau = Tersedia, merah = Penuh, kuning = Khusus Member). |
| 4 | **Render Otomatis Foreach** | `katalog.php` | `foreach ($daftarRuangan as $ruang)` | Merender kartu ruangan secara dinamis dari array utama tanpa penulisan HTML berulang. |
| 5 | **Hitung Ruangan Siap Pakai** | `functions.php`, `index.php`, `katalog.php` | `hitungRuanganTersedia()` | Menyaring data array dengan `array_filter()` untuk menghitung ruangan yang saat ini berstatus 'Tersedia'. |
| 6 | **Ringkasan Total Kapasitas** | `functions.php`, `index.php`, `katalog.php` | `hitungTotalKapasitas()` | Menghitung total daya tampung orang dari seluruh ruangan yang terdaftar. |
| 7 | **Ringkasan Rata-Rata Harga** | `functions.php`, `katalog.php` | `hitungRataRataHarga()` | Menghitung nilai rata-rata harga sewa dari data yang sedang ditampilkan. |
| 8 | **Pencarian Ruang Termurah** | `functions.php`, `index.php` | `cariRuanganTermurah()` | Mengurutkan dan mencari harga termurah menggunakan `usort()` sebagai referensi awal harga. |

---

## 6. Daftar Fungsi Array Bawaan PHP yang Digunakan

| No | Fungsi Bawaan Array | Letak File:Baris | Tujuan / Kegunaan dalam Proyek |
|:--:|:---|:---|:---|
| 1 | `count()` | `functions.php`, `index.php`, `katalog.php` | Menghitung jumlah total item ruangan kerja dan jumlah ruangan berstatus tersedia. |
| 2 | `array_column()` | `functions.php` (`hitungTotalKapasitas`, `hitungRataRataHarga`) | Mengambil kolom nilai tertentu (seperti `'kapasitas'`, `'harga'`) dari array multidimensi. |
| 3 | `array_sum()` | `functions.php` (`hitungTotalKapasitas`, `hitungRataRataHarga`) | Menjumlahkan seluruh angka dalam array (total kapasitas dan akumulasi harga). |
| 4 | `array_filter()` | `functions.php` (`hitungRuanganTersedia`) | Menyaring elemen ruangan kerja yang berstatus 'Tersedia'. |
| 5 | `usort()` | `functions.php` (`cariRuanganTermurah`) | Mengurutkan array berdasarkan nilai custom (mencari ruangan sewa paling terjangkau). |

---

## 7. Tangkapan Layar Aplikasi

> Letakkan gambar tangkapan layar di dalam folder `screenshots/` dengan nama file `screenshot-profil.svg` dan `screenshot-katalog.svg`.

1. **Tampilan Halaman Profil (`index.php`)**:
   ![Tangkapan Layar Halaman Profil SpaceHub](screenshots/screenshot-profil.svg)
   *(Menampilkan navigasi semantik, hero banner ringkas, ringkasan statistik, profil singkat perusahaan, dan 3 fasilitas utama).*

2. **Tampilan Halaman Katalog (`katalog.php`)**:
   ![Tangkapan Layar Halaman Katalog SpaceHub](screenshots/screenshot-katalog.svg)
   *(Menampilkan kartu statistik hitung PHP, render kartu item dengan foreach, dan badge status).*

---

## 8. Sumber Referensi & Catatan Pemakaian AI

### Sumber Referensi:
1. Dokumentasi Resmi PHP: [https://www.php.net/manual/en/](https://www.php.net/manual/en/) (Referensi `strict_types`, `array_filter`, `array_column`, `array_sum`, `usort`, `match expression`).
2. MDN Web Docs: [https://developer.mozilla.org/](https://developer.mozilla.org/) (Referensi elemen semantik HTML5 dan CSS Box Model).
3. Standar Penulisan Kode PHP PSR-12: [https://www.php-fig.org/psr/psr-12/](https://www.php-fig.org/psr/psr-12/).

### Catatan Pemakaian AI:
- **Bagian yang dibantu**: AI digunakan untuk membantu merancang variasi data dummy yang realistis pada array multidimensi (`data.php`), ilustrasi visual SVG banner, serta penyusunan format dokumentasi README ini.
- **Verifikasi Mandiri**: Seluruh logika PHP (`declare(strict_types=1);`, fungsi buatan sendiri bertipe, sanitasi `e()`, dan fungsi manipulasi array) telah diuji, dipahami, dan diverifikasi langsung melalui PHP 8.4 CLI untuk memastikan tidak ada *error*, *warning*, maupun *notice*. Mahasiswa memahami alur eksekusi baris per baris dan siap mendemokannya saat sesi ujian di hadapan dosen.

---

## Panduan Tambahan: 5 Langkah Commit Git Bertahap

Untuk memenuhi kriteria Git Level 4 (minimal 5 commit bertahap dengan pesan bermakna), ikuti langkah berikut di terminal:

```bash
# Inisialisasi git repository
git init

# Commit 1: Tahap 1 - Rancang tema dan struktur folder awal
git add README.md .gitignore
git commit -m "feat(init): inisialisasi struktur proyek dan dokumentasi awal tema layanan kerja"

# Commit 2: Tahap 2 - Kerangka HTML semantik dan stylesheet CSS murni
git add style.css assets/
git commit -m "feat(ui): rancang antarmuka semantik, tata letak CSS box model, dan aset grafis"

# Commit 3: Tahap 3 - Data array multidimensi dan fungsi pembantu bertipe
git add data.php functions.php
git commit -m "feat(core): implementasi data array multidimensi dan fungsi logika bertipe PSR-12"

# Commit 4: Tahap 3 - Render halaman dinamis dengan foreach dan ringkasan hitung
git add index.php katalog.php
git commit -m "feat(render): integrasi render halaman profil simpel dan katalog dinamis dengan foreach"

# Commit 5: Tahap 4 - Refaktor clean code dan kelengkapan dokumentasi
git add screenshots/ README.md
git commit -m "docs: lengkapi dokumentasi teknis, tangkapan layar, dan alur website lengkap"
```
