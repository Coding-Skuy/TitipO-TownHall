> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# PRD 20 — Alur Produk

## Alur Pesan sampai Fee Cair

1. Pesan masuk 05.00 sampai 20.00: rumah tangga memilih 1 vendor dan mengisi keranjang dari katalog tersinkron. Batas 20 kg dan 50 baris per pesanan.
2. Kunci stok dan bayar: sistem membuat pesanan menunggu pembayaran, rumah tangga membayar QRIS dalam 15 menit, lalu status menjadi menunggu konfirmasi.
3. Konfirmasi maksimal 30 menit oleh vendor di aplikasi KMP, boleh luring masuk antrean. Timeout dieksekusi job tiap 1 menit, alihkan maksimal 2 kali.
4. Ambil dan kemas 04.00 sampai 06.00 besok dari pasokan Lumbung; checklist semua item sebelum tombol Siap serah aktif.
5. Serah 06.00 sampai 08.00 dengan foto wajib; tanpa foto tombol mati dan fee ditahan.
6. Rating maksimal 30 jam; otomatis selesai H+1 12.00 bila diam; fee 12 persen cair H+1 00.00 via job dan bisa ditarik mulai 06.00.

## Contoh Nyata

Rumah tangga membuka daftar vendor 00:00, memilih Sari 00:12, menambah bayam 2 kg dan telur 1 kg 00:35, keranjang Rp52.000 00:50, pindai QRIS 01:05, lunas 01:20, layar lacak Menunggu konfirmasi Sari. Sari konfirmasi, ambil di titik serah Lumbung Blok A 04.30, serah 06.40 dengan foto, rumah tangga rating 5, fee Sari cair besok.

## Matriks Platform

- Mobile KMP pemilik: lihat vendor dan katalog, checkout dan bayar, konfirmasi dan foto, rating. Web admin pemilik: kurasi calon, skors dan alihkan, sinkron Lumbung 04.00, cair fee, laporan repeat penuh. Tidak ada layar admin di mobile dan tidak ada checkout di web. Hitungan uang yang sah adalah hitungan server; hitungan mobile hanya pratinjau.

## Batasan

Batasan alur ini: hanya pesan, konfirmasi, alokasi, antar, bukti, tagih, dan bayar. Di luar batas: pengolahan dapur, penjualan ecer, dan pengantar di luar jendela 06.00 sampai 08.00. Katalog kedaluwarsa di atas 24 jam mengunci tombol pesan hingga tarik ulang.
