# Testing: Kalender Pendidikan Dinamis

**Status**: ⚪ [TODO]  
**Role**: Admin  
**Route**: `/siakad/kalender-pendidikan`  
**Controller**: `KalenderPendidikanController`  
**Model**: `KalenderPendidikan`

---

## 📋 Checklist Testing

### 1. CRUD Operations

-   [ ] **Index**: Calendar view dengan events per bulan/minggu
-   [ ] **Create**: Tambah event akademik (tanggal mulai, selesai, jenis)
-   [ ] **Edit**: Update event (tanggal, deskripsi)
-   [ ] **Delete**: Hapus event
-   [ ] **API Get Events**: `/siakad/api/kalender-akademik` untuk FullCalendar

### 2. Validation

-   [ ] Judul event wajib (max 255)
-   [ ] Tanggal mulai wajib
-   [ ] Tanggal selesai >= tanggal mulai
-   [ ] Jenis event enum: Perkuliahan, UTS, UAS, Libur, Pendaftaran, Yudisium
-   [ ] Tahun akademik wajib
-   [ ] Semester wajib (Ganjil/Genap)

### 3. Business Logic

-   [ ] Event tidak overlap dengan event sejenis
-   [ ] Periode UTS/UAS tidak boleh dalam periode perkuliahan
-   [ ] Auto-calculate jumlah hari efektif perkuliahan (exclude weekend & libur)
-   [ ] Minimal 16 minggu efektif untuk perkuliahan

### 4. Integration

-   [ ] API return JSON untuk FullCalendar format
-   [ ] Filter events by tahun akademik + semester
-   [ ] Color coding by jenis event
-   [ ] Click event → modal detail

### 5. UI/UX

-   [ ] FullCalendar integration (month, week, day view)
-   [ ] Drag & drop untuk reschedule event
-   [ ] Modal form untuk create/edit event
-   [ ] Badge jumlah event per hari
-   [ ] Export kalender to PDF/iCal

### 6. Edge Cases

-   [ ] Event lintas tahun (Desember - Januari)
-   [ ] Event recurring (libur nasional tiap tahun)
-   [ ] Update tanggal event yang sudah dimulai → confirm
-   [ ] Delete event dengan relasi (misal: periode pendaftaran KRS)

---

## 🔗 Related

-   [Jadwal Perkuliahan](10_JADWAL_PERKULIAHAN.md)
-   [Monitoring KRS](15_MONITORING_KRS.md)
