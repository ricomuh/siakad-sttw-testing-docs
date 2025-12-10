# Testing: Jadwal Ujian

**Status**: ✅ [TESTED]  
**Role**: Admin  
**Route**: `/siakad/jadwal-ujian`  
**Controller**: `JadwalUjianController`  
**Model**: `JadwalUjian`

---

## 📋 Checklist Testing

### 1. CRUD Operations

-   [ ] Index, Create, Show, Edit, Delete jadwal ujian
-   [ ] Filter: by prodi, by mata kuliah, by jenis (UTS/UAS)

### 2. Validation

-   [ ] Mata kuliah ID, ruangan ID, tanggal, waktu wajib
-   [ ] Jenis ujian enum: UTS, UAS, Remedial
-   [ ] Tanggal ujian dalam periode UTS/UAS (cek kalender pendidikan)
-   [ ] Ruangan tidak bentrok

### 3. Business Logic

-   [ ] Ruangan capacity >= jumlah peserta
-   [ ] Jadwal ujian tidak bentrok dengan perkuliahan
-   [ ] Generate kartu ujian untuk mahasiswa
-   [ ] Pengawas ujian assignment (dosen piket)

### 4. Integration

-   [ ] API check room availability
-   [ ] Generate nomor peserta ujian
-   [ ] Print kartu ujian batch

---

## 🔗 Related

-   [Kalender Pendidikan](09_KALENDER_PENDIDIKAN.md)
-   [Ruangan](03_MASTER_DATA_RUANGAN.md)
