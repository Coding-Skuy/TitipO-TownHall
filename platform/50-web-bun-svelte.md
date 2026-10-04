# 50 — Web Bun + Svelte TitipO (Admin)

> **Tujuan:** Spesifikasi web admin untuk kurasi, pantau, dan job — satu-satunya tempat operasi manusia.
> **Pemilik:** Tim Web Bun (pemilik deploy + migrasi).
> **Status:** Varian 1 — Bun 1.4.x, Svelte 5 (runes), SvelteKit 2, TypeScript 5.9.x, PostgreSQL 16, deploy single VPS.

## 1. Keputusan stack (dikunci)

1. Runtime Bun 1.4.x (bukan Node); package manager `bun`; test `bun test`.
2. Svelte 5 runes (`$state`, `$derived`) — tanpa Svelte 4 options API.
3. SvelteKit 2 dengan adapter-node, SSR untuk tabel admin, CSR untuk grafik.
4. TypeScript 5.9.x strict (`strict: true`, `noUncheckedIndexedAccess: true`).
5. DB: `bun:sql` → PostgreSQL 16; migrasi di `migrasi/*.sql` berurutan (`001_vendor.sql`, `002_pesanan.sql`).

Contoh `package.json` Varian 1:
```json
{"scripts":{"dev":"vite dev","bangun":"vite build","migrasi":"bun ./skrip/migrasi.ts","job":"bun ./skrip/job.ts"}}
```

## 2. Halaman admin (5 halaman pasti)

| Route | Fungsi | Komponen Svelte 5 |
|-------|--------|-------------------|
| `/admin/vendor` | Antrean calon + tombol Setuju/Tolak | `KartuVendor.svelte`, runes `$state` filter status |
| `/admin/pesanan` | Tabel harian + agregat + tombol Alihkan | `TabelPesanan.svelte`, polling 30 detik |
| `/admin/katalog` | Snapshot cermin + tombol Sinkron ulang | `StatusSinkron.svelte`, stempel `diperbarui` |
| `/admin/fee` | Ledger fee + tombol Cair manual | `TabelFee.svelte` (baca `keuangan/10-bagi-fee.md`) |
| `/admin/metrik` | Repeat-order + rating | `GrafikRepeat.svelte` (baca `metrik/10-repeat-order.md`) |

Contoh runes:
```svelte
<script lang="ts">
  let vendor = $state([]); let filter = $state('calon');
  let antrean = $derived(vendor.filter(v => v.status === filter));
</script>
```

## 3. Job Bun (4 job pasti, tanpa tambahan Varian 1)

| Job | Jadwal | Fungsi | Retry |
|-----|--------|--------|-------|
| sinkron-lumbung | 04.00 WIB | Tarik katalog Lumbung → snapshot cermin | 3x interval 30 detik |
| timeout-konfirmasi | tiap 1 menit | Timeout 30 menit → alihkan (maks 2x) → refund | — |
| cair-fee | 00.00 WIB | Hitung fee H-1 → ledger | 1x ulang 01.00 jika gagal |
| arsip | tgl 1 jam 02.00 | Pindah pesanan > 2 tahun ke arsip | — |

Contoh log: `[2026-01-07 04:00:12] sinkron-lumbung v112: 48 SKU ok (1,8 detik)`.

## 4. Operasional

1. Deploy: `bun run bangun && systemctl restart titipo-web` (downtime target < 30 detik).
2. Backup DB harian 03.00 ke `/cadangan/titipo-%Y%m%d.sql.gz`, retensi 30 hari.
3. Secret via `.env` (`DATABASE_URL`, `JWT_RAHASIA`, `KUNCI_GATEWAY`); tidak ada secret di repo.

## 5. Kaitan dokumen

- Kontrak admin: `produk/21-kontrak-api-web-bun.md`
- Matriks: `platform/10-matriks-KMP-web.md`
- Fee: `keuangan/10-bagi-fee.md`
