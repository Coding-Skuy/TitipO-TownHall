# 20 — Model Data Pesanan TitipO

> **Tujuan:** Skema data tunggal untuk pesanan, item, pembayaran, dan audit agar mobile KMP dan web Bun membaca/menulis struktur yang sama.
> **Pemilik:** Tim Produk (pemilik skema), Tim Web Bun (pemilik migrasi PostgreSQL), Tim KMP (pemilik skema SQLDelight lokal).
> **Status:** Varian 1 — PostgreSQL 16 server, SQLDelight 2.x lokal. Tidak ada field opsional yang belum diputuskan.

## 1. Keputusan penyimpanan (konkret)

1. Server (web Bun): PostgreSQL 16, tabel di bawah. ID memakai ULID string 26 karakter (misal `01JMB2...`), bukan auto-increment, agar ID luring bisa dibuat di HP tanpa tabrakan.
2. Lokal (mobile KMP): SQLDelight, 4 tabel cermin (`pesanan`, `item_pesanan`, `antrean`, `foto_serah`) + kolom `sinkron: 0/1`.
3. Uang memakai integer rupiah (bukan float). Berat memakai integer gram.
4. Waktu memakai `timestamptz` UTC; tampilan WIB (UTC+7) hanya di UI.

## 2. Tabel server (DDL ringkas Varian 1)

```sql
CREATE TABLE vendor (
  id TEXT PRIMARY KEY, nama TEXT NOT NULL, status TEXT NOT NULL
    CHECK (status IN ('calon','aktif','prioritas','skors','keluar')),
  kuota_harian INT NOT NULL DEFAULT 30, rating_30h NUMERIC(3,2) NOT NULL DEFAULT 0,
  titik_serah_lumbung TEXT NOT NULL, dibuat TIMESTAMP TZ NOT NULL DEFAULT now()
);
CREATE TABLE pesanan (
  id TEXT PRIMARY KEY, kode TEXT UNIQUE NOT NULL, -- misal T-2026-000112
  id_rumah_tangga TEXT NOT NULL, id_vendor TEXT NOT NULL REFERENCES vendor(id),
  status TEXT NOT NULL CHECK (status IN
    ('menunggu_pembayaran','menunggu_konfirmasi','disiapkan','diserahkan','selesai',
     'kedaluwarsa','dialihkan','dibatalkan_sistem','dibatalkan_vendor')),
  total_item INT NOT NULL, ongkir INT NOT NULL DEFAULT 5000,
  total_bayar INT NOT NULL, jendela_serah TEXT NOT NULL DEFAULT '06.00-08.00',
  dibuat TIMESTAMP TZ NOT NULL DEFAULT now(),
  dibayar TIMESTAMP TZ, dikonfirmasi TIMESTAMP TZ, diserahkan TIMESTAMP TZ
);
CREATE TABLE item_pesanan (
  id TEXT PRIMARY KEY, id_pesanan TEXT NOT NULL REFERENCES pesanan(id),
  sku_lumbung TEXT NOT NULL, nama TEXT NOT NULL,
  jumlah_gram INT NOT NULL CHECK (jumlah_gram > 0),
  harga_per_kg INT NOT NULL, subtotal INT NOT NULL
);
CREATE TABLE pembayaran (
  id TEXT PRIMARY KEY, id_pesanan TEXT UNIQUE NOT NULL REFERENCES pesanan(id),
  metode TEXT NOT NULL DEFAULT 'QRIS', status TEXT NOT NULL
    CHECK (status IN ('menunggu','lunas','kedaluwarsa','refund')),
  id_gateway TEXT, dibayar TIMESTAMP TZ
);
CREATE TABLE audit_pesanan (
  id TEXT PRIMARY KEY, id_pesanan TEXT NOT NULL REFERENCES pesanan(id),
  dari_status TEXT, ke_status TEXT NOT NULL, aktor TEXT NOT NULL,
  waktu TIMESTAMP TZ NOT NULL DEFAULT now()
);
```

## 3. Contoh baris (konkret)

```json
{
  "id": "01JMB2Z9KQ0ABCD1234567890",
  "kode": "T-2026-000112",
  "id_rumah_tangga": "RT-ANI-001",
  "id_vendor": "V-SARI-001",
  "status": "menunggu_konfirmasi",
  "total_item": 47000,
  "ongkir": 5000,
  "total_bayar": 52000,
  "jendela_serah": "06.00-08.00",
  "item": [
    {"sku_lumbung": "LMB-BAYAM-001", "nama": "Bayam 2 kg", "jumlah_gram": 2000, "harga_per_kg": 9000, "subtotal": 18000},
    {"sku_lumbung": "LMB-TELUR-001", "nama": "Telur 1 kg", "jumlah_gram": 1000, "harga_per_kg": 29000, "subtotal": 29000}
  ]
}
```

## 4. Aturan validasi (ditegakkan server, bukan hanya UI)

1. `total_bayar = total_item + ongkir`; `subtotal = jumlah_gram * harga_per_kg / 1000` (pembulatan ke bawah rupiah).
2. Maks 50 baris item dan 20 kg per pesanan — ditolak 422 jika lebih.
3. `sku_lumbung` wajib ada di snapshot katalog tersinkron ≤ 24 jam; jika tidak, tolak 409 `STOK_KEDALUWARSA`.
4. Transisi status hanya lewat mesin di `pesanan/10-alur-titip-harian.md`; transisi ilegal ditolak 422 dan dicatat di `audit_pesanan`.

## 5. Retensi dan audit

1. `audit_pesanan` append-only (REVOKE UPDATE/DELETE untuk role aplikasi).
2. Foto serah disimpan di object storage `titipo-bukti/` dengan nama `{id_pesanan}.jpg`, maks 2 MB, retensi 2 tahun.
3. Data pesanan selesai diarsipkan setelah 2 tahun ke tabel `pesanan_arsip` oleh job Bun bulanan.

## 6. Kaitan dokumen

- Alur: `pesanan/10-alur-titip-harian.md`
- Kontrak API mobile: `produk/20-kontrak-api-KMP-mobile.md`
- Kontrak API web: `produk/21-kontrak-api-web-bun.md`
- Offline: `platform/60-offline-sinkron.md`
