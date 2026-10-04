> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# FSD 30 — Kontrak API, Event, dan Galat

## Basis dan Autentikasi

- Basis: `https://api.titipo.id/v1` dengan basis admin `https://api.titipo.id/v1/admin`. Format JSON UTF-8; uang rupiah integer; berat gram integer; waktu ISO-8601 UTC. Sehat: `GET /healthz` kembali status ok.
- Autentikasi dikunci: JWT pengguna beraudien titipo masa berlaku 24 jam dengan klaim peran vendor, household, admin. Verifikasi via JWKS Lumbung cache 10 menit toleransi 60 detik. Token audien selain titipo ditolak 403.
- Service key titipo ke lumbung untuk panggilan server-ke-server: header `X-Titipo-Service-Key`, simpan di env backend `TITIPO_SERVICE_KEY` dan secret Lumbung, rotasi 90 hari dengan 2 kunci aktif 7 hari saat rotasi. Aplikasi mobile tidak pernah menyimpan service key.
- Kunci dirotasi dan tidak pernah dikirim ke browser. DB yang dipakai: DB titipo; tidak ada akses langsung ke DB Lumbung dari klien.

## Kontrak per Konsumen

- Mobile 7 endpoint: `GET /vendor-terdekat` lat lng radius 5, `GET /katalog/{idVendor}` 409 bila snapshot di atas 24 jam, `POST /pesanan` checkout wajib online dengan header `Idempotency-Key` ULID kembali 201 kode T-YYYY-NNNNNN total bayar dan kedaluwarsa bayar, `POST /pesanan/{kode}/bayar` id gateway kembali menunggu konfirmasi, `POST /vendor/pesanan/{kode}/konfirmasi` terima atau tolak beralasan boleh antre luring, `POST /vendor/pesanan/{kode}/serah` multipart foto JPEG maksimal 2 MB kembali url foto, `POST /pesanan/{kode}/rating` bintang 1 sampai 5 catatan maksimal 200 karakter.
- Web admin 6 endpoint: `GET /admin/vendor` status calon, `POST /admin/vendor/{id}/verifikasi` setuju kuota 30 atau tolak beralasan, `POST /admin/vendor/{id}/skors` hari 3 atau 7, `GET /admin/pesanan` status dan hari dengan agregat total terlambat batal, `POST /admin/pesanan/{kode}/alihkan` id vendor cadangan maksimal 2 kali, `GET /admin/katalog-cermin` diperbarui sumber jumlah SKU status.
- Job Bun 4 jadwal: sinkron-lumbung 04.00 retry 3 kali interval 30 detik, timeout-konfirmasi tiap 1 menit, cair-fee 00.00 ulang 01.00 bila gagal, arsip tanggal 1 jam 02.00.
- Modul KMP bersama: shared murni Kotlin tanpa Compose, ui-bersama komponen Compose, fitur-rumah 5 layar, fitur-vendor 3 layar kamera push, platform expect actual kamera notifikasi lokasi. Satu biner 2 graf Navigation3 dipilih dari klaim peran.
- Platform target: mobile Android 8 ke atas dan iOS 16 ke atas; web admin Bun 1.4 Svelte 5 SvelteKit 2 TypeScript 5.9; buka dingin maksimal 2,5 detik; unduh maksimal 35 MB.

## Event dan Galat

- Event: `pesanan.dibuat`, `pesanan.dibayar`, `pesanan.dikonfirmasi`, `pesanan.diserahkan`, `pesanan.dialihkan`, `fee.dicairkan`, `konflik.dicatat`. Setiap event membawa ULID, waktu, dan versi.
- Idempotensi: semua POST membawa kunci ULID klien; kirim ulang sama kembali 200 tanpa rekaman ganda.
- Galat baku: 401 token kedaluwarsa login ulang, 403 peran ditolak dan audien salah, 409 katalog kedaluwarsa dan stok habis, 422 validasi contoh lebih dari 20 kg atau transisi ilegal, 429 admin 600 per menit per IP.
- Paginasi: batas 20 lanjut kode terakhir, respons data dan lanjut, batas maksimal 50.

## Batasan

Batasan dokumen ini: hanya kontrak, event, dan galat. Implementasi server ada di repo titipo-backend-service. Konsumen dilarang menghitung fee dan total sah di klien; semua wajib memakai angka server. Perhitungan mobile hanya pratinjau.
