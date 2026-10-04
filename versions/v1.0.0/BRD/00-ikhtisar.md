> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# BRD 00 — Ikhtisar TitipO

## Konteks

Divisi TitipO adalah community commerce PT ChefGenie. Tugasnya menghubungkan rumah tangga dengan vendor terpercaya untuk titip harian dan menyerap pasokan Lumbung. TitipO tidak memproduksi bahan baku; TitipO menyalurkan stok Lumbung lewat vendor kurasi: tukang sayur keliling, warung, dan dapur rumahan. Pola tunggal: pesan hari ini maksimal 20.00, terima besok pagi 06.00–08.00. Basis data tunggal: DB titipo. Semua SKU merujuk katalog Lumbung; TitipO tidak membuat SKU sendiri.

## Kebutuhan Bisnis

- BR-001 TitipO wajib melayani titip harian dengan jendela pesan 05.00 sampai 20.00 dan jendela serah besok 06.00 sampai 08.00.
- BR-002 TitipO hanya menjual item katalog Lumbung tersinkron maksimal 24 jam; checkout menolak SKU kedaluwarsa dengan galat 409.
- BR-003 Satu pesanan hanya untuk satu vendor; maksimal 20 kg dan 50 baris per pesanan.
- BR-004 Pendapatan vendor adalah fee 12 persen dari total item ditambah Rp4.000 dari ongkir per pesanan, cair H+1 mulai 06.00.
- BR-005 Sukses diukur sebagai loyalitas: repeat-order minimal 40 persen per vendor per 4 minggu, target kota minimal 35 persen.
- BR-006 Pembayaran rumah tangga di muka via QRIS dengan kedaluwarsa 15 menit; dana ditahan hingga foto serah diunggah.
- BR-007 Autentikasi dikunci: JWT pengguna beraudien titipo dengan peran vendor, household, admin, ditambah service key titipo ke lumbung untuk panggilan server-ke-server, tanpa akun bersama.

## Metrik

- Pesanan per hari per vendor. Konfirmasi median maksimal 20 menit. Tepat serah minimal 95 persen. Batal vendor maksimal 2 persen. Rating minimal 4,5. Repeat-order 28 hari bergulir.

## Batasan

Batasan dokumen ini: hanya menyatakan kebutuhan bisnis dan angka ambang. Cara pemenuhan diatur di PRD, FRD, dan FSD. Di luar batas: penentuan harga pokok Lumbung, resep dapur, penjualan ecer pasar, dan audit independen yang menjadi milik TownHall lain.
