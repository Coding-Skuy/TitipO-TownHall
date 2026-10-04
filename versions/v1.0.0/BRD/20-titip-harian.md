> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# BRD 20 — Titip Harian

## Konteks

Titip harian mencakup 8 langkah dari pilih vendor sampai fee cair, plus mesin status tunggal. Sumber isi lama: `pesanan/10-alur-titip-harian.md`.

## Kebutuhan Bisnis

- BR-201 Langkah titip dikunci 8 tahap: pilih vendor dan isi keranjang 05.00 sampai 20.00, kunci stok dan buat pesanan menunggu konfirmasi, bayar QRIS dalam 15 menit, konfirmasi vendor maksimal 30 menit, ambil dan kemas dari pasokan Lumbung 04.00 sampai 06.00 besok, serah dan foto 06.00 sampai 08.00, konfirmasi terima dan rating maksimal 30 jam atau otomatis selesai H+1 12.00, cairkan fee H+1 dan catat repeat.
- BR-202 Contoh nyata mengikat: bayam Rp9.000 per kg 2 kg ditambah telur Rp29.000 per kg 1 kg ditambah ongkir Rp5.000 sama dengan Rp52.000 total bayar.
- BR-203 Timeout konfirmasi 30 menit dieksekusi job tiap 1 menit; pengalihan otomatis maksimal 2 kali lalu refund penuh H+1.
- BR-204 Aksi vendor yang boleh antre luring: konfirmasi, tolak, tutup jadwal, foto serah. Yang wajib online: checkout dan pembayaran rumah tangga.
- BR-205 Konflik rebut stok dimenangkan konfirmasi yang tiba di server lebih dulu; yang kalah otomatis dialihkan memakai 1 dari 2 jatah pengalihan.
- BR-206 Pembatalan gratis sampai H-1 17.00; batal vendor kurang dari 2 jam sebelum serah memicu denda dan voucher.

## Metrik

- Pesanan menunggu konfirmasi per jam. Rasio dialihkan. Rasio batal vendor. Waktu serah terhadap janji. Contoh ujung-ke-ujung tercatat per kode T-YYYY-NNNNNN.

## Batasan

Batasan segmen ini: hanya pesan, konfirmasi, alokasi, antar, bukti, tagih, dan bayar. Di luar batas: pengolahan dapur, penjualan ecer, dan pengantar ke rumah di luar jendela 06.00 sampai 08.00. Short yang tidak terpenuhi dicatat alasannya sebelum dialihkan ke vendor cadangan.
