# Testing: Jadwal Perkuliahan

**Status**: ⚪ [TODO]  
**Role**: Admin  
**Route**: `/siakad/jadwal-perkuliahan`  
**Controller**: `JadwalPerkuliahanController`  
**Model**: `JadwalPerkuliahan`

---

## 📋 Checklist Testing

### 1. CRUD Operations

-   [ ] **Index**: List jadwal per prodi/dosen/ruangan dengan filter
-   [ ] **Create**: Buat jadwal dari formasi dosen
-   [ ] **Show**: Detail jadwal dengan daftar mahasiswa (KRS)
-   [ ] **Edit**: Update waktu, ruangan, status
-   [ ] **Delete**: Hapus jadwal jika belum ada KRS

### 2. Validation

-   [ ] Formasi dosen ID wajib dan exist
-   [ ] Ruangan ID wajib dan exist
-   [ ] Hari wajib (Senin-Jumat atau include Sabtu)
-   [ ] Waktu mulai wajib (format HH:mm)
-   [ ] Waktu selesai > waktu mulai
-   [ ] Tahun akademik + semester wajib
-   [ ] **Unique constraint**: formasi_dosen_id + hari + waktu (no duplicate jadwal for same class)

### 3. Conflict Detection

-   [ ] **Ruangan conflict**: Ruangan tidak boleh double booking

    -   Check same hari + overlapping time
    -   API: `/siakad/api/ruangan/available`

-   [ ] **Dosen conflict**: Dosen tidak bisa mengajar 2 kelas bersamaan

    -   Check via formasi_dosen_id

-   [ ] **Mahasiswa conflict**: Mahasiswa KRS tidak boleh bentrok jadwal
    -   Validate saat mahasiswa ambil KRS
    -   Real-time check

### 4. Business Logic

-   [ ] Kapasitas ruangan >= jumlah mahasiswa KRS
-   [ ] Durasi kuliah sesuai SKS (2 SKS = 100 menit, 3 SKS = 150 menit)
-   [ ] Jadwal dalam range kalender akademik (periode perkuliahan)
-   [ ] Auto-generate kode kelas (IF-101-A, IF-101-B untuk parallel class)

### 5. Integration

-   [ ] Relasi dengan formasi dosen (belongsTo)
-   [ ] Relasi dengan ruangan (belongsTo) + availability check
-   [ ] Relasi dengan KRS (hasMany through mahasiswa)
-   [ ] Generate sesi kelas untuk 16 pertemuan
-   [ ] Auto-create rombongan belajar dari KRS

### 6. UI/UX

-   [ ] Timetable view (grid: hari vs waktu)
-   [ ] Drag-drop untuk reschedule
-   [ ] Color coding per mata kuliah atau per dosen
-   [ ] Conflict indicator (red highlight)
-   [ ] Quick filter: per prodi, per dosen, per ruangan, per hari

### 7. Scope Methods

-   [ ] `tahunAkademik($year)` - Filter by tahun akademik
-   [ ] `semester($sem)` - Filter by semester
-   [ ] `active()` - Only active schedules
-   [ ] `hari($day)` - Filter by day

### 8. Authorization

-   [ ] Admin: Full CRUD
-   [ ] Akademik: Full CRUD
-   [ ] Dosen: View own schedule only

### 9. Performance

-   [ ] Eager load: with(['formasiDosen.mataKuliah', 'ruangan'])
-   [ ] Cache timetable per prodi (invalidate on change)
-   [ ] Conflict check optimized query

### 10. Edge Cases

-   [ ] Jadwal cross midnight (22:00 - 01:00)
-   [ ] Jadwal Sabtu/Minggu (weekend classes)
-   [ ] Update jadwal setelah mahasiswa KRS → notify students
-   [ ] Parallel classes (IF-101-A, IF-101-B) same time different room
-   [ ] Block schedule (2 sesi berturut-turut tanpa jeda)

---

## 🔗 Related

-   [Formasi Dosen](08_FORMASI_DOSEN.md)
-   [Ruangan](03_MASTER_DATA_RUANGAN.md)
-   [Rombongan Belajar](12_ROMBONGAN_BELAJAR.md)
-   [Monitoring KRS](15_MONITORING_KRS.md)
