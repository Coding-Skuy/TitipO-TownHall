# 60 — Offline & Sinkron TitipO

> **Tujuan:** Aturan tunggal antre luring dan sinkronisasi agar vendor tetap bisa kerja tanpa sinyal tanpa merusak stok.
> **Pemilik:** Tim Mobile KMP (pemilik antrean) + Tim Web Bun (pemilik snapshot + resolusi server).
> **Status:** Varian 1 — last-write-wins dengan tanggal efektif untuk jadwal, first-arrival-wins untuk stok.

## 1. Apa yang bisa antre luring (keputusan konkret)

| Aksi | Bisa luring? | Batas |
|------|--------------|-------|
| Konfirmasi terima/tolak | Ya | 50 aksi / 24 jam |
| Tutup/buka jadwal H-1 | Ya | 5 perubahan/minggu |
| Foto serah | Ya (simpan lokal) | 20 foto, ≤ 800 KB/foto |
| Checkout rumah tangga | Tidak | Wajib online |
| Pembayaran QRIS | Tidak | Wajib online |
| Rating | Ya (1 aksi) | Terkirim ≤ 24 jam |

## 2. Format antrean lokal (SQLDelight)

```sql
CREATE TABLE antrean (
  kunci TEXT PRIMARY KEY,        -- ULID Idempotency-Key
  jenis TEXT NOT NULL,           -- KONFIRMASI | TUTUP_JADWAL | SERAH | RATING
  kode_pesanan TEXT, muatan TEXT NOT NULL,
  dibuat INTEGER NOT NULL, percobaan INTEGER NOT NULL DEFAULT 0,
  sinkron INTEGER NOT NULL DEFAULT 0
);
```

Contoh baris: `K-001 | KONFIRMASI | T-2026-000112 | {"terima":true} | 1767824100 | 0 | 0`.

## 3. Sinkronisasi (urutan pasti)

1. Saat online, kirim FIFO satu per satu dengan header `Idempotency-Key: <kunci>`; jeda 1 detik antar kirim.
2. Retry gagal jaringan tiap 30 detik, maks 24 jam; setelah itu tandai `gagal` dan tampilkan tombol "Kirim ulang manual".
3. Snapshot katalog: mobile menyimpan `versi_snapshot` + `diperbarui`; jika umur > 24 jam, tombol pesan dikunci hingga tarik ulang (GET `/katalog/{id}` ≤ 5 MB).
4. Sinkron Lumbung (server 04.00): tarik → validasi 48 SKU → tulis snapshot `vNNN` → mobile tarik berikutnya. Jika job gagal 3x, snapshot kemarin tetap berlaku + banner merah di admin.

## 4. Resolusi konflik (tanpa TBD)

1. **Rebut stok:** 2 vendor konfirmasi luring atas stok terakhir yang sama → yang tiba di server lebih dulu menang; yang kalah mendapat 409 `STOK_HABIS` dan pesanan otomatis dialihkan ke vendor cadangan (1 dari 2 jatah pengalihan).
2. **Jadwal tutup:** last-write-wins berdasarkan `dibuat` di HP, tetapi `effective_date` = tanggal yang dipilih (bukan tanggal kirim). Contoh di SLA §3.
3. **Foto ganda:** jika 2 foto untuk 1 pesanan, foto terakhir menang, foto lama diarsipkan (tidak dihapus).

Contoh konflik: Sari dan Budi sama-sama terima pesanan yang butuh 10 kg telur terakhir (stok 10 kg). Sari tiba 19.56, Budi tiba 19.58 → Sari menang; pesanan Budi dialihkan ke vendor Cici otomatis + notif "Dialihkan — stok habis."

## 5. Kaitan dokumen

- Alur: `pesanan/10-alur-titip-harian.md`
- Kontrak mobile: `produk/20-kontrak-api-KMP-mobile.md`
- Modul KMP: `produk/30-modul-KMP-bersama.md`
- SLA: `vendor/10-sla-vendor.md`
