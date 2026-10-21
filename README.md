# xclite

**Versi 1.0 Beta** · Windows x64

xclite adalah spreadsheet ringan untuk membuka, mengedit, mencari, memfilter dan meng-query file CSV raksasa (sudah diuji sampai 20 GB dan lebih dari 100 juta baris), juga file Excel (.xlsx).

> xclite adalah hasil **fork dan modifikasi dari [GridCraft](https://github.com/storytold/gridcraft) karya ArtCraft Team dan kontributor GridCraft**. Mesin rumus (501 fungsi), format angka, dan pembaca/penulis xlsx berasal dari GridCraft (MIT OR Apache-2.0) dan dipakai tanpa diubah. Di atasnya xclite menambahkan penampil file raksasa, cache kolom, pencarian cepat, filter, dan SQL.

## Unduh dan jalankan

1. Unduh `release_xclite_betav.01.x64.zip` dari halaman [Releases](https://github.com/muhammadalmuhtadi/xclite/releases).
2. Ekstrak, lalu jalankan `xclite.exe`. Tidak perlu instalasi.

Isi folder:

| File | Fungsi |
|---|---|
| `xclite.exe` | Aplikasinya (sekitar 22 MB). |
| `duckdb.dll` | Mesin SQL dan urut. Simpan di folder yang sama dengan `xclite.exe`. Tanpa file ini aplikasi tetap jalan, tetapi SQL dan urut tidak bisa dipakai. |
| `cache\` | Cache file besar (indeks baris dan cache kolom). Boleh dihapus kapan saja; dibuat ulang saat file dibuka lagi. |
| `LICENSE-MIT`, `LICENSE-APACHE` | Lisensi GridCraft. |

## Kenapa ringan

- **File tidak dimuat ke RAM.** File dibaca langsung dari disk (memory-mapped); hanya baris yang terlihat di layar yang dibaca.
- **Satu kali baca.** Saat file besar pertama kali dibuka, xclite membacanya sekali secara berurutan dan menyimpan indeks baris serta cache kolom (Parquet terkompresi, sekitar 0,3–0,4× ukuran CSV). Pembukaan berikutnya langsung.
- **Mesin SQL dimuat hanya saat dipakai.** `duckdb.dll` baru dibaca ketika Anda menjalankan SQL atau mengurutkan data.
- **Hemat memori.** Sekitar 50 MB saat baru dibuka; memori SQL dilepas lagi satu menit setelah kueri terakhir.

Contoh hasil pengukuran pada file sintetis 20 GB: pembukaan pertama sekitar 3,5 menit; pencarian di semua kolom sekitar 2 detik; filter angka sekitar 0,1 detik.

## Fitur

- **Edit seperti spreadsheet:** ketik, salin/potong/tempel, Paste Special, isi otomatis, sisipkan dan hapus baris atau kolom, undo dan redo. File asli tidak diubah sampai Anda menyimpan.
- **Rumus:** 501 fungsi lembar kerja dari GridCraft, dan format angka.
- **Tabel bernama:** tandai sebuah rentang sebagai tabel, lalu ganti nama dan rentangnya lewat **Kelola tabel**.
- **Cari atau Analisis:**
  - Cari teks di semua kolom.
  - Baris filter dan filter lanjutan (berisi, sama dengan, diawali, kosong, dan aturan angka). Kondisi bisa digabung dengan DAN/ATAU.
  - SQL / Kueri lewat DuckDB. Kolom sheet bernama A, B, …; tiap tabel menjadi view dengan nama kolomnya sendiri.
- **Hasil berantai:** setiap hasil filter atau kueri terbuka di tab baru dan otomatis menjadi tabel. Tabel itu bisa difilter atau di-query lagi, juga digabung (JOIN) dengan tabel di tab lain.
- **Persiapan dan Formating:**
  - format angka;
  - **tipe data** per kolom (bilangan bulat, desimal, tanggal & jam), supaya kueri seperti `status >= 2` berjalan;
  - ubah teks dengan regex (hapus atau ganti, dengan pratinjau langsung);
  - cari & ganti, dan bersihkan data.
- **Simpan** ke CSV atau xlsx.

## Batasan (beta)

- Teks dianggap UTF-8. Font bawaan tidak punya huruf CJK.
- Catatan, validasi data dan nama yang ditentukan hanya tersimpan di .xlsx, tidak di CSV.
- File Excel dibaca seluruhnya ke memori (Excel sendiri dibatasi 1.048.576 baris).
- Sebuah `"` yang nyasar di tengah kolom tanpa tanda kutip bisa menggabungkan baris.

## Membangun dari kode sumber

Ditulis dalam Rust (eframe/egui, memmap2, rayon, arrow/parquet, duckdb). Di Windows, build dijalankan lewat `cargo-msvc.cmd build --release`, yang menjalankan cargo di lingkungan Visual Studio x64.

## Lisensi dan atribusi

- **GridCraft:** © ArtCraft Team dan kontributor GridCraft, MIT OR Apache-2.0, https://github.com/storytold/gridcraft (commit `66ce6be`).
- **DuckDB** (`duckdb.dll`): MIT, https://duckdb.org
- egui/eframe, arrow/parquet dan crate Rust lainnya: MIT dan/atau Apache-2.0.

## Pembuat

**M Al Muhtady**
- Email: muhamadalmuhtadi@gmail.com
- GitHub: https://github.com/muhammadalmuhtadi
