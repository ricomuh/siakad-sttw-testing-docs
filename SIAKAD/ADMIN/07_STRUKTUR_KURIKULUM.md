# Testing: Struktur Kurikulum

**Status**: ⚪ [TODO]  
**Role**: Admin  
**Route**: `/siakad/kurikulum`  
**Controller**: `KurikulumController`  
**Model**: `Kurikulum`

---

## 📋 Checklist Testing

### 1. CRUD Operations

-   [ ] **Index**: List kurikulum per program studi dengan filter tahun akademik
-   [ ] **Create**: Buat kurikulum baru dengan tahun berlaku
-   [ ] **Show**: Detail kurikulum dengan daftar mata kuliah per semester
-   [ ] **Edit**: Update kurikulum (nama, tahun berlaku, status)
-   [ ] **Delete**: Soft delete jika tidak ada mata kuliah atau mahasiswa menggunakan

### 2. Validation

-   [ ] Nama kurikulum wajib (contoh: "Kurikulum 2024")
-   [ ] Tahun berlaku wajib dan format YYYY
-   [ ] Program studi ID wajib dan exist
-   [ ] Unique: kombinasi program_studi_id + tahun_berlaku
-   [ ] Status enum: Aktif, Non-Aktif, Draft

### 3. Business Logic

-   [ ] Hanya 1 kurikulum aktif per program studi
-   [ ] Set kurikulum lain jadi non-aktif saat activate kurikulum baru
-   [ ] Total SKS kurikulum = sum(mata kuliah.sks)
-   [ ] Validasi minimal total SKS (144 untuk S1)

### 4. Integration

-   [ ] Relasi dengan mata kuliah (hasMany)
-   [ ] Mata kuliah tersusun per semester (1-8)
-   [ ] Copy kurikulum existing → create new with mata kuliah sama
-   [ ] Template kurikulum OBE (Outcome-Based Education)

### 5. UI/UX

-   [ ] Grid view struktur kurikulum semester 1-8
-   [ ] Drag-drop mata kuliah antar semester
-   [ ] Summary: Total SKS per semester, Total SKS keseluruhan
-   [ ] Button: Duplicate kurikulum, Export PDF

### 6. Authorization

-   [ ] Admin: Full CRUD
-   [ ] Akademik: View, Edit (no delete)

### 7. Edge Cases

-   [ ] Kurikulum tanpa mata kuliah (draft)
-   [ ] Update tahun berlaku yang sudah dipakai mahasiswa → warning
-   [ ] Delete kurikulum dengan mahasiswa aktif → prevent

---

## 🔗 Related

-   [Mata Kuliah](02_MASTER_DATA_MATA_KULIAH.md)
-   [Program Studi](01_MASTER_DATA_PROGRAM_STUDI.md)
