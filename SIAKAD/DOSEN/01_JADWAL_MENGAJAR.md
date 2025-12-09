# Testing: Jadwal Mengajar (Dosen)

**Status**: ⚪ [TODO]  
**Role**: Dosen  
**Route**: `/siakad/dosen/jadwal-mengajar`  
**Controller**: `Dosen\JadwalMengajarController`  
**Model**: `JadwalPerkuliahan`, `FormasiDosen`, `SesiKelas`

---

## 📋 Checklist Testing

### 1. View Operations

- [ ] **Index**: List jadwal mengajar dosen yang login
- [ ] **Show**: Detail jadwal dengan list mahasiswa
- [ ] **Filter**: Filter by semester, tahun akademik
- [ ] **Filter Hari**: Filter jadwal by hari (Senin-Jumat)

### 2. Authorization

- [ ] Dosen hanya bisa lihat jadwal sendiri
- [ ] Tidak bisa lihat jadwal dosen lain
- [ ] Redirect to login jika belum auth
- [ ] Forbidden jika bukan role dosen

### 3. Jadwal Display

- [ ] Show: Hari, waktu mulai-selesai
- [ ] Show: Nama mata kuliah, SKS
- [ ] Show: Ruangan
- [ ] Show: Jumlah mahasiswa terdaftar
- [ ] Show: Status sesi (Belum Dimulai, Berlangsung, Selesai)

### 4. Sesi Kelas Management

- [ ] Generate sesi otomatis per pertemuan (16 minggu)
- [ ] Sesi numbering: Pertemuan 1-16
- [ ] Track: Tanggal pelaksanaan
- [ ] Track: Topik/materi pertemuan
- [ ] Track: Status (Terjadwal, Berlangsung, Selesai, Dibatalkan)

### 5. Buka/Tutup Presensi

- [ ] **Buka Presensi**: Aktifkan input presensi untuk sesi
- [ ] **Validation**: Hanya bisa buka di hari yang sama
- [ ] **Validation**: Hanya 1 sesi aktif per jadwal
- [ ] **Tutup Presensi**: Nonaktifkan input presensi
- [ ] **Auto Close**: Tutup otomatis setelah waktu selesai + 30 menit
- [ ] **Log Activity**: Catat waktu buka/tutup presensi

### 6. Mahasiswa List

- [ ] Show list mahasiswa yang KRS mata kuliah ini
- [ ] Show: NIM, nama, status KRS (Disetujui)
- [ ] Show: Attendance percentage per mahasiswa
- [ ] Filter: By status presensi (Hadir, Izin, Sakit, Alfa)

### 7. Integration

- [ ] Link ke input presensi mahasiswa
- [ ] Link ke detail mata kuliah
- [ ] Link ke input nilai
- [ ] Notifikasi mahasiswa saat presensi dibuka

### 8. Edge Cases

- [ ] Jadwal kosong (dosen belum ada formasi)
- [ ] Mahasiswa belum KRS
- [ ] Sesi dibatalkan (hari libur)
- [ ] Perubahan jadwal (reschedule)

---

## 🔗 Related

- [Presensi Mahasiswa](02_PRESENSI_MAHASISWA.md)
- [Mata Kuliah Detail](03_MATA_KULIAH_DETAIL.md)
- [Formasi Dosen (Admin)](../ADMIN/08_FORMASI_DOSEN.md)
