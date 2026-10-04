# 20 — Kontrak API Mobile KMP TitipO

> **Tujuan:** Kontrak HTTP tunggal yang dipakai aplikasi Kotlin Multiplatform (Android+iOS) untuk menitip, membayar, konfirmasi, dan serah. Server diimplementasikan web Bun.
> **Pemilik:** Tim Web Bun (pemilik server), Tim Mobile KMP (pemilik klien + DTO bersama).
> **Status:** Varian 1 — base URL `https://api.titipo.id/v1`, auth Bearer JWT 24 jam, versioning via path.

## 1. Aturan umum (keputusan konkret)

1. Format JSON UTF-8; uang integer rupiah; berat integer gram; waktu ISO-8601 UTC (`2026-01-07T00:30:00Z`).
2. ID klien (ULID) dibuat di HP untuk semua POST agar retry luring aman (idempotensi via header `Idempotency-Key: <ulid>`).
3. Error baku: `{ "kode": "STOK_KEDALUWARSA", "pesan": "...", "detail": {} }` dengan HTTP 400/401/404/409/422.
4. Paginasi: `?batas=20&lanjut=<kode_terakhir>`, respons `{ "data": [], "lanjut": null }`. Batas maks 50.

## 2. Endpoint (7 endpoint Varian 1, tanpa tambahan)

### 2.1 GET /vendor-terdekat — daftar vendor
- Query: `lat, lng, radius_km=5`
- Respons 200 contoh:
```json
{"data":[{"id":"V-SARI-001","nama":"Warung Sari","jarak_km":0.8,"rating":4.9,"sisa_kuota":12,"prioritas":true}]}
```

### 2.2 GET /katalog/{idVendor} — katalog tersinkron Lumbung
- Respons 200 contoh:
```json
{"diperbarui":"2026-01-07T04:30:00Z","data":[
  {"sku_lumbung":"LMB-BAYAM-001","nama":"Bayam","harga_per_kg":9000,"stok_gram":50000},
  {"sku_lumbung":"LMB-TELUR-001","nama":"Telur","harga_per_kg":29000,"stok_gram":30000}]}
```
- 409 `KATALOG_KEDALUWARSA` jika snapshot > 24 jam.

### 2.3 POST /pesanan — checkout (wajib online)
- Header: `Idempotency-Key`
- Body contoh:
```json
{"id":"01JMB2Z9KQ0ABCD1234567890","id_vendor":"V-SARI-001",
 "item":[{"sku_lumbung":"LMB-BAYAM-001","jumlah_gram":2000},{"sku_lumbung":"LMB-TELUR-001","jumlah_gram":1000}]}
```
- Respons 201: `{ "kode": "T-2026-000112", "total_bayar": 52000, "kedaluwarsa_bayar": "2026-01-06T13:15:00Z" }`

### 2.4 POST /pesanan/{kode}/bayar — konfirmasi bayar QRIS
- Body: `{ "id_gateway": "QR-67890" }` → 200 `{ "status": "menunggu_konfirmasi" }`

### 2.5 POST /vendor/pesanan/{kode}/konfirmasi — terima/tolak (bisa antre luring)
- Body: `{ "terima": true }` atau `{ "terima": false, "alasan": "Stok habis" }`
- Respons 200 contoh: `{ "status": "disiapkan", "dikonfirmasi": "2026-01-06T12:55:00Z" }`
- Saat luring, KMP menyimpan ke tabel `antrean` lalu POST ulang dengan `Idempotency-Key` sama.

### 2.6 POST /vendor/pesanan/{kode}/serah — foto serah
- Multipart: `foto` (JPEG ≤ 2 MB) + JSON `{ "waktu": "..." }`
- Respons 200: `{ "status": "diserahkan", "url_foto": "https://cdn.titipo.id/titipo-bukti/T-2026-000112.jpg" }`

### 2.7 POST /pesanan/{kode}/rating — rating rumah tangga
- Body: `{ "bintang": 5, "catatan": "Segar" }` (bintang 1–5, catatan ≤ 200 karakter)

## 3. Contoh urutan retry luring

```
HP luring: konfirmasi Terima (ULID K-001) → simpan antrean
Online: POST /konfirmasi Idempotency-Key: K-001 → 200 disiapkan
Retry ganda (sinyal putus): POST ulang K-001 → 200 sama (tidak ganda)
```

## 4. Kaitan dokumen

- Model data: `pesanan/20-model-data.md`
- Offline: `platform/60-offline-sinkron.md`
- Modul bersama: `produk/30-modul-KMP-bersama.md`
- API web admin: `produk/21-kontrak-api-web-bun.md`
