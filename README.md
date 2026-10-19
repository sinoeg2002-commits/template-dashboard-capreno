# Template Dashboard Capreno

Template dashboard demo komersial Capreno (single-file HTML, tanpa build step), direplikasi dari [`Dashboard-Capreno-SS1`](https://github.com/sinoeg2002-commits/Dashboard-Capreno-SS1) — dashboard komersial Capreno South Sumatera. Dipakai untuk membuat dashboard serupa di wilayah lain dengan format/desain yang sama persis.

## Isi repo

- **`template-dashboard-capreno.html`** — versi siap pakai, berisi **1 contoh demo** ("Lampung A 01") agar struktur data terlihat jelas sejak dibuka, bukan sekadar kerangka kosong. Gunakan ini sebagai titik awal: hapus/ganti contohnya, lalu tambahkan data demo wilayah Anda sendiri.
- **`contoh-dashboard-capreno.html`** — salinan persis dashboard South Sumatera lengkap dengan 21 data demo asli, sebagai referensi tambahan untuk format data & tampilan saat sudah terisi penuh.

## Cara pakai `template-dashboard-capreno.html`

1. Buka file dengan editor teks, cari `const DATA = [{ "id": "Lampung A 01", ... }]`. Record contoh ini berisi semua field yang dipakai dashboard (skor, foto, komentar, blind assessment, dll) — salin strukturnya untuk tiap demo baru, atau hapus dan ganti seluruhnya dengan array demo Anda sendiri.
2. Ganti nama wilayah "Lampung" (judul, label skor, footer) dengan wilayah/sumber data yang sesuai:
   - `<title>Dashboard Demo Komersial Capreno Lampung (contoh 1 demo)</title>`
   - `Dashboard Demo Komersial Capreno Lampung`
   - `Total demo komersial di Lampung`
   - `Sumber: Demo_Lampung, 1 demo komersial.` (bagian footer)
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
