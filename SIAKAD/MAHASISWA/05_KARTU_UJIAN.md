# Testing: Kartu Ujian

**Status**: ⚪ [TODO]  
**Role**: Mahasiswa  
**Route**: `/mahasiswa/kartu-ujian`  
**Controller**: `Mahasiswa\KartuUjianController`  
**Model**: `KartuUjianAkses`, `JadwalUjian`, `Mahasiswa`

---

## 📋 Checklist Testing

### 1. Operations

-   [ ] **Index**: List periode ujian (UTS, UAS)
-   [ ] **Show**: View & print kartu ujian per periode
-   [ ] **Validation**: Check eligibility before access

### 2. Authorization

-   [ ] Mahasiswa hanya bisa view kartu ujian sendiri
-   [ ] Cannot access kartu ujian mahasiswa lain
-   [ ] Must meet eligibility requirements

### 3. Periode Ujian

-   [ ] **UTS**: Ujian Tengah Semester
-   [ ] **UAS**: Ujian Akhir Semester
-   [ ] Show periode available (current semester)
-   [ ] Show status: Available, Locked, Expired

### 4. Eligibility Check

-   [ ] **Attendance >= 75%**: Per mata kuliah
-   [ ] **No Financial Debt**: Check from SIMKEU (optional)
-   [ ] **KRS Approved**: Status = 'Disetujui'
-   [ ] **Not Suspended**: Status mahasiswa = 'Aktif'

### 5. Kartu Ujian Display

-   [ ] Header: Logo PT, Nama PT
-   [ ] Foto mahasiswa (from profile)
-   [ ] Identitas: NIM, Nama, Prodi, Semester
-   [ ] QR Code: For validation
-   [ ] Periode: UTS/UAS, Tahun Akademik

### 6. Jadwal Ujian List

-   [ ] List jadwal ujian per mata kuliah
-   [ ] Show: Mata Kuliah, Hari, Tanggal, Waktu
-   [ ] Show: Ruangan, Dosen Pengampu
-   [ ] Show: Eligibility status per mata kuliah
-   [ ] Sort by tanggal ascending

### 7. Eligibility Per Mata Kuliah

-   [ ] **Eligible**: ✅ Bisa ikut ujian
-   [ ] **Not Eligible**: ❌ Tidak bisa (attendance < 75%)
-   [ ] Show reason: "Kehadiran 65% (minimal 75%)"
-   [ ] Show warning icon

### 8. QR Code

-   [ ] Generate unique QR per mahasiswa per periode
-   [ ] QR contains: NIM, periode, hash
-   [ ] Validation: Scan QR at exam location
-   [ ] Security: Signed QR (prevent forgery)

### 9. Print Kartu Ujian

-   [ ] **PDF Format**: A4, landscape
-   [ ] **Layout**: Compact, fit 2 cards per page
-   [ ] **Content**: All jadwal ujian in table
-   [ ] **Footer**: Disclaimer, TTD placeholder
-   [ ] **Filename**: `KartuUjian_UTS_NIM_Date.pdf`

### 10. Lock Mechanism

-   [ ] Kartu ujian locked after access granted
-   [ ] Cannot print multiple times (optional: allow reprint)
-   [ ] Track: first_accessed_at, last_printed_at
-   [ ] Admin/Waket1 can unlock if needed

### 11. Access Log

-   [ ] Log kartu ujian access to kartu_ujian_akses table
-   [ ] Track: accessed_at, printed_at, IP address
-   [ ] Audit trail complete

### 12. Blocked Access

-   [ ] If not eligible: Show error message
-   [ ] List reasons: Attendance, debt, etc.
-   [ ] Contact info: "Hubungi Bagian Akademik"
-   [ ] No print button available

### 13. Special Permission

-   [ ] If attendance < 75%: Require permission
-   [ ] Mahasiswa submit request to Dosen PA/Kaprodi
-   [ ] Admin/Waket1 can grant access
-   [ ] Log: approved_by, reason, approved_at

### 14. Integration

-   [ ] Attendance data from PresensiMahasiswa
-   [ ] Financial check from SIMKEU (optional)
-   [ ] Jadwal ujian from JadwalUjian
-   [ ] QR validation at exam entrance (proctors)

### 15. Notifications

-   [ ] Email when kartu ujian available
-   [ ] Reminder H-7 before UTS/UAS
-   [ ] Alert if not eligible (with reasons)

### 16. Edge Cases

-   [ ] Periode ujian belum dibuka
-   [ ] Mahasiswa tidak ada jadwal ujian
-   [ ] Jadwal ujian berubah setelah print
-   [ ] QR code scanning error
-   [ ] Printer error

---

## 🔗 Related

-   [Presensi](04_PRESENSI.md)
-   [Jadwal Ujian (Admin)](../ADMIN/11_JADWAL_UJIAN.md)
-   [Manajemen Yudisium (Admin)](../ADMIN/16_MANAJEMEN_YUDISIUM.md)
