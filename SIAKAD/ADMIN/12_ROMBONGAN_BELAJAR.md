# Testing: Rombongan Belajar

**Status**: ⚪ [TODO]  
**Role**: Admin (Read-only, Auto-generated)  
**Route**: `/siakad/rombongan-belajar`  
**Controller**: `RombonganBelajarController`  
**Model**: `RombonganBelajar`

---

## 📋 Checklist Testing

### 1. View Operations (Read-only)

-   [ ] **Index**: List rombongan belajar per semester
-   [ ] **Show**: Detail rombongan dengan daftar mahasiswa
-   [ ] **Filter**: By prodi, by angkatan, by semester

### 2. Auto-Generation Logic

-   [ ] Auto-generate dari KRS yang disetujui
-   [ ] Group mahasiswa by jadwal_perkuliahan_id
-   [ ] Update otomatis saat ada KRS baru disetujui
-   [ ] Kode rombel format: {kode_prodi}-{semester}-{kelas}
    -   Example: IF-2024-1A, IF-2024-1B

### 3. Business Logic

-   [ ] Jumlah mahasiswa per rombel
-   [ ] Statistik: rata-rata IPK rombel
-   [ ] List mata kuliah yang diambil rombel
-   [ ] Dosen wali kelas (optional)

### 4. Integration

-   [ ] Sync dengan KRS (real-time atau scheduled job)
-   [ ] Relasi dengan jadwal perkuliahan
-   [ ] Relasi dengan mahasiswa via pivot

### 5. Reports

-   [ ] Export daftar hadir per rombel
-   [ ] Export nilai per rombel
-   [ ] Summary akademik rombel

---

## 🔗 Related

-   [Monitoring KRS](15_MONITORING_KRS.md)
-   [Jadwal Perkuliahan](10_JADWAL_PERKULIAHAN.md)
