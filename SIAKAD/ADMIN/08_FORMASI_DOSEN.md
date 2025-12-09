# Testing: Formasi Dosen

**Status**: ⚪ [TODO]  
**Role**: Admin  
**Route**: `/siakad/formasi-dosen`  
**Controller**: `FormasiDosenController`  
**Model**: `FormasiDosen`

---

## 📋 Checklist Testing

### 1. CRUD Operations

-   [ ] **Index**: List formasi dengan filter (dosen, prodi, mata kuliah, semester)
-   [ ] **Create**: Assign dosen ke mata kuliah untuk semester tertentu
-   [ ] **Show**: Detail formasi dengan jadwal mengajar
-   [ ] **Edit**: Update SKS laku, deskripsi
-   [ ] **Delete**: Hapus formasi jika belum ada jadwal

### 2. Validation

-   [ ] Dosen ID wajib dan exist
-   [ ] Mata Kuliah ID wajib dan exist
-   [ ] Program Studi ID wajib dan exist
-   [ ] SKS Laku wajib dan <= mata kuliah.total_sks
-   [ ] Tahun akademik wajib (contoh: 2024/2025)
-   [ ] Semester wajib (Ganjil/Genap)
-   [ ] **Unique constraint**: dosen_id + mata_kuliah_id + program_studi_id + tahun_akademik + semester

### 3. Business Logic

-   [ ] Validasi teaching load dosen (max 16 SKS per semester)
-   [ ] Dosen harus dari program studi yang sama (atau izin lintas prodi)
-   [ ] Team teaching: multiple dosen untuk 1 mata kuliah
-   [ ] SKS laku distribution: total SKS laku semua dosen = mata kuliah.sks

### 4. Integration

-   [ ] Dynamic dropdown: pilih prodi → filter mata kuliah dari kurikulum aktif
-   [ ] API: `/siakad/formasi-dosen/mata-kuliah/{prodi}`
-   [ ] Relasi dengan jadwal perkuliahan (hasMany)
-   [ ] Formasi generate jadwal otomatis (optional)

### 5. UI/UX

-   [ ] Tabel formasi group by dosen atau by mata kuliah
-   [ ] Badge teaching load indicator (hijau < 12, kuning 12-16, merah > 16)
-   [ ] Filter multi-select (multiple prodi, multiple dosen)
-   [ ] Button: Duplicate formasi semester lalu

### 6. Authorization

-   [ ] Admin: Full CRUD
-   [ ] Akademik: Full CRUD
-   [ ] Dosen: View own formasi only

### 7. Performance

-   [ ] Eager loading: with(['dosen', 'mataKuliah', 'programStudi'])
-   [ ] Cache teaching load per dosen (invalidate on update)

### 8. Edge Cases

-   [ ] Dosen mengajar mata kuliah di multiple prodi
-   [ ] Update formasi setelah ada mahasiswa KRS → impact analysis
-   [ ] Delete formasi dengan jadwal aktif → prevent
-   [ ] SKS laku > SKS mata kuliah → error

---

## 🔗 Related

-   [Dosen](04_MASTER_DATA_DOSEN.md)
-   [Mata Kuliah](02_MASTER_DATA_MATA_KULIAH.md)
-   [Jadwal Perkuliahan](10_JADWAL_PERKULIAHAN.md)
