> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# PRD 30 — Kriteria Keberhasilan

## Cerita Pengguna dan Acceptance

- US-001 Sebagai rumah tangga saya menitip dalam 90 detik sehingga siap bayar sebelum QRIS kedaluwarsa. Acceptance: 5 layar rumah berfungsi; timer 15:00 mundur; total item tambah ongkir Rp5.000 sama dengan total bayar; contoh Rp52.000 tampil benar.
- US-002 Sebagai vendor saya mengonfirmasi dalam 2 ketukan sehingga tidak lewat 30 menit. Acceptance: geser kanan Terima dan geser kiri Tolak beralasan; bisa luring masuk antrean; push berisi kode, total, dan sisa menit.
- US-003 Sebagai vendor saya mengunggah foto serah sehingga fee tidak ditahan. Acceptance: kamera 1280px kompres maksimal 800 KB; tanpa foto tombol mati; 100 persen pesanan selesai membawa foto.
- US-004 Sebagai admin saya mengurasi calon sehingga hanya vendor lolos yang jualan. Acceptance: antrean status calon tampil dengan skor; setuju mengubah ke aktif kuota 30; tolak mencatat alasan; tercatat di audit.
- US-005 Sebagai kepala operasi saya memantau tepat serah sehingga repeat terjaga. Acceptance: tepat dalam toleransi 15 menit minimal 95 persen; rating 30 hari minimal 4,5; batal vendor maksimal 2 persen; median konfirmasi maksimal 20 menit.
- US-006 Sebagai rumah tangga saya kembali menitip sehingga repeat tercapai. Acceptance: repeat 28 hari bergulir minimal 40 persen per vendor Prioritas dan minimal 35 persen kota; query baku dashboard memakai tabel pesanan status selesai.

## Non-Goals v1.0.0

- Tanpa pendaftaran vendor otomatis langsung jualan; tanpa pesan di atas 20.00; tanpa serah di luar 06.00 sampai 08.00; tanpa item manual di luar katalog Lumbung; tanpa mode grosir B2B.

## Batasan

Batasan dokumen ini: hanya kriteria produk yang dapat diuji. Rincian teknis API dan skema ada di FSD. Klaim sukses tanpa skor mingguan repeat, tepat serah, batal, dan konfirmasi dinyatakan tidak berlaku.
