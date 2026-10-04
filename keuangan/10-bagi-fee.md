# 10 — Bagi Fee TitipO

> **Tujuan:** Menetapkan berapa vendor dapat, berapa TitipO simpan, dan kapan cair — selaras dengan model fee Lumbung.
> **Pemilik:** Tim Keuangan TitipO (pemilik angka), Tim Web Bun (pemilik job cair + ledger).
> **Status:** Varian 1 — persen tetap, cair H+1, tanpa negosiasi per vendor.

## 1. Formula Varian 1 (keputusan konkret)

```
total_item = Σ subtotal (tanpa ongkir)
fee_vendor = 12% × total_item
bonus_prioritas = +0,5% × total_item (hanya status Prioritas)
denda = Rp15.000 per batal sepihak < 2 jam (lihat SLA)
bersih_vendor = fee_vendor + bonus_prioritas − denda
kas_titipo = total_item − bersih_vendor − pokok_lumbung
```

Keterangan: `pokok_lumbung` = harga pokok dari Lumbung per SKU (acuan: https://github.com/Coding-Skuy/Lumbung-TownHall/blob/main/keuangan/model-fee.md). TitipO tidak mengubah harga pokok; margin TitipO = sisa setelah fee vendor dan pokok.

Ongkir Rp5.000 flat: Rp4.000 untuk vendor (pengantar), Rp1.000 kas TitipO (infrastruktur). Tidak masuk `total_item`.

## 2. Contoh hitungan (3 kasus pasti)

**Kasus A — vendor Aktif, tanpa denda:**
Total item Rp47.000 (bayam Rp18.000 + telur Rp29.000). Fee = 12% × 47.000 = Rp5.640. Ongkir vendor Rp4.000. Diterima Sari: Rp9.640.

**Kasus B — vendor Prioritas:**
Total item Rp47.000. Fee 12% Rp5.640 + bonus 0,5% Rp235 = Rp5.875 + ongkir Rp4.000 = Rp9.875.

**Kasus C — batal sepihak 1x:**
Fee Rp5.640 + ongkir Rp4.000 − denda Rp15.000 = minus Rp5.360 → dipotong dari saldo minggu berjalan (saldo tidak boleh negatif; sisa denda dibawa ke minggu depan).

## 3. Pencairan

1. Job `cair-fee` 00.00 WIB menghitung pesanan `selesai` H-1, menulis ledger, saldo bisa ditarik mulai 06.00.
2. Penarikan: tombol di aplikasi vendor → transfer bank/ewallet, biaya transfer Rp2.500 ditanggung vendor, minimal tarik Rp50.000.
3. Deposit Rp100.000 (piagam §2) diakumulasi dari 10% fee pertama hingga genap, bukan potong langsung.

Contoh ledger:
```
2026-01-08 cair: T-2026-000112 fee 5.640 + ongkir 4.000 = 9.640 → saldo Sari 128.400
2026-01-08 tarik: Sari tarik 100.000 (biaya 2.500) → terima 97.500, saldo 28.400
```

## 4. Rekonsiliasi dengan Lumbung

1. Setiap Senin 09.00, TitipO membayar pokok Lumbung minggu lalu via transfer tunggal + berita acara CSV dari `/admin/fee`.
2. Selisih stok (susut > 2%) dilaporkan ke Lumbung dan dibagi 50:50 sebagai kerugian bersama (maks Rp200.000/minggu, selebihnya ditanggung TitipO).

## 5. Kaitan dokumen

- Piagam: `vendor/00-piagam-vendor-terpercaya.md`
- SLA: `vendor/10-sla-vendor.md`
- Acuan Lumbung: https://github.com/Coding-Skuy/Lumbung-TownHall/blob/main/keuangan/model-fee.md
