# TitipO-TownHall — Community Commerce (Penyerap Pasokan Lumbung)

> **Tujuan:** Keputusan tunggal TitipO: menghubungkan rumah tangga dengan vendor terpercaya untuk titip harian, menyerap pasokan Lumbung.
> **Pemilik:** Tim Produk TitipO (pemilik TownHall), Tim Operasi (pelaksana vendor), Tim Mobile KMP + Tim Web Bun (pelaksana teknis).
> **Status:** Varian 1 — Bahasa Indonesia, nol TBD. Semua angka di dokumen adalah keputusan berlaku.

## 1. Peran TitipO

TitipO adalah **Community Commerce** — penyerap pasokan Lumbung. Lumbung memasok bahan baku; TitipO menyalurkannya lewat vendor kurasi (warung, tukang sayur, dapur rumahan) ke rumah tangga dengan pola **titip harian**: pesan maksimal 20.00, serah besok 06.00–08.00, bayar QRIS di muka, fee vendor 12% cair H+1.

Yang bukan TitipO (Varian 1): bukan grosir B2B, bukan instan same-day, bukan marketplace bebas (semua SKU dari sinkron Lumbung, tanpa item manual).

Contoh: Ibu Ani menitip 2 kg bayam + 1 kg telur Rp52.000 ke Warung Sari; Sari ambil pasokan di titik Lumbung Blok A pukul 04.30, serah 06.40 + foto, terima Rp9.640 (fee + ongkir).

## 2. Peta folder

```
TitipO-TownHall/
├── README.md                          # dokumen ini
├── vendor/
│   ├── 00-piagam-vendor-terpercaya.md # syarat, tingkatan, sanksi
│   └── 10-sla-vendor.md               # target waktu, kualitas, denda
├── pesanan/
│   ├── 10-alur-titip-harian.md        # 8 langkah + mesin status
│   └── 20-model-data.md               # skema PostgreSQL + SQLDelight + contoh
├── produk/
│   ├── 10-ux-pesan-titip.md           # 5 layar rumah + 3 layar vendor
│   ├── 20-kontrak-api-KMP-mobile.md   # 7 endpoint mobile (Idempotency-Key)
│   ├── 21-kontrak-api-web-bun.md      # 6 endpoint admin + 4 job
│   └── 30-modul-KMP-bersama.md        # shared/ui-bersama/fitur-rumah/fitur-vendor
├── platform/
│   ├── 10-matriks-KMP-web.md          # siapa membangun apa
│   ├── 40-mobile-KMP.md               # Android 8+/iOS 16+, offline-first
│   ├── 50-web-bun-svelte.md           # Bun 1.4 + Svelte 5 + SvelteKit 2 + TS 5.9
│   └── 60-offline-sinkron.md          # antrean + konflik first-arrival-wins
├── keuangan/
│   └── 10-bagi-fee.md                 # fee 12%, bonus 0,5%, denda Rp15.000
└── metrik/
    └── 10-repeat-order.md              # bintang utara repeat ≥ 40%
```

## 3. Stack (dikunci Varian 1)

- **Mobile:** Kotlin Multiplatform + Compose Multiplatform 1.7.x + Navigation3 (Android 8+ / iOS 16+, 1 codebase 2 role: vendor + rumah tangga). Offline-first queue pesanan untuk vendor.
- **Web admin:** Bun 1.4.x + Svelte 5 + SvelteKit 2 + TypeScript 5.9.x + PostgreSQL 16.
- **Tanpa desktop.** Tanpa web untuk pembeli/vendor.
- **Stok/katalog disinkron dari Lumbung:** job Bun 04.00 WIB → snapshot cermin; mobile tarik + stempel 24 jam; checkout menolak SKU kedaluwarsa (409).

## 4. Tautan ke Lumbung-TownHall

- Acuan model fee / harga pokok: https://github.com/Coding-Skuy/Lumbung-TownHall/blob/main/keuangan/model-fee.md
- Aturan rekonsiliasi mingguan (Senin 09.00, susut 50:50 maks Rp200.000/minggu): `keuangan/10-bagi-fee.md` §4.
- Semua `sku_lumbung` di TitipO merujuk katalog Lumbung; TitipO tidak membuat SKU sendiri.

## 5. Mulai dari sini (urutan baca)

1. `vendor/00-piagam-vendor-terpercaya.md` → siapa boleh jualan.
2. `pesanan/10-alur-titip-harian.md` → bagaimana titip berjalan.
3. `produk/20-kontrak-api-KMP-mobile.md` + `produk/30-modul-KMP-bersama.md` → bangun mobile.
4. `platform/60-offline-sinkron.md` → pastikan luring benar.
5. `keuangan/10-bagi-fee.md` + `metrik/10-repeat-order.md` → ukur uang dan loyalitas.
