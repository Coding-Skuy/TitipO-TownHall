> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# BRD 10 — Vendor dan Kurasi

## Konteks

Vendor mencakup kurasi, tingkatan, janji layanan, dan sanksi. Sumber isi lama: `vendor/00-piagam-vendor-terpercaya.md` dan `vendor/10-sla-vendor.md`.

## Kebutuhan Bisnis

- BR-101 Calon vendor wajib lolos 5 syarat tanpa pengecualian: KTP dan domisili radius 5 km dari titik serah Lumbung, perangkat Android 9 ke atas atau iOS 16 ke atas, uji kebersihan skor minimal 80 dari 10 poin, setuju SLA dengan tanda tangan digital, deposit Rp100.000 diakumulasi dari fee.
- BR-102 Tingkatan vendor dikunci: Calon belum bisa terima pesanan, Aktif maksimal 30 pesanan per hari, Prioritas maksimal 60 pesanan per hari dengan badge emas dan fee tambah 0,5 persen, Skors 3 sampai 7 hari tidak bisa terima pesanan, Keluar bila 3 kali skors dalam 90 hari atau pemalsuan.
- BR-103 Syarat Prioritas: repeat-order minimal 40 persen selama 4 minggu berturut ditambah rating minimal 4,7.
- BR-104 Konfirmasi pesanan maksimal 30 menit setelah masuk pada 05.00 sampai 20.00; lewat itu dialihkan ke vendor cadangan radius 2 km.
- BR-105 Kesiapan serah tepat dalam jendela toleransi 15 menit dari janji dalam rentang 06.00 sampai 08.00; respons chat maksimal 15 menit pada jam aktif; sinkron stok selesai sebelum 05.00.
- BR-106 Ketepatan item minimal 98 persen; foto serah 1 foto per pesanan wajib; rating 30 hari minimal 4,5; kemasan sayur basah lapis ganda dan telur wajib egg-tray.
- BR-107 Sanksi dikunci: terlambat serah lebih dari 30 menit 2 kali seminggu berujung skors 3 hari, batal sepihak kurang dari 2 jam sebelum serah denda Rp15.000 dipotong fee ditambah voucher pembeli Rp10.000, stok fiktif skors 7 hari dan kuota turun ke 15 per hari selama 30 hari, pemalsuan foto berujung keluar dan deposit hangus.
- BR-108 Vendor aktif wajib buka minimal 5 hari per minggu; tutup terjadwal wajib diset H-1 pukul 20.00; kuota habis menutup tombol otomatis tanpa override manual.

## Metrik

- Skor kebersihan saat daftar. Waktu konfirmasi per pesanan. Tepat serah mingguan. Rating 30 hari. Repeat 4 minggu. Jumlah skors per 90 hari.

## Batasan

Batasan segmen ini: hanya dari daftar sampai keluar vendor. Di luar batas: alur pesanan rinci, perhitungan fee, dan implementasi aplikasi. Janji harga ke rumah tangga: harga aplikasi sama dengan harga bayar, ongkir tampil terpisah Rp5.000 flat radius 5 km.
