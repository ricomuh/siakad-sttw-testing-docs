# Testing: Log Bimbingan (Dosen PA)

**Status**: ⚪ [TODO]  
**Role**: Dosen (sebagai Dosen PA)  
**Route**: `/siakad/dosen/log-bimbingan`  
**Controller**: `Dosen\LogBimbinganController`  
**Model**: `LogBimbingan`, `Mahasiswa`, `Dosen`

---

## 📋 Checklist Testing

### 1. Operations

-   [ ] **Index**: List log bimbingan yang sudah dicatat
-   [ ] **Show**: Detail log bimbingan
-   [ ] **Create**: Form tambah log bimbingan
-   [ ] **Store**: Simpan log bimbingan baru
-   [ ] **Filter**: By mahasiswa, tanggal, topik

### 2. Authorization

-   [ ] Dosen hanya bisa akses log mahasiswa bimbingannya
-   [ ] Check: mahasiswa.dosen_pa_id === auth()->user()->dosen->id
-   [ ] Cannot create log for mahasiswa dosen PA lain
-   [ ] Cannot edit/delete log (immutable record)

### 3. Log Bimbingan List

-   [ ] Show semua log bimbingan per mahasiswa bimbingan
-   [ ] Grouping: By mahasiswa
-   [ ] Sort: Latest first
-   [ ] Show: Tanggal, topik, status follow-up
-   [ ] Count: Total bimbingan per mahasiswa

### 4. Create Log Bimbingan

-   [ ] **Select Mahasiswa**: Dropdown mahasiswa bimbingan
-   [ ] **Tanggal**: Date picker (cannot future date)
-   [ ] **Topik**: Select (Akademik, Skripsi/TA, Karir, Pribadi, Lainnya)
-   [ ] **Deskripsi**: Rich text editor, min 50 chars
-   [ ] **Hasil**: Text area (hasil pembahasan)
-   [ ] **Tindak Lanjut**: Optional, next action items
-   [ ] **Status**: Select (Selesai, Perlu Follow-up, Urgent)

### 5. Validation Rules

-   [ ] Mahasiswa: Required, must be dosen's mentee
-   [ ] Tanggal: Required, cannot be future date
-   [ ] Topik: Required, enum validation
-   [ ] Deskripsi: Required, min 50 chars
-   [ ] Hasil: Optional, max 1000 chars
-   [ ] Tindak Lanjut: Optional
-   [ ] Status: Required, enum validation

### 6. Topik Categories

-   [ ] **Akademik**: Konsultasi KRS, nilai, akademik
-   [ ] **Skripsi/TA**: Bimbingan Tugas Akhir
-   [ ] **Karir**: Konsultasi karir, magang, pekerjaan
-   [ ] **Pribadi**: Masalah pribadi, kesehatan mental
-   [ ] **Lainnya**: Topik lain di luar kategori

### 7. Status Types

-   [ ] **Selesai**: Bimbingan selesai, tidak perlu follow-up
-   [ ] **Perlu Follow-up**: Butuh bimbingan lanjutan
-   [ ] **Urgent**: Butuh perhatian segera

### 8. Log Detail View

-   [ ] Show mahasiswa info (NIM, Nama, Semester, IPK)
-   [ ] Show tanggal bimbingan
-   [ ] Show topik dan deskripsi lengkap
-   [ ] Show hasil pembahasan
-   [ ] Show tindak lanjut (if any)
-   [ ] Show status
-   [ ] Show dosen pembimbing (created_by)

### 9. Mahasiswa Bimbingan Summary

-   [ ] List mahasiswa bimbingan dengan card/row
-   [ ] Show: Total bimbingan
-   [ ] Show: Last bimbingan date
-   [ ] Show: Status terakhir (Selesai, Follow-up, Urgent)
-   [ ] Show: Total bimbingan per topik (chart)
-   [ ] Flag: Mahasiswa yang lama tidak bimbingan (> 30 hari)

### 10. Reports & Statistics

-   [ ] Total bimbingan per mahasiswa
-   [ ] Total bimbingan per topik
-   [ ] Total bimbingan per bulan (trend chart)
-   [ ] Average bimbingan frequency per mahasiswa
-   [ ] List mahasiswa butuh follow-up
-   [ ] Export to Excel/PDF

### 11. Integration

-   [ ] Link ke profil mahasiswa
-   [ ] Notification to mahasiswa after log created
-   [ ] Dashboard widget: Upcoming follow-ups
-   [ ] Reminder: Mahasiswa yang lama tidak bimbingan

### 12. Notifications

-   [ ] Email mahasiswa after bimbingan logged
-   [ ] Reminder dosen if urgent follow-up
-   [ ] Alert if mahasiswa > 30 hari tidak bimbingan
-   [ ] Monthly summary report to dosen

### 13. Search & Filter

-   [ ] Search: By mahasiswa name/NIM
-   [ ] Filter: By topik
-   [ ] Filter: By status
-   [ ] Filter: By date range
-   [ ] Filter: By mahasiswa

### 14. Edge Cases

-   [ ] Mahasiswa tidak punya dosen PA
-   [ ] Dosen tidak punya mahasiswa bimbingan
-   [ ] Bimbingan di luar jam kerja
-   [ ] Duplicate log (same date, same mahasiswa)
-   [ ] Log tanpa tindak lanjut

### 15. Immutability

-   [ ] Log cannot be edited after creation
-   [ ] Log cannot be deleted
-   [ ] Only soft delete allowed (by admin/waket1)
-   [ ] Audit trail: created_at, created_by

---

## 🔗 Related

-   [Validasi KRS](04_VALIDASI_KRS.md)
-   [Mahasiswa](../ADMIN/06_MASTER_DATA_MAHASISWA.md)
-   [Dosen](../ADMIN/04_MASTER_DATA_DOSEN.md)
