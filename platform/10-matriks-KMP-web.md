# 10 — Matriks KMP vs Web TitipO

> **Tujuan:** Menegaskan apa dibangun di KMP mobile vs web Bun agar tidak ada fitur ganda atau celah tanggung jawab.
> **Pemilik:** Tim Produk (pemilik matriks). Sengketa diputus pemilik dalam 1x24 jam.
> **Status:** Varian 1 — tanpa desktop, tanpa web untuk rumah tangga/vendor.

## 1. Matriks (keputusan konkret, √ = pemilik)

| Kemampuan | Mobile KMP (Android+iOS) | Web Bun + SvelteKit (admin) | Catatan |
|-----------|--------------------------|-----------------------------|---------|
| Lihat vendor + katalog | √ | — | Mobile baca snapshot cermin |
| Checkout + bayar QRIS | √ | — | Wajib online, maks 20 kg |
| Konfirmasi/tolak titipan | √ (bisa luring) | — | Antrean offline hanya di mobile |
| Foto serah | √ | — | Wajib kamera HP |
| Rating | √ | — | 1–5 + catatan 200 karakter |
| Kurasi calon vendor | — | √ | Setuju/tolak + kuota |
| Skors / alihkan manual | — | √ | Hari 3/7, alihkan maks 2x |
| Sinkron Lumbung 04.00 | — (baca hasil) | √ (job Bun) | Retry 3x, stempel 24 jam |
| Cair fee H+1 | — (lihat saldo) | √ (job Bun) | Ledger PostgreSQL |
| Laporan repeat-order | — (ringkas) | √ (penuh) | Lihat `metrik/10-repeat-order.md` |
| Auth rumah/vendor | √ (JWT peran) | — | — |
| Auth admin/operasi | — | √ (JWT admin) | — |

## 2. Aturan anti-ganda

1. Tidak ada layar admin di mobile dan tidak ada checkout di web — pelanggaran ditolak di review.
2. Cache katalog mobile 24 jam; web menyimpan kebenaran snapshot. Jika beda, web menang.
3. Perhitungan uang (subtotal, fee) yang sah adalah perhitungan server Bun; hitungan mobile hanya pratinjau.

Contoh sengketa: "Tombol alihkan vendor di mobile?" Jawab matriks: tidak — hanya di web admin (`produk/21-kontrak-api-web-bun.md` §2.5).

## 3. Contoh aliran lintas platform

```
04.00 web job sinkron Lumbung → snapshot cermin v112
04.30 mobile Sari tarik v112 (online 20 detik) → katalog siap luring
19.42 mobile Ani checkout → server Bun kunci stok
19.55 mobile Sari konfirmasi (luring → antre → terkirim)
00.00 web job cair fee → mobile Sari lihat saldo bertambah
```

## 4. Kaitan dokumen

- Mobile: `platform/40-mobile-KMP.md`
- Web: `platform/50-web-bun-svelte.md`
- Offline: `platform/60-offline-sinkron.md`
