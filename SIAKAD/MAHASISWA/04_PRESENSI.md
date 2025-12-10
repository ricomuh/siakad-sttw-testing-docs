# Testing: Presensi (Mahasiswa)

**Status**: ✅ **COMPLETED**  
**Role**: Mahasiswa  
**Route**: `/mahasiswa/presensi`  
**Controller**: `Mahasiswa\PresensiController`  
**Model**: `PresensiMahasiswa`, `SesiKelas`, `JadwalPerkuliahan`

## 📊 Test Results

- **Total Tests**: 18
- **Passing**: 18 (100%)
- **Test File**: `tests/Feature/Mahasiswa/PresensiTest.php`
- **Execution Time**: 1.20s

---

## 📋 Checklist Testing

### 1. Operations

- [x] **Index**: View rekap presensi all mata kuliah
- [x] **Store**: Self-checkin presensi (jika allowed)
- [x] **Filter**: By mata kuliah, bulan

### 2. Authorization

- [x] Mahasiswa hanya bisa view presensi sendiri
- [x] Cannot view presensi mahasiswa lain
- [x] Can only checkin for own classes (KRS)

### 3. Presensi Display

- [x] List presensi grouped by mata kuliah
- [x] Show: Pertemuan, Tanggal, Status, Keterangan
- [x] Show: Total Hadir, Izin, Sakit, Alfa
- [x] Show: Attendance percentage per mata kuliah

### 4. Attendance Percentage

```
Attendance % = (Hadir + Izin + Sakit) / Total Pertemuan × 100
```

- [x] Calculate per mata kuliah
- [x] Show progress bar (visual indicator)
- [x] Flag jika < 75% (risk tidak bisa ujian)

### 5. Status Types

- [x] **Hadir**: Present
- [x] **Izin**: Excused absence (with reason)
- [x] **Sakit**: Sick (with reason/surat)
- [x] **Alfa**: Absent without permission

### 6. Self-Checkin Feature

- [x] **If Enabled**: Mahasiswa bisa checkin sendiri
- [x] **Validation**: Must be within sesi time (± 15 menit)
- [ ] **Validation**: Must be on campus (GPS/IP check - optional) (not implemented)
- [x] **Validation**: Sesi must be open (is_open = true)
- [ ] **QR Code**: Scan QR from dosen (optional) (not implemented)

### 7. QR Code Attendance

- [ ] Dosen generate QR code per sesi (not implemented)
- [ ] Mahasiswa scan QR via mobile (not implemented)
- [ ] Validation: QR expires after 10 minutes (not implemented)
- [ ] Validation: QR unique per sesi (not implemented)
- [ ] Auto checkin as "Hadir" (not implemented)

### 8. Late Checkin

- [x] If > 15 menit late: Flag as "Terlambat"
- [ ] Dosen can approve/reject late checkin (not implemented)
- [ ] Count as Hadir if approved, Alfa if rejected (not implemented)

### 9. Excuse Request

- [ ] Mahasiswa bisa request Izin/Sakit retroactive (not implemented)
- [ ] Upload surat: Izin/Sakit (PDF, JPG)
- [ ] Alasan: Text field (required)
- [ ] Dosen approve/reject request

### 10. Rekap Per Mata Kuliah

- [ ] Show table: Pertemuan 1-16
- [ ] Show status icon: ✅ Hadir, 📝 Izin, 🤒 Sakit, ❌ Alfa
- [ ] Show date & topic per pertemuan
- [ ] Show total attendance stats

### 11. Warning System

- [ ] **Alert if < 75%**: "Anda berisiko tidak bisa mengikuti ujian"
- [ ] **Email Notification**: Sent when attendance drops below threshold
- [ ] **Dashboard Widget**: Show attendance risk

### 12. Block Ujian Access

- [ ] If attendance < 75%: Cannot access kartu ujian
- [ ] Show error message: "Kehadiran tidak memenuhi syarat"
- [ ] Dosen/Kaprodi can override (special permission)

### 13. Attendance History

- [ ] View all semesters attendance
- [ ] Filter: By semester, tahun akademik
- [ ] Export to PDF/Excel

### 14. Integration

- [ ] Presensi data from PresensiMahasiswa table
- [ ] Sesi data from SesiKelas
- [ ] Linked to KRS (only show enrolled courses)
- [ ] Affects kartu ujian access

### 15. Edge Cases

- [ ] Sesi dibatalkan (dosen tidak masuk)
- [ ] Sesi reschedule
- [ ] Mahasiswa terlambat masuk kelas
- [ ] Network error saat checkin
- [ ] Duplicate checkin attempt

---

## 🔗 Related

- [Presensi Mahasiswa (Dosen)](../DOSEN/02_PRESENSI_MAHASISWA.md)
- [Jadwal Mengajar (Dosen)](../DOSEN/01_JADWAL_MENGAJAR.md)
- [Kartu Ujian](05_KARTU_UJIAN.md)
