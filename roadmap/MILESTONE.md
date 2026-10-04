> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# MILESTONE TitipO — Status Living

Legenda status: todo berarti belum mulai, doing berarti sedang berjalan, done berarti selesai terverifikasi.

## Milestone v1.0.0

- M-001 10 vendor perdana lolos kurasi 5 syarat dan berstatus aktif — status: done. Bukti: arsip KTP, skor kebersihan minimal 80, e-sign SLA di aplikasi.
- M-002 Sinkron katalog Lumbung 04.00 berjalan 7 hari berturut tanpa gagal — status: doing. Bukti: log job Bun sinkron-lumbung dan stempel Diperbarui 04.30 di mobile.
- M-003 Alur titip 8 langkah berjalan ujung-ke-ujung pesan maksimal 20.00 serah 06.00–08.00 — status: doing. Target: konfirmasi maksimal 30 menit, foto serah 100 persen.
- M-004 Job cair-fee 00.00 dan ledger fee 12 persen akurat — status: doing. Bukti: ledger 3 kasus hitungan di BRD keuangan cocok dengan job.
- M-005 Repeat-order kota mencapai minimal 35 persen dan repeat vendor Prioritas minimal 40 persen — status: todo. Syarat mulai: 200 rumah tangga aktif dalam 28 hari bergulir.
- M-006 Tepat serah minimal 95 persen dan batal vendor maksimal 2 persen selama 4 minggu — status: todo. Bukti: dashboard `/admin/metrik`.
- M-007 Autentikasi JWT beraudien titipo dan service key titipo ke lumbung terpasang di backend — status: doing. Bukti: token audien selain titipo ditolak 403.
- M-008 40 vendor aktif dan 2 titik serah beroperasi — status: todo. Syarat lulus rintisan: M-005 dan M-006 berstatus done.

## Aturan Pembaruan

- Status diubah hanya oleh Kepala Operasi TitipO dengan bukti tanggal. Milestone yang sudah done tidak dihapus, hanya ditambah catatan verifikasi.

## Batasan

Batasan dokumen ini: hanya status milestone dan bukti ringkas. Rincian angka ada di `versions/v1.0.0/BRD/` dan `versions/v1.0.0/PRD/30-kriteria.md`. Dokumen ini tidak mengubah janji beku v1.0.0.
