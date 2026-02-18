# Dokumentasi Pengujian Review Dosen (KKN, PKL, TA)

## Deskripsi
Modul ini menangani fungsi Dosen Pembimbing (DPL/Dosbing) dalam melakukan validasi, memberikan feedback, dan menilai progres mahasiswa.

## Fitur Utama
1. **Validasi KKN**: DPL melihat dan menyetujui logbook kelompok KKN dampingannya.
2. **Review Laporan PKL**: Dosbing melakukan review bab per bab laporan PKL mahasiswa.
3. **Penilaian Sidang PKL**: Dosbing Penguji memberikan nilai dan status kelulusan (Lulus/Revisi/Gagal).
4. **Feedback TA**: Dosbing TA memberikan komentar dan mengubah status logbook bimbingan mahasiswa.

## Status Verifikasi (18 Feb 2026)
| Kasus Uji | Modul | Status | Catatan |
|-----------|-------|--------|---------|
| DPL Validasi Logbook | KKN | LULUS | Mahasiswa menerima notifikasi |
| Dosen Review Laporan | PKL | LULUS | Status berubah jadi 'disetujui' |
| Dosen Input Nilai Sidang| PKL | LULUS | Status registrasi PKL update otomatis |
| Dosen Feedback Logbook | TA | LULUS | Komentar tersimpan & status update |
| Filter Grup DPL | KKN | LULUS | DPL hanya bisa validasi kelompoknya |

## Cara Menjalankan Test
```bash
php artisan test --filter=KknLogbookNotificationTest
php artisan test --filter=PklLaporanTest
php artisan test --filter=PklModuleTest
php artisan test --filter=TaLogbookTest
```
