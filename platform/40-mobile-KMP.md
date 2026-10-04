# 40 — Mobile KMP TitipO (Android + iOS)

> **Tujuan:** Spesifikasi teknis aplikasi mobile satu codebase untuk 2 role, offline-first untuk vendor.
> **Pemilik:** Tim Mobile KMP (pemilik rilis Play Store + App Store).
> **Status:** Varian 1 — minSdk Android 26 (Android 8), iOS 16+, tanpa tablet khusus, tanpa desktop.

## 1. Keputusan platform (konkret)

1. Bahasa UI: Indonesia saja di Varian 1 (tidak ada i18n).
2. Navigasi: Navigation3 dengan 2 graf (`GrafRumah`, `GrafVendor`), deep-link `titipo://lacak/{kode}`.
3. Jaringan: Ktor client, timeout 15 detik, retry 2x untuk GET; POST memakai `Idempotency-Key` (lihat kontrak API).
4. Lokal: SQLDelight 4 tabel (`pesanan`, `item_pesanan`, `antrean`, `foto_serah`), batas cache katalog 50 MB, foto antre maks 20 file (lebih dari itu wajib online dulu).
5. Push: FCM (Android) + APNs (iOS) via satu topik `titipan-{idVendor}`; notifikasi konfirmasi berbunyi + bergetar.

## 2. Offline-first vendor (perilaku pasti)

1. Saat luring, tombol Terima/Tolak tetap aktif; aksi masuk `antrean` dengan status chip "Antre 1".
2. Banner luring kuning selalu tampil + jumlah antrean ("Antrean 2 — terkirim otomatis").
3. Foto serah luring disimpan di `foto_serah` (JPEG 1280px, ≤ 800 KB) lalu diunggah FIFO saat online.
4. Batas antrean: 50 aksi / 24 jam. Lewat itu tombol dikunci dengan pesan "Terlalu banyak antrean — sambungkan internet."

Contoh: Sari di pasar tanpa sinyal menekan Terima 3 pesanan + 1 foto → antrean 4 → tiba di area sinyal → terkirim berurutan 4/4 dalam 40 detik → banner hijau "Semua terkirim."

## 3. Izin perangkat

| Izin | Android | iOS | Dipakai untuk |
|------|---------|-----|---------------|
| Kamera | CAMERA | NSCameraUsageDescription | Foto serah wajib |
| Lokasi | ACCESS_FINE_LOCATION | NSLocationWhenInUse | Daftar vendor terdekat |
| Notifikasi | POST_NOTIFICATIONS | APNs | Timer 30 menit |
| Penyimpanan | — (app-sandbox) | — | Foto antre (tanpa akses galeri di Varian 1) |

## 4. Kinerja dan rilis

1. Target: buka dingin ≤ 2,5 detik (Android menengah), checkout ≤ 90 detik, ukuran unduh ≤ 35 MB.
2. Rilis: versionCode Android + CFBundleVersion iOS naik tiap rilis; changelog Indonesia; staged rollout 20% → 100% dalam 3 hari.
3. Crash-free target ≥ 99,5% (diukur Firebase Crashlytics, kunci API per flavor).

## 5. Kaitan dokumen

- Modul bersama: `produk/30-modul-KMP-bersama.md`
- UX: `produk/10-ux-pesan-titip.md`
- Offline detail: `platform/60-offline-sinkron.md`
