> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# FSD 10 — Alur Sistem

## Urutan Tulis Lokal Dahulu

1. Perangkat menulis ke basis lokal dahulu dalam kurang dari 200 ms dengan ULID 26 karakter, lalu menampilkan sukses tanpa menunggu server.
2. Antrean lokal FIFO diproses saat online satu per satu dengan jeda 1 detik dan header kunci idempoten sama. Retry gagal jaringan tiap 30 detik maksimal 24 jam; setelah itu tandai gagal dan tampilkan tombol kirim ulang manual.
3. Batas antrean: 50 aksi per 24 jam, 20 foto maksimal 800 KB per foto, 5 perubahan jadwal per minggu. Lewat itu tombol dikunci dengan pesan sambungkan internet.
4. Snapshot katalog: mobile menyimpan versi snapshot dan waktu diperbarui; bila umur di atas 24 jam tombol pesan dikunci hingga tarik ulang maksimal 5 MB. Sinkron server 04.00 tarik Lumbung, validasi SKU, tulis snapshot vNNN; bila job gagal 3 kali snapshot kemarin tetap berlaku dengan banner merah di admin.
5. Resolusi konflik: rebut stok dimenangkan yang tiba di server lebih dulu; jadwal tutup memakai last-write-wins berdasarkan waktu dibuat di HP dengan tanggal efektif tanggal yang dipilih; foto ganda dimenangkan foto terakhir dan foto lama diarsipkan.
6. Urutan layar rumah: daftar vendor, katalog vendor, keranjang terkunci 06.00 sampai 08.00, bayar QRIS timer 15:00, lacak kode. Urutan layar vendor: masuk daftar titip, kemas checklist titik Lumbung, serah kamera. Bilah selalu menampilkan luring dan jumlah antrean bila offline.

## Batasan

Batasan dokumen ini: hanya urutan sistem dan aturan sinkron. Formula bisnis ada di FSD model data dan kontrak. Di luar batas: desain visual dan merek. Target mutu: antrean tidak pernah kehilangan diam-diam dan tidak pernah mengirim ganda.
