> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# TitipO-TownHall — Divisi Community Commerce PT ChefGenie

## Peran TitipO

TitipO adalah divisi community commerce PT ChefGenie: menghubungkan rumah tangga dengan vendor terpercaya untuk titip harian, sekaligus menyerap pasokan Lumbung. Aturan inti v1.0.0: pesan maksimal pukul 20.00, serah besok 06.00–08.00, bayar QRIS di muka, fee vendor 12 persen cair H+1. Sukses = loyalitas: repeat-order minimal 40 persen. Bukan grosir B2B, bukan instan same-day, bukan marketplace bebas: semua SKU berasal dari sinkron Lumbung, tanpa item manual. Basis data: DB titipo. Autentikasi: JWT pengguna beraudien titipo (verifikasi via JWKS Lumbung) ditambah service key titipo ke lumbung untuk panggilan server-ke-server.

## Peta Versi Aktif

- Versi aktif: v1.0.0 (disetujui). Isi beku ada di `versions/v1.0.0/`.
- `versions/v1.0.0/CHANGELOG.md` — ringkasan versi awal.
- `versions/v1.0.0/BRD/` — kebutuhan bisnis BR-001 dan seterusnya.
- `versions/v1.0.0/PRD/` — pengguna dan kriteria US-001 dan seterusnya.
- `versions/v1.0.0/FRD/` — kebutuhan fungsional FR-001 dan seterusnya.
- `versions/v1.0.0/FSD/` — rancangan alur, model data pesanan, dan kontrak API mobile serta web admin.
- `versions/v1.0.0/SNAPSHOT-ROADMAP.md` — salinan beku janji v1.0.0.
- Peta hidup lintas versi ada di `roadmap/`: `TIMELINE.md`, `MILESTONE.md`, `ROADMAP.md`.

## Cara Baca History

1. Mulai dari `versions/v1.0.0/CHANGELOG.md` untuk ringkasan versi.
2. Lanjut ke `versions/v1.0.0/BRD/00-ikhtisar.md` untuk konteks bisnis, lalu `PRD/10-pengguna.md` untuk peran.
3. Untuk janji waktu itu, baca `versions/v1.0.0/SNAPSHOT-ROADMAP.md` yang sudah dibekukan dan tidak diubah lagi.
4. Untuk kondisi terkini lintas versi, baca `roadmap/TIMELINE.md` dan `roadmap/MILESTONE.md`.
5. Riwayat perubahan antar versi dilacak lewat `git log` dan `CHANGELOG.md` tiap versi. File lama sengaja dihapus setelah dipindah dengan `git mv` agar tidak ada dua sumber kebenaran.

## TownHall Lain dan Pedoman Induk

Pedoman induk: https://github.com/Coding-Skuy/ChefGenie-TownHall.

Pola yang ditiru: pola template emas https://github.com/Coding-Skuy/Lumbung-TownHall — penamaan `versions/vX.Y.Z/BRD|PRD|FRD|FSD/`, file `NN-nama-kebab.md`, header versi satu baris, dan bagian Batasan di tiap file.

TownHall lain:

- https://github.com/Coding-Skuy/Pawonee-TownHall — dapur dan pengolahan.
- https://github.com/Coding-Skuy/Pasaree-TownHall — pasar dan penjualan.
- https://github.com/Coding-Skuy/Pedaree-TownHall — pengantar dan last-mile.
- https://github.com/Coding-Skuy/Titeny-TownHall — ketelitian dan audit mutu.
- https://github.com/Coding-Skuy/Lumbung-TownHall — hulu pasokan, sumber katalog TitipO.

## Batasan

Batasan ruang lingkup repo ini: hanya kurasi vendor, titip harian pesan maksimal 20.00 serah 06.00–08.00, fee 12 persen, repeat-order minimal 40 persen, kontrak API TitipO, dan sinkron katalog dari Lumbung. Di luar batas: produksi bahan baku dan papan harga milik Lumbung, resep dapur milik Pawonee, harga ecer pasar milik Pasaree, routing last-mile milik Pedaree, dan audit independen milik Titeny. Semua angka di dokumen versi adalah keputusan berlaku.
