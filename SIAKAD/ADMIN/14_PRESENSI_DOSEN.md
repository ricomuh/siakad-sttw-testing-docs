# Testing: Presensi Dosen

**Status**: ✅ [TESTED]  
**Role**: Admin (View-only)  
**Route**: `/siakad/presensi-dosen`  
**Controller**: `PresensiDosenController`  
**Model**: `PresensiDosen`

---

## 📋 Checklist Testing

### 1. Operations (Read-only for Admin)

-   [ ] **Index**: List presensi dosen per bulan
-   [ ] **Show**: Detail presensi 1 dosen
-   [ ] **Filter**: By dosen, by tanggal range

### 2. Auto-Generation

-   [ ] Auto-create dari jadwal mengajar dosen
-   [ ] Status default: Scheduled
-   [ ] Dosen input via Dosen Portal (not admin)

### 3. Statistics

-   [ ] Persentase kehadiran dosen per bulan
-   [ ] Total jam mengajar efektif
-   [ ] Laporan ketidakhadiran

### 4. Reports

-   [ ] Export rekap presensi dosen per semester
-   [ ] Grafik kehadiran bulanan

---

## 🔗 Related

-   [Dosen](04_MASTER_DATA_DOSEN.md)
-   [Jadwal Perkuliahan](10_JADWAL_PERKULIAHAN.md)
