# Testing: Presensi Mahasiswa

**Status**: ⚪ [TODO]  
**Role**: Admin (View/Edit)  
**Route**: `/siakad/presensi-mahasiswa`  
**Controller**: `PresensiMahasiswaController`  
**Model**: `PresensiMahasiswa`

---

## 📋 Checklist Testing

### 1. Operations

-   [ ] **Index**: List sesi kelas per jadwal perkuliahan
-   [ ] **Edit**: Input/update presensi batch per sesi
-   [ ] **Show**: Detail presensi 1 sesi

### 2. Validation

-   [ ] Sesi kelas ID wajib dan exist
-   [ ] Mahasiswa ID wajib dan exist
-   [ ] Status enum: Hadir, Izin, Sakit, Alfa
-   [ ] Tanggal presensi <= today (tidak bisa presensi masa depan)

### 3. Business Logic

-   [ ] Minimum kehadiran 75% untuk bisa ujian
-   [ ] Auto-calculate persentase kehadiran per mahasiswa
-   [ ] Warning jika < 75% (tampilkan di kartu ujian)
-   [ ] Lock presensi setelah 7 hari

### 4. Integration

-   [ ] Input presensi batch (1 sesi, all students)
-   [ ] Export rekap presensi to Excel
-   [ ] Notification ke mahasiswa jika Alfa > 3x

### 5. Authorization

-   [ ] Admin: Full access (view all, edit all)
-   [ ] Dosen: Edit own class only
-   [ ] Mahasiswa: View own only

---

## 🔗 Related

-   [Jadwal Perkuliahan](10_JADWAL_PERKULIAHAN.md)
-   [Mahasiswa](06_MASTER_DATA_MAHASISWA.md)
