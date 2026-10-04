# 10 — Metrik Repeat Order TitipO

> **Tujuan:** Mengukur apakah rumah tangga kembali menitip — satu-satunya metrik bintang utara Varian 1.
> **Pemilik:** Tim Produk (pemilik definisi), Tim Web Bun (pemilik query dashboard `/admin/metrik`).
> **Status:** Varian 1 — ambang 40%, dihitung mingguan, tanpa alat analitik pihak ketiga.

## 1. Definisi (keputusan konkret)

1. **Repeat-order mingguan** = rumah tangga yang memesan ≥ 2 pesanan `selesai` dalam 28 hari kalender bergulir / total rumah tangga aktif (≥ 1 selesai) dalam 28 hari yang sama.
2. **Aktif** = minimal 1 pesanan selesai dalam 28 hari. Dormant = 0 dalam 28 hari.
3. **Cohort** = minggu kalender checkout pertama (Senin–Minggu, WIB).
4. Target Varian 1: **repeat ≥ 40%** per vendor per 4 minggu untuk naik Prioritas; target kota ≥ 35%.

Contoh: minggu 6–12 Jan ada 200 rumah tangga aktif; 84 di antaranya memesan ≥ 2x dalam 28 hari terakhir → repeat = 84/200 = 42% (lolos ambang).

## 2. Query baku (PostgreSQL, dipakai dashboard)

```sql
WITH aktif AS (
  SELECT id_rumah_tangga, COUNT(*) AS n
  FROM pesanan WHERE status='selesai'
    AND diserahkan >= now() - INTERVAL '28 days'
  GROUP BY 1 HAVING COUNT(*) >= 1
),
ulang AS (
  SELECT id_rumah_tangga FROM aktif WHERE n >= 2
)
SELECT (SELECT COUNT(*) FROM ulang)::float
     / NULLIF((SELECT COUNT(*) FROM aktif),0) AS repeat_28h;
```

## 3. Metrik pendamping (4 metrik, ambang pasti)

| Metrik | Definisi | Target Varian 1 |
|--------|----------|-----------------|
| Rating 30 hari | Rata-rata bintang pesanan selesai | ≥ 4,5 |
| Tepat serah | % serah dalam ±15 menit janji | ≥ 95% |
| Batal vendor | % dibatalkan_vendor / total | ≤ 2% |
| Waktu konfirmasi | Median created→confirmed | ≤ 20 menit |

Contoh dashboard 7 Jan: repeat 42% (hijau), tepat serah 96% (hijau), batal 1,2% (hijau), median konfirmasi 13 menit (hijau).

## 4. Tindakan atas angka

1. Repeat vendor < 30% selama 2 minggu → pembinaan + audit katalog (apakah sering habis?).
2. Repeat kota < 30% selama 4 minggu → review harga 3 SKU teratas + uji ongkir Rp3.000 selama 2 minggu (satu-satunya eksperimen harga yang diizinkan Varian 1).
3. Repeat ≥ 40% 4 minggu + rating ≥ 4,7 → otomatis usul Prioritas di `/admin/vendor` (admin tinggal setuju).

## 5. Kaitan dokumen

- SLA: `vendor/10-sla-vendor.md` (rating, tepat serah)
- Fee: `keuangan/10-bagi-fee.md` (bonus Prioritas)
- Alur: `pesanan/10-alur-titip-harian.md`
