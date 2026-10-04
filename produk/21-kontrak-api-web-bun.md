# 21 — Kontrak API Web Bun TitipO (Admin)

> **Tujuan:** Kontrak HTTP untuk web admin (SvelteKit) — kurasi vendor, pantau pesanan, kelola katalog cermin, dan jalankan job. Tidak dipakai aplikasi mobile.
> **Pemilik:** Tim Web Bun (pemilik server + UI admin).
> **Status:** Varian 1 — base URL sama `https://api.titipo.id/v1/admin`, auth JWT role `admin` + `operasi`, semua aksi tulis wajib catat aktor.

## 1. Aturan umum

1. Auth: Bearer JWT dengan klaim `peran: admin|operasi`. Endpoint tulis butuh `admin`; baca boleh `operasi`.
2. Semua respons tulis mengembalikan `{ "ok": true, "id": "...", "dicatat_oleh": "admin@titipo.id" }`.
3. Rate-limit admin: 600 req/menit/IP (Bun `Bun.serve` + middleware).
4. Job terjadwal (Bun cron): `sinkron-lumbung 04.00`, `timeout-konfirmasi tiap 1 menit`, `cair-fee 00.00`, `arsip bulanan tanggal 1`.

## 2. Endpoint (6 endpoint Varian 1)

### 2.1 GET /admin/vendor?status=calon — antrean kurasi
- Respons 200 contoh:
```json
{"data":[{"id":"V-BUDI-002","nama":"Warung Budi","skor_kebersihan":85,"dokumen":["ktp.jpg","lapak1.jpg"]}],"lanjut":null}
```

### 2.2 POST /admin/vendor/{id}/verifikasi — setujui/tolak calon
- Body: `{ "setuju": true, "kuota_harian": 30 }` atau `{ "setuju": false, "alasan": "Skor < 80" }`
- Efek: `calon → aktif` atau tetap `calon` dengan catatan; dicatat di audit.

### 2.3 POST /admin/vendor/{id}/skors — skorsing
- Body contoh: `{ "hari": 3, "alasan": "Terlambat 3x/minggu" }` (hari 3 atau 7, tanpa nilai lain di Varian 1).

### 2.4 GET /admin/pesanan?status=disiapkan&hari=2026-01-07 — pantau harian
- Respons: daftar + agregat `{ "total": 132, "terlambat": 4, "batal": 1 }`.

### 2.5 POST /admin/pesanan/{kode}/alihkan — alihkan manual ke vendor cadangan
- Body: `{ "id_vendor_cadangan": "V-SARI-001" }` (maks 2x per pesanan, ditolak 422 jika lebih).

### 2.6 GET /admin/katalog-cermin — snapshot Lumbung terakhir
- Respons contoh:
```json
{"diperbarui":"2026-01-07T04:00:00Z","sumber":"lumbung","jumlah_sku":48,"status":"ok"}
```
- Tombol "Sinkron ulang" memanggil `POST /admin/katalog-cermin/sinkron` (job ≤ 5 menit, retry 3x interval 30 detik).

## 3. Contoh kurasi ujung-ke-ujung

```
GET /admin/vendor?status=calon → [V-BUDI-002 skor 85]
→ POST /admin/vendor/V-BUDI-002/verifikasi {"setuju":true,"kuota_harian":30}
→ 200 {"ok":true,"id":"V-BUDI-002","dicatat_oleh":"admin@titipo.id"}
→ vendor muncul di GET /vendor-terdekat mobile dalam 1 menit (cache 60 detik)
```

## 4. Perbedaan dengan API mobile

| Aspek | Mobile (`20-...`) | Admin (dokumen ini) |
|-------|-------------------|---------------------|
| Auth | JWT rumah tangga/vendor | JWT admin/operasi |
| Tulis | pesanan, konfirmasi, serah | verifikasi, skors, alihkan |
| Idempotensi | Header Idempotency-Key | Body berisi alasan + audit |

## 5. Kaitan dokumen

- Kontrak mobile: `produk/20-kontrak-api-KMP-mobile.md`
- Web Bun: `platform/50-web-bun-svelte.md`
- SLA: `vendor/10-sla-vendor.md`
