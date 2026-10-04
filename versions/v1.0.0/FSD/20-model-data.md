> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# FSD 20 — Model Data

## Entitas Inti

- Vendor: id teks, nama, status enum calon, aktif, prioritas, skors, keluar, kuota harian integer 30 atau 60, rating 30 hari, titik serah Lumbung teks, dibuat timestamptz. Contoh: V-SARI-001 Warung Sari aktif rating 4,9.
- Pesanan: id ULID 26 karakter, kode unik T-YYYY-NNNNNN contoh T-2026-000112, id rumah tangga, id vendor, status enum menunggu pembayaran, menunggu konfirmasi, disiapkan, diserahkan, selesai, kedaluwarsa, dialihkan, dibatalkan sistem, dibatalkan vendor, total item integer rupiah, ongkir 5000, total bayar integer, jendela serah teks default 06.00-08.00, dibuat, dibayar, dikonfirmasi, diserahkan timestamptz.
- Item pesanan: id ULID, id pesanan, sku lumbung teks contoh LMB-BAYAM-001, nama, jumlah gram integer di atas 0, harga per kg integer, subtotal integer. Aturan: subtotal sama dengan jumlah gram dikali harga per kg dibagi 1000 dibulatkan ke bawah.
- Pembayaran: id ULID, id pesanan unik, metode default QRIS, status enum menunggu, lunas, kedaluwarsa, refund, id gateway, dibayar timestamptz.
- Audit pesanan: id ULID, id pesanan, dari status, ke status, aktor, waktu timestamptz default now. Tabel append-only tanpa update dan delete untuk peran aplikasi.
- Entitas pendukung: antrean lokal kunci ULID jenis konfirmasi tutup jadwal serah rating, foto serah object storage titipo-bukti dengan nama id pesanan jpg maksimal 2 MB retensi 2 tahun, pesanan arsip untuk data di atas 2 tahun.

## Aturan Angka

- Uang selalu rupiah integer, berat selalu gram integer, waktu selalu timestamptz UTC dan tampil WIB hanya di UI.
- Lokal mobile KMP memakai SQLDelight 4 tabel cermin pesanan, item pesanan, antrean, foto serah ditambah kolom sinkron 0 atau 1. ID memakai ULID agar bisa dibuat luring tanpa tabrakan.

## Batasan

Batasan dokumen ini: hanya definisi entitas, kunci, enum, dan aturan angka. Serialisasi JSON dan endpoint ada di `30-kontrak.md`. Perubahan skema butuh persetujuan Tech Lead dan migrasi teruji. Sumber kebenaran tetap DB titipo di server; DB lokal hanya antre dan cache.
