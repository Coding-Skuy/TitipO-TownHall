> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# CHANGELOG v1.0.0 — Versi Awal TitipO

## Ringkasan Isi

v1.0.0 adalah versi awal TownHall TitipO yang dibekukan mengikuti pola template emas Lumbung-TownHall. Seluruh isi lama dari folder `vendor/`, `pesanan/`, `produk/`, `platform/`, `keuangan/`, dan `metrik/` dipecah dan dipindah dengan `git mv` ke struktur versi ini, lalu folder lama dihapus agar hanya ada satu sumber kebenaran.

## Isi per Direktori

- BRD: `00-ikhtisar.md` memuat piagam, aturan pesan maksimal 20.00 serah 06.00–08.00, fee 12 persen, repeat minimal 40 persen, DB titipo, JWT beraudien titipo dan service key titipo ke lumbung. `10-vendor-kurasi.md` memuat 5 syarat lolos, tingkatan, dan SLA. `20-titip-harian.md` memuat 8 langkah titip dan mesin status. `30-keuangan-fee.md` memuat formula fee, ongkir Rp5.000, dan rekonsiliasi Lumbung Senin 09.00.
- PRD: `10-pengguna.md` memuat 3 peran rumah tangga, vendor, admin dan operasi serta 5 layar rumah dan 3 layar vendor. `20-alur.md` memuat matriks KMP lawan web dan perjalanan 90 detik. `30-kriteria.md` memuat US-001 dan seterusnya, ambang repeat, rating, tepat serah, dan non-goals.
- FRD: `10-fungsional.md` memuat FR-001 dan seterusnya per segmen vendor, pesanan, keuangan, tanpa cara implementasi.
- FSD: `10-alur.md` memuat urutan tulis lokal dahulu dan resolusi konflik first-arrival-wins. `20-model-data.md` memuat entitas vendor, pesanan, item, pembayaran, audit dengan ULID dan rupiah integer. `30-kontrak.md` memuat kontrak API mobile 7 endpoint, admin 6 endpoint, event, galat, dan autentikasi JWT audien titipo 24 jam serta service key.
- `SNAPSHOT-ROADMAP.md` memuat salinan beku janji 90 hari operasi.

## Sumber Pemindahan

- `vendor/00-piagam-vendor-terpercaya.md` menjadi BRD ikhtisar. `vendor/10-sla-vendor.md` menjadi BRD vendor kurasi. `pesanan/10-alur-titip-harian.md` menjadi BRD titip harian. `keuangan/10-bagi-fee.md` menjadi BRD keuangan fee.
- `produk/10-ux-pesan-titip.md` dan `platform/10-matriks-KMP-web.md` menjadi PRD pengguna dan alur. `metrik/10-repeat-order.md` menjadi PRD kriteria.
- `produk/20-kontrak-api-KMP-mobile.md` menjadi FRD fungsional sebagai basis kebutuhan, lalu ditulis ulang tanpa cara implementasi.
- `platform/60-offline-sinkron.md` menjadi FSD alur. `pesanan/20-model-data.md` menjadi FSD model data. `produk/21-kontrak-api-web-bun.md` menjadi FSD kontrak.
- `produk/30-modul-KMP-bersama.md`, `platform/40-mobile-KMP.md`, dan `platform/50-web-bun-svelte.md` dilebur ke FSD kontrak dan FSD alur sebagai rincian implementasi, lalu file asal dihapus dari lokasi lama.

## Batasan

Batasan versi ini: hanya titip harian rumah tangga, vendor kurasi radius 5 km, pesan maksimal 20.00, serah 06.00–08.00, fee 12 persen, repeat minimal 40 persen, DB titipo. Perubahan setelah ini wajib masuk v1.1.0 atau v2.0.0 dan dicatat di `roadmap/` living, bukan dengan mengubah file beku ini.
