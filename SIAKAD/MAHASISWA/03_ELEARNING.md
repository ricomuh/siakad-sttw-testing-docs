# Testing: E-Learning (Mahasiswa)

**Status**: ✅ **COMPLETED**  
**Role**: Mahasiswa  
**Route**: `/mahasiswa/elearning`  
**Controller**: `Mahasiswa\ElearningController`  
**Model**: `MataKuliah`, `MateriSAP`, `Tugas`, `TugasSubmission`

## 📊 Test Results

- **Total Tests**: 24
- **Passing**: 24 (100%)
- **Test File**: `tests/Feature/Mahasiswa/ElearningTest.php`
- **Execution Time**: 1.47s

---

## 📋 Checklist Testing

### 1. Operations

- [x] **Index**: List mata kuliah yang diambil (KRS)
- [x] **Show**: Detail mata kuliah (materi, tugas)
- [x] **Show Materi**: View materi SAP
- [x] **Show Tugas**: View tugas & submit
- [x] **Store Submission**: Upload tugas

### 2. Authorization

- [x] Mahasiswa hanya bisa akses mata kuliah yang diambil (KRS)
- [x] Cannot access mata kuliah yang tidak diambil
- [x] Verified via Krs::where('mahasiswa_id', auth()->id())

### 3. Mata Kuliah List

- [x] Show mata kuliah from approved KRS
- [x] Show: Nama MK, SKS, Dosen Pengampu
- [x] Show: Progress (materi dibaca, tugas submitted)
- [x] Show: Nilai tugas (if graded)
- [x] Card layout with image/icon

### 4. Mata Kuliah Detail

- [x] **Tab Informasi**: Deskripsi, RPS, Capaian
- [x] **Tab Materi**: List materi per minggu
- [x] **Tab Tugas**: List tugas & submissions
- [x] **Tab Penilaian**: Nilai komponen (UTS, UAS, Tugas)
- [x] **Tab Classmates**: List teman sekelas

### 5. Materi SAP

- [x] List materi ordered by minggu (1-16)
- [x] Show: Minggu, Topik, Deskripsi
- [x] Download file (PDF, PPT, DOC)
- [x] Watch video (embedded YouTube/Vimeo)
- [ ] Mark as read (optional tracking) (not implemented)

### 6. Materi Tracking

- [ ] Track which materi has been viewed (not implemented)
- [ ] Progress bar: X/16 materi dibaca (not implemented)
- [ ] Last viewed timestamp (not implemented)
- [ ] View count per materi (not implemented)

### 7. Tugas List

- [x] Show all tugas for this mata kuliah
- [x] Show: Judul, Deskripsi, Deadline
- [x] Show: Status (Belum Submit, Submitted, Graded)
- [x] Show: Nilai (if graded)
- [x] Sort: Upcoming deadlines first

### 8. Tugas Detail

- [x] Full deskripsi (rich text)
- [x] File attachment from dosen (if any)
- [x] Deadline (date & time)
- [x] Max file size & allowed types
- [x] Late submission policy

### 9. Submit Tugas

- [x] **Upload File**: Support multiple file types
- [x] **File Validation**: Max size (default 5MB)
- [x] **Allowed Types**: pdf, doc, docx, zip, jpg, png
- [x] **Komentar**: Optional text comment
- [x] **Timestamp**: submitted_at
- [x] **Status Change**: Belum Submit → Submitted

### 10. Submission Validation

- [x] Deadline check: Warning if late
- [x] File size check: Max size per tugas
- [x] File type check: Only allowed extensions
- [x] Required fields: File + (optional komentar)
- [x] One submission per tugas (or allow resubmit if not graded)

### 11. Late Submission

- [x] Allow late submission (if dosen allows)
- [ ] Flag as "Late" (submitted_at > deadline)
- [ ] Show warning: "Pengumpulan terlambat"
- [ ] Potential penalty (dosen decision)

### 12. Resubmit Tugas

- [ ] If not yet graded: Can resubmit (update file)
- [ ] If graded: Cannot resubmit (unless dosen allows revision)
- [ ] Show previous submission history

### 13. View Submission

- [ ] Show uploaded file (download link)
- [ ] Show submission date
- [ ] Show grade (if graded)
- [ ] Show dosen feedback (komentar)

### 14. Graded Tugas

- [ ] Show nilai: 0-100
- [ ] Show nilai huruf (A, B, C, etc.)
- [ ] Show dosen feedback
- [ ] Show graded_at timestamp

### 15. Notifications

- [ ] New materi uploaded
- [ ] New tugas created
- [ ] Tugas deadline reminder (H-3, H-1)
- [ ] Tugas graded (nilai sudah keluar)

### 16. Integration

- [ ] Materi from dosen MataKuliahDetailController
- [ ] Tugas from dosen MataKuliahDetailController
- [ ] Nilai sync to NilaiMahasiswa (komponen Tugas)
- [ ] Progress tracked for gamification (optional)

### 17. Edge Cases

- [ ] Mata kuliah without materi
- [ ] Mata kuliah without tugas
- [ ] Tugas sudah lewat deadline
- [ ] File upload failed (network error)
- [ ] Duplicate submission

---

## 🔗 Related

- [Mata Kuliah Detail (Dosen)](../DOSEN/03_MATA_KULIAH_DETAIL.md)
- [KRS](01_KRS.md)
- [Mata Kuliah (Admin)](../ADMIN/05_MASTER_DATA_MATA_KULIAH.md)
