> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# TIMELINE TitipO — Garis Waktu Hidup Lintas Versi

Dokumen living: diperbarui tiap ada versi baru. Salinan beku v1.0.0 ada di `versions/v1.0.0/SNAPSHOT-ROADMAP.md` dan tidak diubah lagi.

## Garis Waktu

- 03 Jan 2026 sampai 09 Okt 2026 — Persiapan. Kurasi 10 vendor perdana radius 5 km dari titik serah Lumbung, uji sinkron katalog 04.00, uji alur pesan maksimal 20.00 dan serah 06.00–08.00. Sumber isi lama: `vendor/00-piagam-vendor-terpercaya.md` dan `pesanan/10-alur-titip-harian.md`.
- 10 Okt 2026 — v1.0.0 disetujui. Struktur versi BRD, PRD, FRD, FSD dibekukan mengikuti pola emas Lumbung. Aturan dikunci: pesan maksimal 20.00, serah 06.00–08.00, fee vendor 12 persen, repeat-order minimal 40 persen, DB titipo, JWT beraudien titipo ditambah service key titipo ke lumbung.
- Hari 1 sampai 30 operasi — Operasi kecil. 10 vendor aktif, kuota 30 pesanan per hari, 1 titik serah Lumbung. Target arus: pesan H maksimal 20.00, konfirmasi maksimal 30 menit, serah H+1 pukul 06.00–08.00, fee cair H+2 pukul 06.00.
- Hari 31 sampai 60 operasi — Perluasan. Total 25 vendor, 2 titik serah, vendor pertama naik Prioritas bila repeat minimal 40 persen selama 4 minggu dan rating minimal 4,7.
- Hari 61 sampai 90 operasi — Kesiapan lepas rintisan. Total 40 vendor, repeat kota minimal 35 persen, batal vendor maksimal 2 persen, tepat serah minimal 95 persen.
- Setelah 90 hari — Skala dan versi berikutnya. Penambahan titik serah ketiga, uji ongkir Rp3.000 bila repeat kota di bawah 30 persen selama 4 minggu. Rencana rinci menunjuk `ROADMAP.md` untuk v1.1.0 dan v2.0.0.

## Keterkaitan Versi

- v1.0.0 menjadi acuan awal. Perubahan jadwal pada versi baru dicatat di sini dengan tanggal dan nomor versi, tanpa mengubah snapshot beku.

## Batasan

Batasan dokumen ini: hanya mencatat tonggak waktu dan fase. Detail kebutuhan tetap di `versions/v1.0.0/BRD/`, detail kriteria lulus di `versions/v1.0.0/PRD/30-kriteria.md`, dan detail janji beku di `SNAPSHOT-ROADMAP.md`. Dokumen ini tidak mengatur tarif fee, kuota vendor, atau kontrak API.
