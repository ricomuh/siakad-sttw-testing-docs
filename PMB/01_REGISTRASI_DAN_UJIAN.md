# Dokumentasi Pengujian PMB (Penerimaan Mahasiswa Baru)

## Deskripsi
Modul ini menangani alur pendaftaran calon mahasiswa baru mulai dari registrasi akun, unggah berkas validasi, pembayaran, hingga pelaksanaan ujian TPA online.

## Fitur Utama
1. **Registrasi Calon Mahasiswa**: Pembuatan akun otomatis sekaligus profil mahasiswa dengan status 'Calon'.
2. **Validasi Berkas**: Unggah Ijazah, KTP, KK, dan Pas Foto.
3. **Ujian TPA (Tes Potensi Akademik)**:
    - Engine ujian yang mengambil 30 soal acak dari Bank Soal.
    - Timer otomatis 60 menit.
    - Penilaian otomatis (KKM 60).
    - Integrasi status kelulusan ke tahap berikutnya.
4. **Bank Soal Admin**: Pengelolaan soal TPA (Verbal, Numerik, Logika).

## Status Verifikasi (18 Feb 2026)
| Kasus Uji | Status | Catatan |
|-----------|--------|---------|
| Registrasi Calon (Guest) | LULUS | Berhasil redirect ke dashboard pmb |
| Unggah Berkas Validasi | LULUS | Berkas tersimpan di storage & DB |
| Admin Approve Berkas | LULUS | Tahap tes mahasiswa naik ke level 2 |
| Start Ujian TPA | LULUS | Sesi dibuat & 30 soal diacak |
| Submit Jawaban (Lulus) | LULUS | Skor >= 60, status pendaftaran 'passed_tpa' |
| Submit Jawaban (Gagal) | LULUS | Skor < 60, status pendaftaran 'failed_tpa' |
| CRUD Bank Soal | LULUS | Admin dapat menambah/edit/hapus soal |

## Cara Menjalankan Test
```bash
php artisan test --filter=PmbRegistrationTest
php artisan test --filter=UjianTest
php artisan test --filter=BankSoalTest
```
