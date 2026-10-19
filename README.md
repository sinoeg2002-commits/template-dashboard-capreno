# Template Dashboard Capreno

Template dashboard demo komersial Capreno (single-file HTML, tanpa build step), direplikasi dari [`Dashboard-Capreno-SS1`](https://github.com/sinoeg2002-commits/Dashboard-Capreno-SS1) — dashboard komersial Capreno South Sumatera. Dipakai untuk membuat dashboard serupa di wilayah lain dengan format/desain yang sama persis.

## Isi repo

- **`template-dashboard-capreno.html`** — versi siap pakai. Array data (`DATA`) dikosongkan, judul & label wilayah diganti placeholder (`[Nama Wilayah]`, `[Nama sumber data]`, `[jumlah]`). Gunakan ini sebagai titik awal untuk wilayah/periode demo baru.
- **`contoh-dashboard-capreno.html`** — salinan persis dashboard South Sumatera lengkap dengan 21 data demo asli, sebagai referensi format data & tampilan saat sudah terisi.

## Cara pakai `template-dashboard-capreno.html`

1. Buka file dengan editor teks, cari `const DATA = []` dan isi dengan array data demo (lihat struktur tiap record di `contoh-dashboard-capreno.html`, variabel `DATA`, baris yang sama).
2. Ganti placeholder berikut dengan nama wilayah/sumber data yang sesuai:
   - `<title>Template Dashboard Demo Komersial Capreno</title>`
   - `Dashboard Demo Komersial Capreno [Nama Wilayah]`
   - `Total demo komersial di [Nama Wilayah]`
   - `Sumber: [Nama sumber data], [jumlah] demo komersial.` (bagian footer)
3. Simpan, lalu buka file `.html`-nya langsung di browser — tidak perlu server atau instalasi apa pun.

## Fitur yang sudah termasuk

- 3 halaman: Skoring Herbi FP vs Capreno, Win-Lose Capreno - Herbi FP, Dokumentasi Demo Komersial.
- Filter multi-kriteria (herbi pembanding, varietas, petugas, lokasi, kabupaten, timing aplikasi, fase daun gulma, olah tanah, pola tanam, bulan tanam) dengan jumlah demo per opsi.
- Peta sebaran demo (basemap Esri, 2 gaya: Minimal & Terrain) — butuh koneksi internet saat dibuka; tanpa koneksi otomatis fallback ke grid koordinat.
- Perbandingan foto 0/7/14/21 HSA (Capreno vs FP) dengan popup pembesaran.
- Analisis data otomatis (ringkasan teks) di tiap halaman, termasuk analisis faktor penyebab crop performance rendah.
- Mode tampilan **Landscape** dan **Handphone** (freeze panes: header & filter tetap terlihat saat data di-scroll), bisa dipilih lewat bar di atas.

## Catatan

- Semua data demo di-hardcode langsung di dalam `const DATA = [...]` pada file HTML — tidak ada backend/database terpisah.
- Foto diambil dari link Google Drive (field `cap`/`fp` per periode HSA berisi file ID Google Drive).
- Peta memakai Esri ArcGIS (gratis, tanpa API key) — pastikan koneksi internet saat membuka dashboard agar basemap termuat.
