# Testing: Presensi (Mahasiswa)

**Status**: ⚪ [TODO]  
**Role**: Mahasiswa  
**Route**: `/mahasiswa/presensi`  
**Controller**: `Mahasiswa\PresensiController`  
**Model**: `PresensiMahasiswa`, `SesiKelas`, `JadwalPerkuliahan`

---

## 📋 Checklist Testing

### 1. Operations

- [ ] **Index**: View rekap presensi all mata kuliah
- [ ] **Store**: Self-checkin presensi (jika allowed)
- [ ] **Filter**: By mata kuliah, bulan

### 2. Authorization

- [ ] Mahasiswa hanya bisa view presensi sendiri
- [ ] Cannot view presensi mahasiswa lain
- [ ] Can only checkin for own classes (KRS)

### 3. Presensi Display

- [ ] List presensi grouped by mata kuliah
- [ ] Show: Pertemuan, Tanggal, Status, Keterangan
- [ ] Show: Total Hadir, Izin, Sakit, Alfa
- [ ] Show: Attendance percentage per mata kuliah

### 4. Attendance Percentage

```
Attendance % = (Hadir + Izin + Sakit) / Total Pertemuan × 100
```

- [ ] Calculate per mata kuliah
- [ ] Show progress bar (visual indicator)
- [ ] Flag jika < 75% (risk tidak bisa ujian)

### 5. Status Types

- [ ] **Hadir**: Present
- [ ] **Izin**: Excused absence (with reason)
- [ ] **Sakit**: Sick (with reason/surat)
- [ ] **Alfa**: Absent without permission

### 6. Self-Checkin Feature

- [ ] **If Enabled**: Mahasiswa bisa checkin sendiri
- [ ] **Validation**: Must be within sesi time (± 15 menit)
- [ ] **Validation**: Must be on campus (GPS/IP check - optional)
- [ ] **Validation**: Sesi must be open (is_open = true)
- [ ] **QR Code**: Scan QR from dosen (optional)

### 7. QR Code Attendance

- [ ] Dosen generate QR code per sesi
- [ ] Mahasiswa scan QR via mobile
- [ ] Validation: QR expires after 10 minutes
- [ ] Validation: QR unique per sesi
- [ ] Auto checkin as "Hadir"

### 8. Late Checkin

- [ ] If > 15 menit late: Flag as "Terlambat"
- [ ] Dosen can approve/reject late checkin
- [ ] Count as Hadir if approved, Alfa if rejected

### 9. Excuse Request

- [ ] Mahasiswa bisa request Izin/Sakit retroactive
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
