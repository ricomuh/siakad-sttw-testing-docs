# Testing: E-Learning (Mahasiswa)

**Status**: ⚪ [TODO]  
**Role**: Mahasiswa  
**Route**: `/mahasiswa/elearning`  
**Controller**: `Mahasiswa\ElearningController`  
**Model**: `MataKuliah`, `MateriSAP`, `Tugas`, `TugasSubmission`

---

## 📋 Checklist Testing

### 1. Operations

-   [ ] **Index**: List mata kuliah yang diambil (KRS)
-   [ ] **Show**: Detail mata kuliah (materi, tugas)
-   [ ] **Show Materi**: View materi SAP
-   [ ] **Show Tugas**: View tugas & submit
-   [ ] **Store Submission**: Upload tugas

### 2. Authorization

-   [ ] Mahasiswa hanya bisa akses mata kuliah yang diambil (KRS)
-   [ ] Cannot access mata kuliah yang tidak diambil
-   [ ] Verified via Krs::where('mahasiswa_id', auth()->id())

### 3. Mata Kuliah List

-   [ ] Show mata kuliah from approved KRS
-   [ ] Show: Nama MK, SKS, Dosen Pengampu
-   [ ] Show: Progress (materi dibaca, tugas submitted)
-   [ ] Show: Nilai tugas (if graded)
-   [ ] Card layout with image/icon

### 4. Mata Kuliah Detail

-   [ ] **Tab Informasi**: Deskripsi, RPS, Capaian
-   [ ] **Tab Materi**: List materi per minggu
-   [ ] **Tab Tugas**: List tugas & submissions
-   [ ] **Tab Penilaian**: Nilai komponen (UTS, UAS, Tugas)
-   [ ] **Tab Classmates**: List teman sekelas

### 5. Materi SAP

-   [ ] List materi ordered by minggu (1-16)
-   [ ] Show: Minggu, Topik, Deskripsi
-   [ ] Download file (PDF, PPT, DOC)
-   [ ] Watch video (embedded YouTube/Vimeo)
-   [ ] Mark as read (optional tracking)

### 6. Materi Tracking

-   [ ] Track which materi has been viewed
-   [ ] Progress bar: X/16 materi dibaca
-   [ ] Last viewed timestamp
-   [ ] View count per materi

### 7. Tugas List

-   [ ] Show all tugas for this mata kuliah
-   [ ] Show: Judul, Deskripsi, Deadline
-   [ ] Show: Status (Belum Submit, Submitted, Graded)
-   [ ] Show: Nilai (if graded)
-   [ ] Sort: Upcoming deadlines first

### 8. Tugas Detail

-   [ ] Full deskripsi (rich text)
-   [ ] File attachment from dosen (if any)
-   [ ] Deadline (date & time)
-   [ ] Max file size & allowed types
-   [ ] Late submission policy

### 9. Submit Tugas

-   [ ] **Upload File**: Support multiple file types
-   [ ] **File Validation**: Max size (default 5MB)
-   [ ] **Allowed Types**: pdf, doc, docx, zip, jpg, png
-   [ ] **Komentar**: Optional text comment
-   [ ] **Timestamp**: submitted_at
-   [ ] **Status Change**: Belum Submit → Submitted

### 10. Submission Validation

-   [ ] Deadline check: Warning if late
-   [ ] File size check: Max size per tugas
-   [ ] File type check: Only allowed extensions
-   [ ] Required fields: File + (optional komentar)
-   [ ] One submission per tugas (or allow resubmit if not graded)

### 11. Late Submission

-   [ ] Allow late submission (if dosen allows)
-   [ ] Flag as "Late" (submitted_at > deadline)
-   [ ] Show warning: "Pengumpulan terlambat"
-   [ ] Potential penalty (dosen decision)

### 12. Resubmit Tugas

-   [ ] If not yet graded: Can resubmit (update file)
-   [ ] If graded: Cannot resubmit (unless dosen allows revision)
-   [ ] Show previous submission history

### 13. View Submission

-   [ ] Show uploaded file (download link)
-   [ ] Show submission date
-   [ ] Show grade (if graded)
-   [ ] Show dosen feedback (komentar)

### 14. Graded Tugas

-   [ ] Show nilai: 0-100
-   [ ] Show nilai huruf (A, B, C, etc.)
-   [ ] Show dosen feedback
-   [ ] Show graded_at timestamp

### 15. Notifications

-   [ ] New materi uploaded
-   [ ] New tugas created
-   [ ] Tugas deadline reminder (H-3, H-1)
-   [ ] Tugas graded (nilai sudah keluar)

### 16. Integration

-   [ ] Materi from dosen MataKuliahDetailController
-   [ ] Tugas from dosen MataKuliahDetailController
-   [ ] Nilai sync to NilaiMahasiswa (komponen Tugas)
-   [ ] Progress tracked for gamification (optional)

### 17. Edge Cases

-   [ ] Mata kuliah without materi
-   [ ] Mata kuliah without tugas
-   [ ] Tugas sudah lewat deadline
-   [ ] File upload failed (network error)
-   [ ] Duplicate submission

---

## 🔗 Related

-   [Mata Kuliah Detail (Dosen)](../DOSEN/03_MATA_KULIAH_DETAIL.md)
-   [KRS](01_KRS.md)
-   [Mata Kuliah (Admin)](../ADMIN/05_MASTER_DATA_MATA_KULIAH.md)
