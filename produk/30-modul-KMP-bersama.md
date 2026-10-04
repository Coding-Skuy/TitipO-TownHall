# 30 — Modul KMP Bersama TitipO

> **Tujuan:** Pembagian kode Kotlin Multiplatform agar 1 codebase melayani 2 role (vendor + rumah tangga) di Android dan iOS tanpa duplikasi logika.
> **Pemilik:** Tim Mobile KMP (pemilik modul + versi).
> **Status:** Varian 1 — Kotlin 2.1.x, Compose Multiplatform 1.7.x, Navigation3 1.0.x, SQLDelight 2.x, Ktor 3.x. Tanpa modul desktop.

## 1. Struktur modul (keputusan konkret)

```
titipo-kmp/
├── shared/               # logika bersama (murni Kotlin, tanpa UI)
│   ├── model/            # DTO pesanan, vendor, katalog (mirror kontrak API 20-)
│   ├── validasi/         # hitung subtotal, batas 20 kg / 50 baris
│   ├── antrean/          # queue offline (tabel antrean + retry)
│   └── sinkron/          # tarik katalog Lumbung-cermin, stempel 24 jam
├── ui-bersama/           # komponen Compose bersama (kartu, stepper kg, banner luring)
├── fitur-rumah/          # 5 layar rumah tangga (lihat produk/10-)
├── fitur-vendor/         # 3 layar vendor + kamera + push
└── platform/             # expect/actual: kamera, notifikasi, lokasi, penyimpanan
```

Keputusan: `shared` tidak boleh mengimpor Compose atau Android/iOS API — hanya Kotlin murni + kotlinx-datetime + kotlinx-serialization. Pelanggaran ditolak di code review.

## 2. Pembagian 2 role dalam 1 aplikasi

1. Satu binary, dua graf Navigation3: `GrafRumah` dan `GrafVendor`, dipilih saat login berdasarkan klaim JWT `peran`.
2. State peran disimpan di DataStore (`peran: rumah|vendor`), bukan dua aplikasi toko.
3. Contoh routing:
```kotlin
NavDisplay(
  backStack = when (peran) {
    Peran.Rumah -> backStackRumah   // rumah/daftar-vendor ...
    Peran.Vendor -> backStackVendor // vendor/masuk ...
  }
)
```

Contoh: akun Sari (`peran=vendor`) login di Android → langsung `vendor/masuk`; akun Ani (`peran=rumah`) di iPhone → `rumah/daftar-vendor`. Ganti peran butuh logout + login ulang (tidak ada toggle di Varian 1).

## 3. Dependensi kunci (versi dikunci Varian 1)

| Pustaka | Versi | Fungsi |
|---------|-------|--------|
| Kotlin | 2.1.20 | Bahasa + expect/actual |
| Compose Multiplatform | 1.7.3 | UI Android+iOS |
| Navigation3 | 1.0.0 | Navigasi 2 graf |
| SQLDelight | 2.0.3 | antrean + cache katalog lokal |
| Ktor client | 3.1.2 | HTTP + retry idempoten |
| Coil 3 | 3.1.0 | Muat foto serah |

## 4. Contoh kode antrean (konkret)

```kotlin
// shared/antrean/Antrean.kt
data class AksiAntre(val kunci: String, val jenis: String, val muatanJson: String)

suspend fun kirimAntrean(aksi: AksiAntre) {
  db.antreanQueries.simpan(aksi.kunci, aksi.jenis, aksi.muatanJson, sinkron = 0)
  while (true) {
    try {
      http.post("/vendor/pesanan/${aksi.kode}/konfirmasi") {
        header("Idempotency-Key", aksi.kunci); setBody(aksi.muatanJson)
      }
      db.antreanQueries.tandaiSinkron(aksi.kunci); break
    } catch (e: Exception) { delay(30_000) } // retry 30 detik, maks 24 jam
  }
}
```

## 5. Kaitan dokumen

- UX: `produk/10-ux-pesan-titip.md`
- Kontrak API: `produk/20-kontrak-api-KMP-mobile.md`
- Mobile: `platform/40-mobile-KMP.md`
- Offline: `platform/60-offline-sinkron.md`
