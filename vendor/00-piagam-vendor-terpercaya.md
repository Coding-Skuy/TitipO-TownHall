# 00 — Piagam Vendor Terpercaya TitipO

> **Tujuan:** Menetapkan siapa yang boleh menjadi vendor TitipO, apa yang dijanjikan kepada rumah tangga, dan bagaimana kepercayaan ditegakkan. Dokumen ini adalah rujukan tunggal untuk semua keputusan vendor.
> **Pemilik:** Tim Operasi TitipO (pemilik piagam), didukung Tim Katalog Lumbung (pemilik data pasokan).
> **Status:** Varian 1 — berlaku sejak commit awal. Perubahan butuh PR dan persetujuan pemilik.
> **Stack terkait:** Mobile KMP (2 role: vendor + rumah tangga), sinkron katalog dari Lumbung, offline-first queue pesanan vendor.

## 1. Peran TitipO

TitipO adalah **Community Commerce** — penyerap pasokan Lumbung yang menghubungkan rumah tangga dengan vendor terpercaya. TitipO tidak memproduksi bahan baku; TitipO menyalurkan stok Lumbung lewat vendor kurasi (tukang sayur keliling, warung, dapur rumahan) ke rumah tangga dalam pola **titip harian** (pesan hari ini, terima besok pagi).

Batas peran (keputusan konkret):
1. TitipO hanya menjual item yang ada di katalog Lumbung yang disinkron (tidak ada item manual bebas).
2. TitipO tidak melayani B2B grosir — hanya rumah tangga (maks 20 kg / 50 item per pesanan).
3. Semua vendor wajib lolos kurasi — tidak ada pendaftaran otomatis langsung jualan.

## 2. Kriteria vendor (keputusan konkret)

Varian 1 menetapkan 5 syarat lolos, tanpa pengecualian:

| # | Syarat | Bukti | Verifikator |
|---|--------|-------|-------------|
| 1 | KTP + domisili radius 5 km dari titik serah Lumbung | Foto KTP + share-loc | Operasi |
| 2 | Memiliki Android 9+ / iPhone iOS 16+ untuk aplikasi KMP | Device-check saat onboarding | Otomatis (aplikasi) |
| 3 | Lolos uji kebersihan (foto lapak/dapur + checklist 10 poin, skor ≥ 80) | Form checklist + 3 foto | Operasi |
| 4 | Setuju SLA vendor (lihat `vendor/10-sla-vendor.md`) + tanda tangan digital | E-sign di aplikasi | Otomatis |
| 5 | Deposit awal Rp100.000 (dipotong dari fee, bukan transfer tunai) | Ledger fee | Keuangan |

Contoh: Ibu Sari (warung, Sleman) daftar 3 Jan → upload KTP + 3 foto → skor kebersihan 90 → e-sign SLA → status `calon`. Setelah deposit terakumulasi dari 5 pesanan pertama, status naik `aktif`.

## 3. Tingkatan vendor

1. **Calon** — baru daftar, belum bisa terima pesanan. Katalog terlihat tapi tombol terima terkunci.
2. **Aktif** — lolos 5 syarat, bisa terima maksimal 30 pesanan titip/hari.
3. **Prioritas** — repeat-order ≥ 40% selama 4 minggu berturut + rating ≥ 4,7. Kuota 60 pesanan/hari + badge emas + fee +0,5%.
4. **Skors** — melanggar SLA (lihat sanksi). Tidak bisa terima pesanan 3–7 hari.
5. **Keluar** — 3x skors dalam 90 hari atau pelanggaran berat (pemalsuan stok). Deposit hangus masuk kas bersama.

## 4. Janji ke rumah tangga

1. Harga di aplikasi = harga bayar (tidak ada ongkir siluman; ongkir tampil terpisah Rp5.000 flat radius 5 km).
2. Jika pesanan dibatalkan vendor < 2 jam sebelum serah, rumah tangga dapat voucher Rp10.000 otomatis.
3. Foto barang saat serah wajib diunggah vendor (bukti visual 1 foto per pesanan).

Contoh voucher: pesanan #T-2026-0112 dibatalkan vendor jam 05.30 untuk serah jam 07.00 → sistem menerbitkan voucher `VCH-BATAL-0112` Rp10.000 ke akun rumah tangga, berlaku 14 hari.

## 5. Sanksi (konkret, bukan TBD)

| Pelanggaran | Sanksi Varian 1 |
|-------------|-----------------|
| Terlambat serah > 30 menit, 2x/minggu | Skors 3 hari + pembinaan via modul KMP |
| Batal sepihak < 2 jam sebelum serah | Denda Rp15.000 dipotong fee + voucher pembeli otomatis |
| Stok fiktif (terima pesanan melebihi stok Lumbung tersinkron) | Skors 7 hari, kuota turun ke 15/hari selama 30 hari |
| Pemalsuan foto serah | Status `keluar`, deposit hangus |

## 6. Contoh siklus hidup vendor

```
calon (daftar) → verifikasi 1x24 jam → aktif (kuota 30/hari)
  → 4 minggu repeat ≥40% → prioritas (kuota 60/hari, fee +0,5%)
  → 2x terlambat → skors 3 hari → aktif kembali (kuota 15/hari 14 hari)
  → 3x skors/90 hari → keluar
```

## 7. Kaitan dokumen

- SLA operasional: `vendor/10-sla-vendor.md`
- Alur pesanan: `pesanan/10-alur-titip-harian.md`
- Model data: `pesanan/20-model-data.md`
- Bagi fee: `keuangan/10-bagi-fee.md`
- Acuan pasokan Lumbung: https://github.com/Coding-Skuy/Lumbung-TownHall/blob/main/keuangan/model-fee.md
