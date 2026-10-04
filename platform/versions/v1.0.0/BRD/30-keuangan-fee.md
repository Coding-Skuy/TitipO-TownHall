> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# BRD 30 — Keuangan dan Fee

## Konteks

Keuangan TitipO hanya dari sisa setelah fee vendor dan pokok Lumbung. Tidak ada margin dari selisih harga pokok. Sumber isi lama: `keuangan/10-bagi-fee.md`.

## Kebutuhan Bisnis

- BR-301 Formula dikunci: total item sama dengan jumlah subtotal tanpa ongkir; fee vendor 12 persen dari total item; bonus Prioritas tambah 0,5 persen; denda Rp15.000 per batal sepihak kurang dari 2 jam; bersih vendor sama dengan fee tambah bonus kurang denda.
- BR-302 Ongkir Rp5.000 flat: Rp4.000 untuk vendor pengantar, Rp1.000 kas TitipO infrastruktur. Tidak masuk total item.
- BR-303 Kasus A mengikat: total item Rp47.000 fee Rp5.640 ditambah ongkir vendor Rp4.000 sama dengan Rp9.640 diterima vendor Aktif.
- BR-304 Kasus B mengikat: total item Rp47.000 fee Rp5.640 ditambah bonus Rp235 ditambah ongkir Rp4.000 sama dengan Rp9.875 diterima vendor Prioritas.
- BR-305 Kasus C mengikat: fee Rp5.640 ditambah ongkir Rp4.000 kurang denda Rp15.000 sama dengan minus Rp5.360 dipotong dari saldo minggu berjalan; saldo tidak boleh negatif dan sisa dibawa ke minggu depan.
- BR-306 Pencairan: job cair-fee 00.00 menghitung pesanan selesai H-1; saldo bisa ditarik mulai 06.00; minimal tarik Rp50.000; biaya transfer Rp2.500 ditanggung vendor; deposit Rp100.000 diakumulasi dari 10 persen fee pertama hingga genap.
- BR-307 Rekonsiliasi Lumbung tiap Senin 09.00: TitipO membayar pokok minggu lalu via transfer tunggal ditambah berita acara CSV; selisih susut di atas 2 persen dibagi 50:50 maksimal Rp200.000 per minggu, selebihnya ditanggung TitipO.

## Metrik

- Fee tertagih per minggu. Saldo tertahan. Denda terpotong. Selisih rekonsiliasi Senin. Ledger tersimpan untuk audit 2 tahun.

## Batasan

Batasan segmen ini: hanya fee, ongkir, ledger, penarikan, dan rekonsiliasi Lumbung. Di luar batas: pembukuan PT induk, pajak, dan pinjaman. Harga pokok Lumbung mengikat sebagai acuan; TitipO tidak mengubah harga pokok.
