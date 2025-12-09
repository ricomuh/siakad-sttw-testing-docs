# Testing: Mata Kuliah Detail (Dosen)

**Status**: ⚪ [TODO]  
**Role**: Dosen  
**Route**: `/siakad/dosen/mata-kuliah/{id}`  
**Controller**: `Dosen\MataKuliahDetailController`  
**Model**: `MataKuliah`, `MateriSAP`, `Tugas`, `KomponenPenilaian`, `NilaiMahasiswa`

---

## 📋 Checklist Testing

### 1. View Operations

- [ ] **Index**: List mata kuliah yang diajar dosen
- [ ] **Show**: Detail mata kuliah dengan tabs
- [ ] **Update Deskripsi**: Edit deskripsi mata kuliah

### 2. Tabs Structure

- [ ] **Tab Informasi**: Deskripsi, RPS, Capaian Pembelajaran
- [ ] **Tab Materi SAP**: List materi per minggu
- [ ] **Tab Tugas**: List tugas dan deadline
- [ ] **Tab Penilaian**: Komponen nilai dan input nilai
- [ ] **Tab Mahasiswa**: List mahasiswa KRS

### 3. Materi SAP Management

- [ ] **Create**: Tambah materi baru (minggu, topik, file)
- [ ] **Edit**: Update materi existing
- [ ] **Delete**: Hapus materi (soft delete)
- [ ] **Ordering**: Urutkan by minggu (1-16)
- [ ] **File Upload**: PDF, PPT, DOC (max 10MB)
- [ ] **Video Link**: Embed YouTube/Vimeo link

### 4. Materi Validation

- [ ] Minggu: 1-16, unique per mata kuliah
- [ ] Topik: Required, max 255 chars
- [ ] File: Optional, allowed types: pdf, ppt, pptx, doc, docx
- [ ] Video: Optional, valid URL format
- [ ] Deskripsi: Optional, rich text editor

### 5. Tugas Management

- [ ] **Create**: Buat tugas baru
- [ ] **Edit**: Update tugas existing
- [ ] **Delete**: Hapus tugas (cascade delete submissions)
- [ ] **Show Submissions**: List mahasiswa yang submit
- [ ] **Grade Submission**: Input nilai per submission

### 6. Tugas Validation

- [ ] Judul: Required, max 255 chars
- [ ] Deskripsi: Required, rich text
- [ ] Deadline: Required, future date
- [ ] Max File Size: Default 5MB, max 50MB
- [ ] Allowed File Types: pdf, doc, docx, zip
- [ ] Late Submission: Allow/disallow late submission

### 7. Grade Submission

- [ ] Input nilai: 0-100
- [ ] Komentar dosen: Optional feedback
- [ ] Status: Submitted → Graded
- [ ] Notifikasi mahasiswa saat dinilai
- [ ] Revision: Allow resubmission (jika nilai < 60)

### 8. Komponen Penilaian

- [ ] **Create**: Tambah komponen (UTS, UAS, Tugas, Quiz)
- [ ] **Edit**: Update komponen
- [ ] **Delete**: Hapus komponen (jika belum ada nilai)
- [ ] **Total Weight**: Must sum to 100%
- [ ] **Ordering**: Sort by bobot descending

### 9. Komponen Validation

- [ ] Nama: Required, unique per mata kuliah
- [ ] Bobot: Required, 0-100, total must = 100%
- [ ] Jenis: Enum (UTS, UAS, Tugas, Quiz, Partisipasi)
- [ ] Description: Optional

### 10. Input Nilai

- [ ] **Bulk Input**: Input nilai multiple mahasiswa
- [ ] **Per Komponen**: Input per komponen (UTS, UAS, dll)
- [ ] **Auto Calculate**: Hitung nilai akhir otomatis
- [ ] **Grade Conversion**: Nilai angka → huruf (A, B, C, D, E)
- [ ] **Lock Nilai**: Lock nilai akhir (prevent changes)

### 11. Nilai Validation

- [ ] Nilai: 0-100 per komponen
- [ ] Nilai Akhir: Weighted average
- [ ] Nilai Huruf: Based on bobot nilai (configurable)
- [ ] Status: Draft → Locked
- [ ] Cannot edit if locked (except by admin/waket1)

### 12. Export Nilai

- [ ] **Export to Excel**: Format template standard
- [ ] **Columns**: NIM, Nama, per Komponen, Nilai Akhir, Huruf
- [ ] **Filter**: By semester, tahun akademik
- [ ] **Filename**: `Nilai_{MataKuliah}_{Semester}_{Date}.xlsx`

### 13. Integration

- [ ] Materi visible to mahasiswa di portal
- [ ] Tugas submission via mahasiswa portal
- [ ] Nilai sync to transkrip
- [ ] Notify mahasiswa on new materi/tugas

### 14. Edge Cases

- [ ] Mata kuliah tanpa mahasiswa KRS
- [ ] Upload file duplicate name
- [ ] Submit tugas after deadline
- [ ] Change bobot after nilai diinput
- [ ] Delete komponen with existing nilai

---

## 🔗 Related

- [Jadwal Mengajar](01_JADWAL_MENGAJAR.md)
- [Mata Kuliah (Admin)](../ADMIN/05_MASTER_DATA_MATA_KULIAH.md)
- [Bobot Nilai (Waket1)](../WAKET1/BOBOT_NILAI.md)
