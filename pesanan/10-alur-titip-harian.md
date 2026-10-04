# 10 — Alur Titip Harian TitipO

> **Tujuan:** Menetapkan langkah pasti dari rumah tangga menitip hingga serah terima, termasuk peran vendor, Lumbung, dan sistem. Satu alur untuk 2 role dalam 1 aplikasi KMP.
> **Pemilik:** Tim Produk TitipO (pemilik alur), Tim Operasi (pemilik jam operasional).
> **Status:** Varian 1 — jendela titip tetap, tidak ada varian kilat di Varian 1.

## 1. Prinsip (keputusan konkret)

1. **Titip harian, bukan instan:** pesan maksimal pukul 20.00, serah besok 06.00–08.00. Tidak ada same-day di Varian 1.
2. **Stok dari Lumbung:** vendor tidak input stok manual; stok = hasil sinkron Lumbung pukul 04.00–05.00.
3. **Satu keranjang satu vendor:** rumah tangga memilih 1 vendor per pesanan (tidak bisa campur 2 vendor dalam 1 checkout).
4. **Pembayaran di muka:** QRIS/transfer via gateway, dana ditahan (escrow) hingga foto serah diunggah.

## 2. Langkah alur (8 langkah pasti)

| # | Aktor | Aksi | Batas waktu | Contoh |
|---|-------|------|-------------|--------|
| 1 | Rumah tangga | Pilih vendor + isi keranjang dari katalog tersinkron | 05.00–20.00 | Ibu Ani pilih Vendor Sari, 2 kg bayam + 1 kg telur |
| 2 | Sistem | Kunci stok (reservasi) + buat pesanan `menunggu_konfirmasi` | Instan | Stok bayam Sari 50→48 kg (reservasi 2 kg) |
| 3 | Rumah tangga | Bayar via QRIS (kedaluwarsa 15 menit) | 15 menit | QR Rp47.000 (Rp42.000 + ongkir Rp5.000) |
| 4 | Vendor | Konfirmasi (terima/tolak) di aplikasi KMP (bisa luring → antre) | ≤ 30 menit | Sari tekan Terima (luring, masuk queue, terkirim 5 menit kemudian) |
| 5 | Vendor | Ambil/kemas dari pasokan Lumbung pagi | 04.00–06.00 besok | Sari ambil 2 kg bayam di titik serah Lumbung Blok A |
| 6 | Vendor | Serah ke rumah tangga + foto bukti | 06.00–08.00 | Foto 1x, tekan Selesai |
| 7 | Rumah tangga | Konfirmasi terima + rating (otomatis selesai H+1 12.00 jika diam) | ≤ 30 jam | Ani beri 5 bintang |
| 8 | Sistem | Cairkan fee vendor H+1, catat repeat-order | H+1 00.00 job Bun | Fee Sari Rp6.300 masuk ledger |

## 3. Status pesanan (mesin status tunggal)

```
menunggu_pembayaran → (bayar) → menunggu_konfirmasi → (terima) → disiapkan
  → (serah+foto) → diserahkan → (rating/otomatis) → selesai
Cabang: menunggu_pembayaran --(15 mnt habis)--> kedaluwarsa
       menunggu_konfirmasi --(tolak/timeout 30 mnt)--> dialihkan (ke vendor cadangan) maks 2x --> dibatalkan_sistem (refund otomatis)
       disiapkan --(batal vendor <2 jam)--> dibatalkan_vendor (denda+voucher)
```

Keputusan konkret: timeout konfirmasi 30 menit dieksekusi job Bun tiap 1 menit (`*/1 * * * *`), bukan cron mobile. Pengalihan otomatis maksimal 2 kali, lalu refund penuh H+1.

## 4. Contoh nyata ujung-ke-ujung

- 6 Jan 19.42: Ani checkout 2 kg bayam (Rp12.000/kg) + 1 kg telur (Rp28.000/kg) + ongkir Rp5.000 = Rp57.000? Koreksi harga Varian 1: bayam Rp9.000/kg, telur Rp29.000/kg → total Rp18.000+Rp29.000+Rp5.000 = Rp52.000.
- 19.42–19.57: QRIS lunas 19.50.
- 19.55: Sari konfirmasi (online).
- 7 Jan 04.30: sinkron Lumbung menegaskan stok; 06.40: Sari serah + foto.
- 7 Jan 09.00: Ani rating 5. 8 Jan 00.00: fee Sari cair (lihat `keuangan/10-bagi-fee.md`).

## 5. Aturan offline (ringkas, detail di `platform/60-offline-sinkron.md`)

1. Aksi vendor yang bisa antre luring: konfirmasi, tolak, tutup jadwal, foto serah (foto disimpan lokal, diunggah saat online).
2. Aksi yang tidak bisa luring: checkout dan pembayaran rumah tangga (wajib online).
3. Konflik (2 vendor klaim stok sama saat luring) dimenangkan oleh konfirmasi yang tiba di server lebih dulu; yang kalah otomatis dialihkan.

## 6. Kaitan dokumen

- Model data: `pesanan/20-model-data.md`
- UX pesan: `produk/10-ux-pesan-titip.md`
- Offline: `platform/60-offline-sinkron.md`
- Metrik: `metrik/10-repeat-order.md`
