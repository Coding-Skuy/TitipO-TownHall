# 10 — UX Pesan Titip TitipO

> **Tujuan:** Menetapkan layar, langkah, dan aturan tampilan agar rumah tangga bisa menitip dalam < 90 detik dan vendor bisa konfirmasi dalam 2 ketukan, di Android dan iOS dari 1 codebase.
> **Pemilik:** Tim Produk (pemilik alur layar), Tim Mobile KMP (pemilik implementasi Compose Multiplatform + Navigation3).
> **Status:** Varian 1 — 5 layar rumah tangga + 3 layar vendor, tanpa mode gelap khusus di Varian 1 (mengikuti sistem).

## 1. Prinsip UX (keputusan konkret)

1. **Satu vendor satu layar:** daftar vendor diurut jarak terdekat, menampilkan rating, estimasi serah (selalu "Besok 06.00–08.00" di Varian 1), dan sisa kuota ("Sisa 12 titipan").
2. **Harga jujur:** setiap layar checkout menampilkan rincian `Total item + Ongkir Rp5.000 = Total bayar`. Tidak ada biaya muncul belakangan.
3. **2 ketukan vendor:** push notif → layar detail → tombol Terima/Tolak (min 48 dp, kontras 4,5:1).
4. **Luring jelas:** banner kuning "Luring — aksi akan dikirim otomatis" + badge jumlah antrean di semua layar vendor.

## 2. Layar rumah tangga (5 layar pasti)

| # | Route Navigation3 | Isi | Aturan |
|---|-------------------|-----|--------|
| 1 | `rumah/daftar-vendor` | Daftar vendor radius 5 km, search, filter Prioritas | Skeleton 3 kartu saat muat; kosong → ilustrasi + tombol "Coba radius 10 km" (tetap ongkir Rp5.000) |
| 2 | `rumah/katalog/{idVendor}` | Katalog tersinkron (stempel "Diperbarui 04.30"), stepper kg | Stok 0 → tombol mati "Habis"; harga per kg + contoh subtotal 1 kg |
| 3 | `rumah/keranjang` | Item, ubah jumlah, pilih jendela (terkunci 06.00–08.00 di Varian 1) | Maks 20 kg divalidasi inline ("Kelebihan 2 kg, kurangi") |
| 4 | `rumah/bayar` | QRIS + timer 15:00 mundur, tombol "Saya sudah bayar" | Timer habis → layar kedaluwarsa + tombol buat ulang (ID baru) |
| 5 | `rumah/lacak/{kode}` | Timeline 8 langkah + foto serah + tombol rating 1–5 | Rating wajib pilih sebelum tombol "Selesai" aktif |

Contoh teks layar bayar: "Pindai QRIS Rp52.000 — Bayam 2 kg (Rp18.000) + Telur 1 kg (Rp29.000) + Ongkir Rp5.000. Berlaku 14:32."

## 3. Layar vendor (3 layar pasti)

| # | Route | Isi | Aturan |
|---|-------|-----|--------|
| 1 | `vendor/masuk` | Daftar titip hari ini: kartu pesanan (kode, item ringkas, total, timer 30 menit) | Geser kanan = Terima, geser kiri = Tolak + alasan (Stok habis/Tutup). Bisa luring (masuk antrean). |
| 2 | `vendor/kemas` | Checklist ambil di titik Lumbung + tombol "Siap serah" | Wajib centang semua item sebelum tombol aktif |
| 3 | `vendor/serah/{kode}` | Tombol kamera, pratinjau foto, tombol "Selesai serah" | Foto wajib (kamera 1280px, kompres ≤ 800 KB). Tanpa foto tombol mati. |

Contoh push: "Titipan baru T-2026-000112 — Rp52.000 — konfirmasi sebelum 20.12 (12 menit)."

## 4. Keadaan kosong / gagal (konkret)

1. Katalog belum sinkron (> 24 jam): tampil banner merah "Katalog kedaluwarsa — tarik untuk muat ulang", tombol pesan mati.
2. Gagal bayar: snackbar "Pembayaran gagal (kode G-102). Coba lagi." + tombol ulangi (maks 3x lalu buat QR baru).
3. Luring vendor saat konfirmasi: toast "Tersimpan — terkirim otomatis saat online (antrean 2)."

## 5. Contoh perjalanan 90 detik

```
00:00 buka daftar-vendor → 00:12 pilih Sari (4,9★, sisa 12)
→ 00:35 tambah bayam 2 kg + telur 1 kg → 00:50 keranjang (Rp52.000)
→ 01:05 pindai QRIS → 01:20 lunas → layar lacak "Menunggu konfirmasi Sari (28 menit)"
```

## 6. Kaitan dokumen

- Alur: `pesanan/10-alur-titip-harian.md`
- Modul KMP: `produk/30-modul-KMP-bersama.md`
- Mobile KMP: `platform/40-mobile-KMP.md`
