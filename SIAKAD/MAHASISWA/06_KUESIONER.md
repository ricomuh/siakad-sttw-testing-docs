# Testing: Kuesioner Evaluasi Dosen

**Status**: ⚪ [TODO]  
**Role**: Mahasiswa  
**Route**: `/mahasiswa/kuesioner/{periode}/{jenis}`  
**Controller**: `Mahasiswa\KuesionerController`  
**Model**: `KuesionerPertanyaan`, `KuesionerJawaban`, `KuesionerKategori`

---

## 📋 Checklist Testing

### 1. Operations

-   [ ] **Form**: Display kuesioner per periode
-   [ ] **Submit**: Simpan jawaban kuesioner
-   [ ] **Validation**: Check if already submitted

### 2. Authorization

-   [ ] Mahasiswa hanya bisa isi kuesioner periode aktif
-   [ ] Cannot submit multiple times (per periode)
-   [ ] Must be enrolled in classes to evaluate

### 3. Periode Kuesioner

-   [ ] **UTS**: Mid-semester evaluation
-   [ ] **UAS**: End-semester evaluation
-   [ ] Periode defined by KalenderPendidikan
-   [ ] Auto open/close based on dates

### 4. Jenis Kuesioner

-   [ ] **Evaluasi Dosen**: Per dosen pengampu
-   [ ] **Evaluasi Mata Kuliah**: Per mata kuliah
-   [ ] **Evaluasi Sarana**: Facilities evaluation
-   [ ] **Evaluasi Layanan**: Service evaluation

### 5. Kuesioner Structure

-   [ ] Multiple categories (Mengajar, Komunikasi, Penguasaan Materi)
-   [ ] Multiple questions per category
-   [ ] Question types: Likert scale (1-5), Multiple choice, Text
-   [ ] Optional comment field

### 6. Question Categories

-   [ ] **Kemampuan Mengajar**: Teaching skills
-   [ ] **Penguasaan Materi**: Subject mastery
-   [ ] **Komunikasi**: Communication
-   [ ] **Kedisiplinan**: Punctuality & discipline
-   [ ] **Evaluasi**: Assessment fairness

### 7. Likert Scale (1-5)

-   [ ] **5**: Sangat Baik / Sangat Setuju
-   [ ] **4**: Baik / Setuju
-   [ ] **3**: Cukup / Netral
-   [ ] **2**: Kurang / Tidak Setuju
-   [ ] **1**: Sangat Kurang / Sangat Tidak Setuju

### 8. Form Display

-   [ ] List dosen & mata kuliah yang diajar semester ini
-   [ ] Show: Nama Dosen, Mata Kuliah
-   [ ] Radio buttons for Likert scale questions
-   [ ] Textarea for optional comments
-   [ ] Progress indicator (X/Y pertanyaan)

### 9. Validation

-   [ ] All required questions must be answered
-   [ ] Scale value: 1-5 only
-   [ ] Comment: Optional, max 500 chars
-   [ ] Cannot skip dosen (must evaluate all)

### 10. Submit Kuesioner

-   [ ] **Validation**: Check all questions answered
-   [ ] **Save**: Batch insert to kuesioner_jawaban
-   [ ] **Timestamp**: submitted_at
-   [ ] **Anonymous**: No name stored (only mahasiswa_id for tracking)
-   [ ] **Lock**: Cannot edit after submit

### 11. Submission Check

-   [ ] Check if mahasiswa already submitted for this periode
-   [ ] Query: kuesioner_jawaban WHERE mahasiswa_id AND periode
-   [ ] Show message: "Anda sudah mengisi kuesioner"
-   [ ] Cannot resubmit

### 12. Anonymity

-   [ ] Jawaban anonymous to dosen
-   [ ] Only aggregate results visible to dosen
-   [ ] Mahasiswa_id for admin tracking only
-   [ ] Comments anonymous (no identifiable info)

### 13. Reminder System

-   [ ] Email reminder H-7, H-3, H-1 before deadline
-   [ ] Dashboard notification: "Isi kuesioner evaluasi"
-   [ ] Block access to nilai if not submitted (optional policy)

### 14. Incentive System (Optional)

-   [ ] Cannot view nilai UTS/UAS before submitting kuesioner
-   [ ] Block kartu ujian access if not submitted
-   [ ] Policy configurable by admin

### 15. Integration

-   [ ] Dosen list from FormasiDosen (current semester)
-   [ ] Mata kuliah from KRS approved
-   [ ] Results aggregated for dosen dashboard
-   [ ] Used for dosen performance evaluation

### 16. Reports (Admin/Dosen View)

-   [ ] Aggregate score per category
-   [ ] Average score per question
-   [ ] Response rate: X% mahasiswa submitted
-   [ ] Comments list (anonymous)
-   [ ] Trend analysis (compare with previous semesters)

### 17. Edge Cases

-   [ ] Periode kuesioner belum dibuka
-   [ ] Periode kuesioner sudah ditutup
-   [ ] Mahasiswa tidak ada kelas semester ini
-   [ ] Dosen tidak ada di kuesioner (cuti/resign)
-   [ ] Network error saat submit

---

## 🔗 Related

-   [Jadwal Mengajar (Dosen)](../DOSEN/01_JADWAL_MENGAJAR.md)
-   [KRS](01_KRS.md)
-   [Kalender Pendidikan (Admin)](../ADMIN/09_KALENDER_PENDIDIKAN.md)
