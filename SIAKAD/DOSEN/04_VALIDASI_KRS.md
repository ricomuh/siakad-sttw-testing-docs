# Testing: Validasi KRS (Dosen PA)

**Status**: ⚪ [TODO]  
**Role**: Dosen (sebagai Dosen PA)  
**Route**: `/siakad/dosen/validasi-krs`  
**Controller**: `Dosen\ValidasiKrsController`  
**Model**: `Krs`, `Mahasiswa`, `MataKuliah`, `KrsLog`

---

## 📋 Checklist Testing

### 1. Operations

-   [ ] **Index**: List mahasiswa bimbingan dengan status KRS
-   [ ] **Show**: Detail KRS mahasiswa
-   [ ] **Approve All**: Setujui semua mata kuliah KRS mahasiswa
-   [ ] **Reject All**: Tolak semua mata kuliah KRS mahasiswa
-   [ ] **Approve Item**: Setujui 1 mata kuliah saja
-   [ ] **Reject Item**: Tolak 1 mata kuliah dengan alasan

### 2. Authorization

-   [ ] Dosen hanya bisa validasi mahasiswa bimbingannya
-   [ ] Check: mahasiswa.dosen_pa_id === auth()->user()->dosen->id
-   [ ] Cannot validasi KRS mahasiswa dosen PA lain
-   [ ] Cannot validasi outside periode KRS

### 3. Mahasiswa Bimbingan List

-   [ ] Show list mahasiswa where dosen_pa_id = dosen login
-   [ ] Show: NIM, Nama, Semester, IPK
-   [ ] Show: Total SKS diambil
-   [ ] Show: Status KRS (Diajukan, Disetujui, Ditolak, Draft)
-   [ ] Filter: By status, semester, tahun akademik

### 4. KRS Detail View

-   [ ] List mata kuliah yang diambil mahasiswa
-   [ ] Show: Kode MK, Nama MK, SKS, Semester, Dosen Pengampu
-   [ ] Show: Jadwal (Hari, Jam)
-   [ ] Show: Status per mata kuliah (Diajukan, Disetujui, Ditolak)
-   [ ] Show: Conflict detection (bentrok jadwal)

### 5. Validation Rules

-   [ ] **Max SKS by IPK**:
    -   IPK >= 3.0: Max 24 SKS
    -   IPK 2.5 - 2.99: Max 20 SKS
    -   IPK < 2.5: Max 18 SKS
-   [ ] **Prerequisite Check**: Mata kuliah prasyarat harus lulus
-   [ ] **Schedule Conflict**: Tidak ada jadwal bentrok
-   [ ] **Already Passed**: Tidak boleh ambil MK yang sudah lulus
-   [ ] **Min SKS**: Minimal ambil 12 SKS (kecuali semester akhir)

### 6. Approve Process

-   [ ] **Validation**: Check all rules before approve
-   [ ] **Update Status**: Krs.status = 'Disetujui'
-   [ ] **Log Activity**: Catat di krs_logs
-   [ ] **Notification**: Notify mahasiswa via email/notif
-   [ ] **Timestamp**: approved_at, approved_by

### 7. Reject Process

-   [ ] **Alasan Required**: Wajib isi alasan penolakan
-   [ ] **Update Status**: Krs.status = 'Ditolak'
-   [ ] **Log Activity**: Catat alasan di krs_logs
-   [ ] **Notification**: Notify mahasiswa dengan alasan
-   [ ] **Allow Resubmit**: Mahasiswa bisa edit dan ajukan lagi

### 8. Partial Approval

-   [ ] Approve beberapa mata kuliah
-   [ ] Reject beberapa mata kuliah
-   [ ] Mahasiswa must resubmit rejected items
-   [ ] Status KRS = 'Partial' if mixed

### 9. Bulk Operations

-   [ ] **Approve All Mahasiswa**: Setujui KRS semua mahasiswa bimbingan
-   [ ] **Validation Check**: Skip mahasiswa yang tidak memenuhi syarat
-   [ ] **Report**: Show success/failed per mahasiswa
-   [ ] **Log**: Catat bulk operation

### 10. Reports & Statistics

-   [ ] Count mahasiswa by status (Draft, Diajukan, Disetujui, Ditolak)
-   [ ] Average SKS diambil per mahasiswa
-   [ ] List mahasiswa overload (> max SKS)
-   [ ] List mahasiswa pending validation
-   [ ] Export to Excel

### 11. Integration

-   [ ] KRS sync dengan rombongan belajar (after approved)
-   [ ] Generate jadwal mahasiswa (after approved)
-   [ ] Block changes after approved (lock KRS)
-   [ ] Allow revision with dosen PA approval

### 12. Edge Cases

-   [ ] Mahasiswa belum submit KRS (status Draft)
-   [ ] Periode KRS sudah ditutup
-   [ ] Mata kuliah tidak dibuka semester ini
-   [ ] Dosen PA tidak assigned
-   [ ] Mahasiswa cuti/non-aktif

---

## 🔗 Related

-   [Monitoring KRS (Admin)](../ADMIN/15_MONITORING_KRS.md)
-   [Mahasiswa](../ADMIN/06_MASTER_DATA_MAHASISWA.md)
-   [Mata Kuliah](../ADMIN/05_MASTER_DATA_MATA_KULIAH.md)
