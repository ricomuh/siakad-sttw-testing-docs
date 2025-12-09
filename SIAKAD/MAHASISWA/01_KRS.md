# Testing: KRS (Kartu Rencana Studi)

**Status**: ⚪ [TODO]  
**Role**: Mahasiswa  
**Route**: `/mahasiswa/krs`  
**Controller**: `Mahasiswa\KrsController`  
**Model**: `Krs`, `MataKuliah`, `JadwalPerkuliahan`, `KrsLog`

---

## 📋 Checklist Testing

### 1. Operations

- [ ] **Index**: View KRS current semester
- [ ] **Store**: Tambah mata kuliah ke KRS
- [ ] **Destroy**: Hapus mata kuliah dari KRS
- [ ] **Submit**: Submit KRS untuk approval Dosen PA

### 2. Authorization

- [ ] Mahasiswa hanya bisa akses KRS sendiri
- [ ] Cannot access KRS mahasiswa lain
- [ ] Must be within periode KRS (tanggal buka-tutup)
- [ ] Cannot edit after status = 'Disetujui'

### 3. View KRS

- [ ] Show mata kuliah yang sudah diambil (status Draft/Diajukan)
- [ ] Show: Kode MK, Nama MK, SKS, Semester
- [ ] Show: Dosen Pengampu, Jadwal (Hari, Jam)
- [ ] Show: Total SKS diambil
- [ ] Show: Max SKS allowed (based on IPK)
- [ ] Show: Status per mata kuliah

### 4. Tambah Mata Kuliah

- [ ] List mata kuliah available (semester berjalan)
- [ ] Filter: By semester, program studi
- [ ] Show: Jadwal, dosen, ruangan, sisa kuota
- [ ] Validation: Max SKS limit
- [ ] Validation: Prerequisite check
- [ ] Validation: Schedule conflict detection
- [ ] Validation: Already passed check

### 5. Max SKS Validation

- [ ] **IPK >= 3.0**: Max 24 SKS
- [ ] **IPK 2.5 - 2.99**: Max 20 SKS
- [ ] **IPK < 2.5**: Max 18 SKS
- [ ] **Semester 1**: Max 20 SKS (no IPK yet)
- [ ] Show warning when approaching limit

### 6. Prerequisite Check

- [ ] Check mata kuliah prasyarat
- [ ] Prasyarat must have nilai >= C (or custom threshold)
- [ ] Show error message with prasyarat list
- [ ] Allow override by admin (special case)

### 7. Schedule Conflict Detection

- [ ] Detect jadwal bentrok (same hari + overlapping time)
- [ ] Check across all KRS items
- [ ] Show conflict details (mata kuliah, waktu)
- [ ] Prevent adding conflicting jadwal

### 8. Already Passed Check

- [ ] Check if mahasiswa already passed this mata kuliah
- [ ] Nilai huruf >= C = passed
- [ ] Show warning "Mata kuliah sudah lulus"
- [ ] Prevent re-taking passed courses (except perbaikan nilai)

### 9. Delete Mata Kuliah

- [ ] Can delete if status = Draft
- [ ] Cannot delete if status = Diajukan/Disetujui
- [ ] Recalculate total SKS after delete
- [ ] Log activity to krs_logs

### 10. Submit KRS

- [ ] **Min SKS**: Minimal 12 SKS (except semester akhir)
- [ ] **Status Change**: Draft → Diajukan
- [ ] **Timestamp**: submitted_at
- [ ] **Notification**: Notify Dosen PA
- [ ] **Lock**: Cannot edit after submit (only delete if not approved)

### 11. KRS Status Flow

- [ ] **Draft**: Mahasiswa sedang menyusun
- [ ] **Diajukan**: Menunggu approval Dosen PA
- [ ] **Disetujui**: Approved by Dosen PA
- [ ] **Ditolak**: Rejected with reason
- [ ] **Partial**: Sebagian disetujui, sebagian ditolak

### 12. Resubmit After Rejection

- [ ] If rejected, status back to Draft
- [ ] Mahasiswa can edit rejected items
- [ ] Show rejection reason from Dosen PA
- [ ] Can resubmit after fixing

### 13. KRS History

- [ ] View KRS log (krs_logs table)
- [ ] Show: Action, timestamp, user
- [ ] Actions: Created, Submitted, Approved, Rejected
- [ ] Audit trail complete

### 14. Periode KRS

- [ ] Check if within periode KRS (tanggal_buka - tanggal_tutup)
- [ ] Show countdown timer to deadline
- [ ] Cannot add/delete outside periode
- [ ] Can still view after periode closed

### 15. Integration

- [ ] Approved KRS → Generate RombonganBelajar
- [ ] Approved KRS → Create Jadwal Mahasiswa
- [ ] Sync with Presensi (list mata kuliah)
- [ ] Sync with E-Learning (enroll mahasiswa)

### 16. Edge Cases

- [ ] KRS di luar periode
- [ ] Mata kuliah tidak dibuka semester ini
- [ ] Kuota kelas penuh
- [ ] Mahasiswa cuti/non-aktif
- [ ] Perubahan jadwal setelah KRS disetujui

---

## 🔗 Related

- [Validasi KRS (Dosen PA)](../DOSEN/04_VALIDASI_KRS.md)
- [Monitoring KRS (Admin)](../ADMIN/15_MONITORING_KRS.md)
- [Mata Kuliah](../ADMIN/05_MASTER_DATA_MATA_KULIAH.md)
