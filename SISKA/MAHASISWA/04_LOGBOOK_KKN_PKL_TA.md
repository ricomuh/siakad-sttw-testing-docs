# Dokumentasi Pengujian Logbook Mahasiswa (KKN, PKL, TA)

## Deskripsi
Modul ini memungkinkan mahasiswa untuk mencatat aktivitas harian atau progres pengerjaan tugas akhir dan laporan PKL/KKN.

## Alur Kerja
1. **KKN**: Mahasiswa yang sudah diterima di grup KKN dapat mengisi logbook harian.
2. **PKL**: Mahasiswa harus mengisi KRS PKL terlebih dahulu, kemudian mendaftar, mengisi logbook, mengunggah bab laporan (upload file + deskripsi), hingga mendaftar sidang.
3. **TA**: Mahasiswa mengajukan proposal, setelah disetujui admin akan dibuatkan registrasi TA, kemudian mahasiswa mengisi logbook bimbingan mandiri.

## Status Verifikasi (18 Feb 2026)
| Kasus Uji | Modul | Status | Catatan |
|-----------|-------|--------|---------|
| Submit Logbook KKN | KKN | LULUS | Notifikasi terkirim ke DPL |
| Registrasi PKL | PKL | LULUS | Validasi syarat KRS terpenuhi |
| Submit Logbook PKL | PKL | LULUS | Data tersimpan dengan dokumentasi |
| Upload Bab Laporan | PKL | LULUS | Alur 2-step (Upload -> Deskripsi) |
| Daftar Sidang PKL | PKL | LULUS | Judul final & file laporan akhir terverifikasi |
| Submit Proposal TA | TA | LULUS | Data proposal masuk ke daftar review admin |
| Submit Logbook TA | TA | LULUS | Status awal 'pending' menunggu feedback |

## Cara Menjalankan Test
```bash
php artisan test --filter=KknLogbookNotificationTest
php artisan test --filter=PklModuleTest
php artisan test --filter=PklLaporanTest
php artisan test --filter=TaLogbookTest
php artisan test --filter=TaProposalTest
```
