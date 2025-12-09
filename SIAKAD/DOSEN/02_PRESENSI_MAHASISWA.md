# Testing: Presensi Mahasiswa (Dosen)

**Status**: ⚪ [TODO]  
**Role**: Dosen  
**Route**: `/siakad/dosen/presensi-mahasiswa`  
**Controller**: `Dosen\PresensiMahasiswaController`  
**Model**: `PresensiMahasiswa`, `SesiKelas`, `Mahasiswa`

---

## 📋 Checklist Testing

### 1. Operations

- [ ] **Index**: List sesi yang presensi dibuka
- [ ] **Store**: Input presensi per mahasiswa per sesi
- [ ] **Bulk Update**: Update multiple mahasiswa sekaligus
- [ ] **Export**: Download rekap presensi ke Excel

### 2. Authorization

- [ ] Hanya dosen pengampu yang bisa input presensi
- [ ] Tidak bisa input presensi mata kuliah dosen lain
- [ ] Presensi hanya bisa diinput saat sesi aktif (opened)

### 3. Status Presensi

- [ ] **Hadir**: Default status, counted as present
- [ ] **Izin**: Dengan alasan (keterangan wajib)
- [ ] **Sakit**: Dengan alasan (keterangan wajib)
- [ ] **Alfa**: Tanpa keterangan, counted as absent

### 4. Input Validation

- [ ] **Sesi Active**: Presensi hanya bisa diinput saat sesi dibuka
- [ ] **Duplicate Prevention**: Tidak bisa input presensi 2x untuk 1 mahasiswa
- [ ] **Required Keterangan**: Izin/Sakit wajib ada keterangan
- [ ] **Mahasiswa Valid**: Hanya mahasiswa KRS yang bisa diabsen

### 5. Bulk Input

- [ ] **Mark All Hadir**: Set semua mahasiswa hadir sekaligus
- [ ] **Mark Selected**: Checkbox multiple mahasiswa
- [ ] **Quick Status**: Dropdown status per mahasiswa
- [ ] **Save All**: Submit semua presensi dalam 1 request

### 6. Attendance Tracking

- [ ] Calculate total hadir per mahasiswa
- [ ] Calculate attendance percentage (hadir + izin) / total sesi
- [ ] Flag mahasiswa with < 75% attendance (risk dropout)
- [ ] Show attendance history per mahasiswa

### 7. Reports

- [ ] Rekap presensi per sesi
- [ ] Rekap presensi per mahasiswa
- [ ] Export to Excel with filters
- [ ] Chart: Attendance trend per pertemuan

### 8. Integration

- [ ] Presensi terkoneksi ke sesi kelas
- [ ] Auto-notify mahasiswa alfa
- [ ] Block ujian jika attendance < 75%
- [ ] Sync ke nilai partisipasi (jika ada)

### 9. Late Submission

- [ ] Dosen bisa input presensi setelah sesi ditutup (max 24 jam)
- [ ] Flagged sebagai "Late Entry"
- [ ] Approval by Kaprodi jika > 24 jam

---

## 🔗 Related

- [Jadwal Mengajar](01_JADWAL_MENGAJAR.md)
- [Mata Kuliah Detail](03_MATA_KULIAH_DETAIL.md)
- [Presensi Mahasiswa (Admin)](../ADMIN/13_PRESENSI_MAHASISWA.md)
