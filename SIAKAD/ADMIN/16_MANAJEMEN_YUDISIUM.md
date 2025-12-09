# Testing: Manajemen Yudisium

**Status**: ⚪ [TODO]  
**Role**: Admin, Waket1  
**Route**: `/siakad/manajemen-yudisium`  
**Controller**: `ManajemenYudisiumController`  
**Model**: `ApprovalYudisium`, `KartuUjianAkses`

---

## 📋 Checklist Testing

### 1. Operations

-   [ ] **Index**: List mahasiswa eligible for yudisium per prodi
-   [ ] **Kunci Nilai**: Lock nilai mahasiswa (prevent changes)
-   [ ] **Buka Akses**: Unlock nilai untuk revisi
-   [ ] **Reset Kunci**: Reset all locks (emergency)

### 2. Eligibility Check

-   [ ] Minimal 144 SKS lulus (S1)
-   [ ] IPK minimal 2.0
-   [ ] Tidak ada nilai E atau F
-   [ ] Tidak ada tunggakan keuangan (check SIMKEU)
-   [ ] TA/Skripsi lulus
-   [ ] Semua nilai sudah locked (tidak ada yang pending)

### 3. Kunci Nilai Process

-   [ ] **Lock Single Mahasiswa**: Kunci nilai 1 mahasiswa
-   [ ] **Lock Batch**: Kunci nilai multiple mahasiswa
-   [ ] **Validation**: Semua mata kuliah sudah ada nilai
-   [ ] **Effect**: Nilai tidak bisa diubah oleh dosen
-   [ ] **Log**: Catat siapa, kapan kunci nilai

### 4. Buka Akses (Unlock)

-   [ ] **Authorization**: Hanya Waket1 atau admin
-   [ ] **Reason Required**: Wajib isi alasan unlock
-   [ ] **Time Limit**: Buka akses max 7 hari
-   [ ] **Notification**: Notify dosen terkait
-   [ ] **Log**: Audit trail unlock

### 5. Kartu Ujian Access

-   [ ] Generate kartu ujian setelah nilai locked
-   [ ] Kartu ujian berisi: Foto, NIM, Nama, Prodi, IPK, Total SKS
-   [ ] QR Code untuk validasi
-   [ ] Print batch kartu ujian per prodi

### 6. Reports

-   [ ] List mahasiswa siap yudisium
-   [ ] List mahasiswa pending (with reasons)
-   [ ] Export to Excel for yudisium committee
-   [ ] Transkip nilai final

### 7. Integration

-   [ ] Check SIMKEU untuk tunggakan
-   [ ] Notify mahasiswa via email saat eligible
-   [ ] Generate SK yudisium (Surat Keputusan)

---

## 🔗 Related

-   [Mahasiswa](06_MASTER_DATA_MAHASISWA.md)
-   [Monitoring KRS](15_MONITORING_KRS.md)
-   [Bobot Nilai (Waket1)](../WAKET1/BOBOT_NILAI.md)
