# Template Dashboard Capreno

Template dashboard demo komersial Capreno (single-file HTML, tanpa build step), direplikasi dari [`Dashboard-Capreno-SS1`](https://github.com/sinoeg2002-commits/Dashboard-Capreno-SS1) — dashboard komersial Capreno South Sumatera. Dipakai untuk membuat dashboard serupa di wilayah lain dengan format/desain yang sama persis.

## Isi repo

- **`template-dashboard-capreno.html`** — versi **live**: saat dibuka, otomatis mengambil data terbaru langsung dari spreadsheet Google Sheets sumber tiap wilayah ("Demo SS", "Demo SS1" dari akun `demoherbisida@gmail.com`, "Demo EJ", "Demo SUL"/"Demo Sul 2"), menggabungkannya, lalu merender dashboard **nasional** dengan sidebar filter wilayah. Tidak ada data yang di-hardcode di file ini.
- **`contoh-dashboard-capreno.html`** — salinan persis dashboard South Sumatera lengkap dengan 21 data demo asli (data statis, tidak live), sebagai referensi format data & tampilan saat sudah terisi penuh.

## Cakupan wilayah & sidebar filter

Dashboard ini dirancang untuk menampung demo dari **6 wilayah**: SS, EJ EI, Sulawesi, NS, CJ, WJ. Sidebar di sisi kiri (menyatu dengan header, tetap terlihat saat di-scroll di mode Handphone) berisi filter eksklusif. Saat ini baru **SS, EJ EI, dan Sulawesi** yang sudah ada sumber datanya, jadi tombol sidebar yang tampil baru:

`Nasional` (gabungan semua wilayah) · `SS` · `EJ EI` · `Sulawesi`

`NS`, `CJ`, `WJ` untuk sementara disembunyikan dari sidebar (dikomentari di HTML, cari `<!-- NS/CJ/WJ sengaja disembunyikan -->`) sampai sheet sumbernya tersedia — tinggal di-uncomment begitu waktunya tiba, tanpa perlu ubah logika lain (lihat bagian berikutnya).

Tiap tombol menampilkan jumlah demo di wilayah itu. Memilih satu wilayah akan mempersempit seluruh dashboard (skor, peta, filter lain, analisis teks) ke demo wilayah tersebut saja; `Nasional` menampilkan gabungan semuanya.

### Link langsung ke satu wilayah (`?wilayah=...`)

Tambahkan parameter `?wilayah=<nama wilayah>` di URL untuk membuka dashboard yang otomatis tersaring ke wilayah itu saat pertama dibuka (tanpa perlu klik sidebar) — cocok untuk dibagikan ke tiap Squad agar langsung melihat datanya sendiri. Nilainya harus persis salah satu dari `Nasional`, `SS`, `EJ EI` (spasi di-encode jadi `%20`), `Sulawesi`, `NS`, `CJ`, `WJ`; nilai lain/tidak dikenal otomatis jatuh ke `Nasional`. Sidebar & filter lain tetap bisa diklik seperti biasa setelah halaman terbuka. Contoh (ganti domain sesuai URL GitHub Pages repo ini):

```
https://sinoeg2002-commits.github.io/template-dashboard-capreno/template-dashboard-capreno.html?wilayah=SS
https://sinoeg2002-commits.github.io/template-dashboard-capreno/template-dashboard-capreno.html?wilayah=EJ%20EI
https://sinoeg2002-commits.github.io/template-dashboard-capreno/template-dashboard-capreno.html?wilayah=Sulawesi
```

Parameter ini juga dipertahankan kalau linknya ke halaman utama (`index.html`), karena `index.html` meneruskannya saat redirect ke `template-dashboard-capreno.html`.

## Cara kerja `template-dashboard-capreno.html` (live Google Sheets, multi-wilayah)

Saat halaman dimuat, skrip di bagian `SHEET_SOURCES` mengambil sheet bernama **"Demo"** dari tiap spreadsheet sumber lewat endpoint publik Google (`.../gviz/tq?tqx=out:json&sheet=Demo&headers=1`), membaca baris headernya, lalu **memetakan kolom secara otomatis berdasarkan nama/label kolom** (fungsi `resolveColumns`) — bukan berdasarkan posisi tetap. Ini penting karena sheet "Demo" tiap wilayah ternyata punya jumlah & urutan kolom yang **berbeda-beda** (SS-family 131 kolom, EJ 112 kolom, Sulawesi 102 kolom, dst) walau isi datanya setara; resolver mengenali tiap field lewat teks label section ("Data 7HSA", "Data 0HSA (saat aplikasi)", dst) dan nama kolomnya, sehingga sheet sumber wilayah baru (CJ/WJ/NS) tidak perlu punya susunan kolom identik dengan SS — cukup pakai nama kolom & section yang sama (mengikuti form demo komersial Capreno standar).

Beberapa field diturunkan otomatis kalau sheet sumber tidak punya kolom eksplisit untuk itu:
- **Kabupaten** — kalau tidak ada kolom "Kabupaten", dicoba diambil dari teks "Lokasi Demo" dengan pola `Kabupaten <nama>`; kalau tidak ketemu juga, tampil sebagai "Tidak diisi".
- **Hari Aplikasi** (HST) — dihitung dari selisih "Tanggal Tanam" dan "Tanggal Aplikasi" kalau tidak ada kolom eksplisit.
- **Timing Aplikasi** — diturunkan dari Hari Aplikasi di atas ke salah satu kelompok: `<10 HST`, `10-15 HST`, `16-20 HST`, `21-30 HST`, `>30 HST`.

Record dengan ID demo (`Nomor Demo`) yang sama di lebih dari satu sheet pada wilayah yang sama otomatis di-dedup (sheet yang disebut lebih dulu di `SHEET_SOURCES` menang).

**Menambah/mengganti sumber sheet (termasuk mengisi NS/CJ/WJ):** edit array `SHEET_SOURCES` di dalam file (cari `const SHEET_SOURCES = [`), tambahkan entri `{ tag: '<label bebas>', wilayah: '<salah satu dari SS/EJ EI/Sulawesi/NS/CJ/WJ>', id: '<ID spreadsheet>' }` (ID spreadsheet = bagian di URL antara `/d/` dan `/edit`). Spreadsheet tujuan wajib:
1. Punya sheet/tab bernama persis **"Demo"**, mengikuti form entri demo komersial Capreno standar (nama kolom & section boleh beda jumlah/urutan dari wilayah lain — lihat di atas).
2. Dibagikan dengan akses **"Anyone with the link: Viewer"** — karena pengambilan data dilakukan langsung dari browser pengguna tanpa login, bukan lewat server.

> **TODO — NS, CJ, WJ:** belum ada sheet sumbernya (per Okt 2026), jadi `SHEET_SOURCES` baru berisi entri SS/EJ EI/Sulawesi. Begitu sheet "Demo" untuk NS/CJ/WJ sudah dibuat & dibagikan sesuai syarat di atas, tinggal un-comment 3 baris TODO yang sudah disiapkan di bawah array `SHEET_SOURCES` dalam file, lalu isi `id` masing-masing dengan ID spreadsheet-nya — tidak perlu perubahan kode lain (resolver kolom & sidebar sudah menangani wilayah baru secara otomatis).

**Butuh internet setiap kali dibuka** — tanpa koneksi, pojok kanan atas bar "Tampilan" akan menampilkan "Gagal memuat data — cek koneksi internet" dan dashboard tampil kosong (0 demo), tanpa error/crash.

Untuk dashboard **statis** (data dikunci di dalam file, tidak berubah walau sheet sumber diedit lagi) — gunakan pola `contoh-dashboard-capreno.html` sebagai acuan: ganti bagian live-fetch dengan `const DATA = [...]` berisi data final.

## Fitur yang sudah termasuk

- Sidebar filter wilayah (Nasional/SS/EJ EI/Sulawesi, NS/CJ/WJ menyusul) + link langsung per wilayah lewat `?wilayah=...` — lihat bagian di atas.
- 3 halaman: Skoring Herbi FP vs Capreno, Win-Lose Capreno - Herbi FP, Dokumentasi Demo Komersial.
- Filter multi-kriteria (herbi pembanding, varietas, petugas, lokasi, kabupaten, timing aplikasi, fase daun gulma, olah tanah, pola tanam, bulan tanam) dengan jumlah demo per opsi.
- Peta sebaran demo (basemap Esri, 2 gaya: Minimal & Terrain) — butuh koneksi internet saat dibuka; tanpa koneksi otomatis fallback ke grid koordinat.
- Perbandingan foto 0/7/14/21 HSA (Capreno vs FP) dengan popup pembesaran.
- Analisis data otomatis (ringkasan teks) di tiap halaman, termasuk analisis faktor penyebab crop performance rendah.
- Mode tampilan **Landscape** dan **Handphone** (freeze panes: header & filter tetap terlihat saat data di-scroll), bisa dipilih lewat bar di atas.

## Catatan

- `template-dashboard-capreno.html` mengambil data live dari Google Sheets saat dibuka (lihat bagian di atas); `contoh-dashboard-capreno.html` memakai data statis `const DATA = [...]` di dalam file, tanpa backend/database terpisah.
- Beberapa field hasil transformasi dari sheet mentah disederhanakan otomatis (mis. "Olah tanah sempurna (menggunakan traktor)" → "Olah tanah sempurna"), kecuali `lokasi` yang ditampilkan apa adanya dari kolom "Lokasi Demo" (alamat lengkap, tidak dipersingkat otomatis seperti pada data South Sumatera asli).
- Foto diambil dari link Google Drive (field `cap`/`fp` per periode HSA berisi file ID Google Drive, diekstrak dari kolom `linkFoto...` pada sheet).
- Peta memakai Esri ArcGIS (gratis, tanpa API key) — pastikan koneksi internet saat membuka dashboard agar basemap termuat.
