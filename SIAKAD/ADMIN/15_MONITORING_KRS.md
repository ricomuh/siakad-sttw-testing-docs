# Testing: Monitoring KRS

**Status**: ⚪ [TODO]  
**Role**: Admin  
**Route**: `/siakad/monitoring-krs`  
**Controller**: `MonitoringKrsController`  
**Model**: `Krs`, `KrsLog`, `PeriodeKrs`

---

## 📋 Checklist Testing

### 1. Operations

-   [ ] **Index**: Dashboard monitoring KRS per prodi
-   [ ] **Show**: Detail KRS 1 mahasiswa
-   [ ] **Periode Management**: CRUD periode KRS (tanggal buka/tutup)
-   [ ] **Statistics**: Total mahasiswa, total SKS, status distribution

### 2. Periode KRS

-   [ ] **Create Periode**: Set tanggal buka, tanggal tutup per prodi/semester
-   [ ] **Validation**: Tanggal tutup > tanggal buka
-   [ ] **Business Logic**: Hanya 1 periode aktif per prodi per semester
-   [ ] **Auto-close**: Periode otomatis close setelah tanggal tutup

### 3. KRS Validation Rules

-   [ ] **Max SKS per semester**:

    -   IPK >= 3.0 → max 24 SKS
    -   IPK 2.5-2.99 → max 20 SKS
    -   IPK < 2.5 → max 18 SKS

-   [ ] **Prerequisites check**: Mata kuliah prasyarat harus lulus (nilai min C)
-   [ ] **Conflict detection**: Jadwal tidak boleh bentrok
-   [ ] **Status validation**: Hanya mahasiswa Aktif bisa KRS

### 4. Approval Workflow

-   [ ] Status KRS: Draft → Diajukan → Disetujui/Ditolak
-   [ ] Mahasiswa submit → Dosen PA review → Approve/Reject
-   [ ] Auto-approve jika <= max SKS tanpa conflict
-   [ ] Notification ke mahasiswa saat status berubah

### 5. Reports & Analytics

-   [ ] Progress KRS per prodi (X% sudah submit, Y% disetujui)
-   [ ] Mahasiswa belum KRS (reminder list)
-   [ ] Statistik: Rata-rata SKS diambil, mata kuliah terpopuler
-   [ ] Export KRS per mahasiswa to PDF

### 6. Integration

-   [ ] Auto-generate rombongan belajar dari KRS approved
-   [ ] Update kapasitas kelas saat KRS disetujui
-   [ ] Sync dengan jadwal perkuliahan

### 7. KRS Log (Audit Trail)

-   [ ] Log semua perubahan KRS
-   [ ] Fields: action, old_value, new_value, user_id, timestamp
-   [ ] View history per mahasiswa

---

## 🔗 Related

-   [Mahasiswa](06_MASTER_DATA_MAHASISWA.md)
-   [Mata Kuliah](02_MASTER_DATA_MATA_KULIAH.md)
-   [Jadwal Perkuliahan](10_JADWAL_PERKULIAHAN.md)
-   [Validasi KRS (Dosen)](../DOSEN/VALIDASI_KRS.md)
