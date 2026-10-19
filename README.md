# Template Dashboard Capreno

Template dashboard demo komersial Capreno (single-file HTML, tanpa build step), direplikasi dari [`Dashboard-Capreno-SS1`](https://github.com/sinoeg2002-commits/Dashboard-Capreno-SS1) — dashboard komersial Capreno South Sumatera. Dipakai untuk membuat dashboard serupa di wilayah lain dengan format/desain yang sama persis.

## Isi repo

- **`template-dashboard-capreno.html`** — versi **live**: saat dibuka, otomatis mengambil data terbaru langsung dari 3 spreadsheet Google Sheets ("Demo SS", "Demo SS1", "Demo SS2" milik akun `demoherbisida@gmail.com`), menggabungkannya, lalu merender dashboard. Tidak ada data yang di-hardcode di file ini.
- **`contoh-dashboard-capreno.html`** — salinan persis dashboard South Sumatera lengkap dengan 21 data demo asli (data statis, tidak live), sebagai referensi format data & tampilan saat sudah terisi penuh.

## Cara kerja `template-dashboard-capreno.html` (live Google Sheets)

Saat halaman dimuat, skrip di bagian `SHEET_IDS` mengambil sheet bernama **"Demo"** dari tiap spreadsheet lewat endpoint publik Google (`.../gviz/tq?tqx=out:json&sheet=Demo`), mem-parsing 131 kolomnya (sama persis format kolom antar ketiga sheet, sudah dicek konsisten), lalu memetakannya ke struktur data dashboard. Record dengan ID demo (`Nomor Demo`) yang sama di lebih dari satu sheet otomatis di-dedup (prioritas: SS1 → SS2 → SS, sheet yang disebut lebih dulu menang).

**Menambah/mengganti sumber sheet:** edit objek `SHEET_IDS` di dalam file (cari `const SHEET_IDS = {`), isi dengan ID spreadsheet (bagian di URL antara `/d/` dan `/edit`). Spreadsheet tujuan wajib:
1. Punya sheet/tab bernama persis **"Demo"**, dengan susunan kolom yang sama (131 kolom, A sampai EA, mengikuti form entri demo komersial Capreno standar).
2. Dibagikan dengan akses **"Anyone with the link: Viewer"** — karena pengambilan data dilakukan langsung dari browser pengguna tanpa login, bukan lewat server.

**Butuh internet setiap kali dibuka** — tanpa koneksi, pojok kanan atas bar "Tampilan" akan menampilkan "Gagal memuat data — cek koneksi internet" dan dashboard tampil kosong (0 demo), tanpa error/crash.

Untuk dashboard **statis** (data dikunci di dalam file, tidak berubah walau sheet sumber diedit lagi) — gunakan pola `contoh-dashboard-capreno.html` sebagai acuan: ganti bagian live-fetch dengan `const DATA = [...]` berisi data final.

## Fitur yang sudah termasuk

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
