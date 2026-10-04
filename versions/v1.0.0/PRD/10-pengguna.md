> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# PRD 10 — Pengguna TitipO

## Daftar Peran

- Rumah tangga: memilih vendor radius 5 km, mengisi keranjang dari katalog tersinkron, membayar QRIS dalam 15 menit, melacak 8 langkah, memberi rating 1 sampai 5. Kebutuhan: daftar vendor jarak terdekat, katalog stempel Diperbarui 04.30, keranjang maksimal 20 kg, layar bayar timer 15:00, layar lacak timeline dan foto.
- Vendor: mengonfirmasi maksimal 30 menit, mengambil di titik Lumbung 04.00 sampai 06.00, menyerah 06.00 sampai 08.00 dengan 1 foto, melihat saldo fee. Kebutuhan: daftar titip hari ini dengan timer, checklist kemas, kamera 1280px kompres maksimal 800 KB, banner luring dan badge antrean.
- Admin dan operasi: mengurasi calon, memantau pesanan harian, mengelola snapshot cermin, mencairkan fee manual bila job gagal. Kebutuhan: halaman vendor, pesanan, katalog, fee, metrik; polling pesanan 30 detik; tombol sinkron ulang dan alihkan manual.

## Hak Akses

- Rumah tangga hanya melihat pesanan miliknya. Vendor hanya melihat titipan lapaknya hari itu. Admin melihat seluruh vendor dan pesanan. Operasi boleh baca semua tetapi tulis hanya verifikasi baca dan pantau, bukan skors.
- Autentikasi: JWT beraudien titipo masa berlaku 24 jam dengan klaim peran rumah, vendor, admin. Satu orang satu akun. Ganti peran butuh logout dan login ulang. Panggilan ke Lumbung hanya dari server memakai service key titipo ke lumbung; aplikasi tidak pernah menyimpan service key.

## Batasan

Batasan dokumen ini: hanya peran, kebutuhan pandang, dan hak akses. Aturan bisnis rinci ada di BRD, langkah sistem ada di FSD. Di luar batas: peran pemasok Lumbung dan peran audit mutu yang diatur TownHall masing-masing.
