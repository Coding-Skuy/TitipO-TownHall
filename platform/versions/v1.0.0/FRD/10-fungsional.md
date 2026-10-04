> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# FRD 10 — Kebutuhan Fungsional

Dokumen ini menyatakan apa yang wajib dilakukan sistem, tanpa menyatakan cara implementasi.

## Vendor

- FR-001 Sistem wajib mengelola pendaftaran calon dengan KTP, domisili radius 5 km, skor kebersihan, tanda tangan SLA, dan deposit Rp100.000.
- FR-002 Sistem wajib mengelola tingkatan calon, aktif, prioritas, skors, keluar beserta kuota 30 dan 60 dan aturan 3 kali skors dalam 90 hari.
- FR-003 Sistem wajib menegakkan SLA konfirmasi maksimal 30 menit, serah toleransi 15 menit dalam 06.00 sampai 08.00, dan foto wajib 1 per pesanan.
- FR-004 Sistem wajib mencatat sanksi denda Rp15.000 dan voucher Rp10.000 serta skors 3 dan 7 hari ke log audit append-only.

## Pesanan

- FR-101 Sistem wajib membuat pesanan satu vendor dengan maksimal 20 kg dan 50 baris, total bayar sama dengan total item tambah ongkir Rp5.000.
- FR-102 Sistem wajib menjalankan mesin status tunggal menunggu pembayaran, menunggu konfirmasi, disiapkan, diserahkan, selesai, kedaluwarsa, dialihkan, dibatalkan sistem, dibatalkan vendor.
- FR-103 Sistem wajib menolak SKU kedaluwarsa di atas 24 jam dengan galat 409 dan menolak transisi ilegal dengan galat 422.
- FR-104 Sistem wajib mengalihkan otomatis maksimal 2 kali lalu refund penuh H+1.
- FR-105 Sistem wajib mengelola rating bintang 1 sampai 5 dengan catatan maksimal 200 karakter.

## Keuangan

- FR-201 Sistem wajib menghitung fee 12 persen, bonus Prioritas 0,5 persen, dan denda Rp15.000 per batal sepihak.
- FR-202 Sistem wajib menjalankan ledger fee H+1 mulai 06.00 dengan minimal tarik Rp50.000 dan biaya transfer Rp2.500.
- FR-203 Sistem wajib menerbitkan berita acara CSV rekonsiliasi Lumbung tiap Senin 09.00 dan membagi selisih 50:50 maksimal Rp200.000 per minggu.

## Lintas Segmen

- FR-301 Sistem wajib memberi kode tunggal T-YYYY-NNNNNN dari pesan sampai fee cair.
- FR-302 Sistem wajib mencatat setiap aksi dengan pembuat, waktu, dan versi untuk resolusi konflik antrean.
- FR-303 Sistem wajib menegakkan hak peran: rumah tangga hanya miliknya, vendor hanya lapaknya, admin melihat operasional dan keuangan melihat ledger.
- FR-304 Sistem wajib menyimpan foto serah maksimal 2 MB dengan retensi 2 tahun dan mengarsipkan pesanan selesai di atas 2 tahun.

## Batasan

Batasan dokumen ini: hanya kebutuhan fungsional. Bahasa pemrograman, pustaka, basis data lokal, dan pola navigasi tidak diatur di sini dan hanya boleh muncul di FSD. Setiap kebutuhan di atas wajib punya uji penerimaan di PRD/30-kriteria.md.
