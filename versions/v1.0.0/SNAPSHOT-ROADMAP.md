> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# SNAPSHOT-ROADMAP v1.0.0 — Salinan Beku

Salinan beku janji v1.0.0 pada 10 Okt 2026. Tidak diubah lagi. Perubahan masa depan dicatat di `roadmap/` living dan dirilis sebagai versi baru.

## Janji Beku

- Aturan titip: pesan maksimal pukul 20.00, serah besok 06.00–08.00, bayar QRIS di muka kedaluwarsa 15 menit, satu pesanan satu vendor, maksimal 20 kg dan 50 baris per pesanan.
- Vendor: 5 syarat lolos, kuota aktif 30 dan Prioritas 60 pesanan per hari, radius 5 km, ongkir Rp5.000 flat, foto serah 1 foto per pesanan.
- Uang: fee vendor 12 persen dari total item, bonus Prioritas 0,5 persen, denda batal sepihak Rp15.000, voucher pembeli Rp10.000, cair H+1 mulai 06.00, rekonsiliasi Lumbung tiap Senin 09.00.
- Loyalitas: repeat-order minimal 40 persen per vendor per 4 minggu untuk naik Prioritas, target kota minimal 35 persen, rating minimal 4,5, tepat serah minimal 95 persen, batal vendor maksimal 2 persen, median konfirmasi maksimal 20 menit.
- Sistem: mobile KMP 1 codebase 2 peran, web admin Bun 1.4 Svelte 5, job sinkron Lumbung 04.00, timeout konfirmasi tiap 1 menit, cair fee 00.00, arsip tanggal 1. DB titipo. JWT beraudien titipo 24 jam, service key titipo ke lumbung rotasi 90 hari.
- Fase: operasi kecil hari 1 sampai 30 dengan 10 vendor, perluasan hari 31 sampai 60 dengan 25 vendor, lepas rintisan hari 61 sampai 90 dengan 40 vendor.

## Sumber

- Dibekukan dari `vendor/`, `pesanan/`, `produk/`, `platform/`, `keuangan/`, dan `metrik/` versi lama. File lama sudah dipindah dengan `git mv` dan dihapus dari lokasi asal.

## Batasan

Batasan dokumen ini: hanya salinan janji saat v1.0.0 disetujui. Tidak menjadi acuan operasional terkini; acuan terkini ada di `roadmap/TIMELINE.md` dan `roadmap/MILESTONE.md`.
