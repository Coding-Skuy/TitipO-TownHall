# 10 — SLA Vendor TitipO

> **Tujuan:** Menetapkan target layanan vendor yang terukur, cara mengukurnya, dan konsekuensi gagalnya. Semua angka di bawah adalah komitmen Varian 1.
> **Pemilik:** Tim Operasi TitipO (pemilik SLA), Tim Mobile KMP (pemilik instrumentasi timer di aplikasi).
> **Status:** Varian 1. Review tiap 30 hari berdasarkan `metrik/10-repeat-order.md`.

## 1. Waktu layanan (keputusan konkret)

| Momen | Target Varian 1 | Diukur dari |
|-------|-----------------|-------------|
| Konfirmasi pesanan titip | ≤ 30 menit setelah pesanan masuk (05.00–20.00) | Timestamp `created_at` → `confirmed_at` di API |
| Kesiapan serah pagi | Tepat dalam jendela ±15 menit dari janji (default 06.00–08.00) | Timestamp scan/foto serah |
| Respons chat pembeli | ≤ 15 menit pada jam aktif | Event chat di mobile KMP |
| Sinkron stok pagi | Selesai sebelum 05.00 (tarik dari Lumbung) | Log sinkron `platform/60-offline-sinkron.md` |

Contoh: pesanan masuk 19.42 → vendor wajib konfirmasi paling lambat 20.12. Jika lewat, pesanan otomatis dialihkan ke vendor cadangan radius 2 km.

## 2. Kualitas

1. **Ketepatan item:** ≥ 98% baris pesanan benar (salah 1 dari 50 baris = 98%).
2. **Foto serah:** 100% pesanan wajib 1 foto (tanpa foto = dianggap belum serah, fee ditahan).
3. **Rating minimum:** ≥ 4,5/5,0 rata-rata 30 hari. Di bawah itu masuk pembinaan 14 hari.
4. **Kemasan:** sayur basah wajib alas daun/plastik ganda; telur wajib egg-tray (disediakan Lumbung, Rp2.000/pcs dipotong fee).

## 3. Ketersediaan

1. Vendor aktif wajib buka ≥ 5 hari/minggu, jam terima 05.00–20.00.
2. Tutup terjadwal wajib di-set H-1 pukul 20.00 di aplikasi (masuk antrean offline jika luring).
3. Kuota harian Varian 1: aktif 30, prioritas 60. Kuota habis → tombol tutup otomatis, tidak bisa override manual.

Contoh antrean luring: vendor di area tanpa sinyal menekan "Tutup besok" jam 19.00 luring → tersimpan di queue `Intent.TutupJadwal` → terkirim saat online jam 19.20 → server mencatat `updated_at` 19.20 tetapi `effective_date` besok (aturan last-write-wins dengan tanggal efektif).

## 4. Denda dan insentif (angka pasti)

| Kejadian | Dampak fee |
|----------|------------|
| Konfirmasi > 30 menit | Peringatan 1; 3x peringatan/minggu = skors 3 hari |
| Batal sepihak < 2 jam | Denda Rp15.000 + voucher pembeli Rp10.000 (kas vendor) |
| Rating 30 hari < 4,5 | Pembinaan, kuota -50% selama 14 hari |
| Repeat-order ≥ 40% (4 minggu) | Naik ke Prioritas, fee +0,5% (lihat `keuangan/10-bagi-fee.md`) |
| 0 pelanggaran 30 hari + ≥ 100 pesanan | Bonus Rp150.000/bulan |

## 5. Contoh perhitungan

Vendor Budi, minggu 5–11 Jan: 120 pesanan, 2 terlambat (>15 menit), 1 batal sepihak.
- Keterlambatan 2x → peringatan tertulis (belum skors, butuh 3x/minggu untuk skors keterlambatan; aturan batal terpisah).
- 1 batal sepihak → denda Rp15.000 + skors 3 hari (aturan piagam).
- Fee minggu itu Rp480.000 → bersih Rp465.000, dibayar H+1 via ledger.

## 6. Eskalasi

1. Otomatis oleh sistem (timer + job Bun) — tanpa keputusan manusia.
2. Banding via chat operasi di web admin maksimal 2x24 jam setelah sanksi, diputus 1x24 jam.
3. Semua sanksi tercatat di `vendor_audit_log` (append-only, tidak bisa dihapus, retensi 2 tahun).

## 7. Kaitan dokumen

- Piagam: `vendor/00-piagam-vendor-terpercaya.md`
- Alur pesanan: `pesanan/10-alur-titip-harian.md`
- Offline/sinkron: `platform/60-offline-sinkron.md`
- Fee: `keuangan/10-bagi-fee.md`
