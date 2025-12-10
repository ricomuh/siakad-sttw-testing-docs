# Testing: KHS (Kartu Hasil Studi)

**Status**: ✅ **COMPLETED**  
**Role**: Mahasiswa  
**Route**: `/mahasiswa/khs`  
**Controller**: `Mahasiswa\KhsController`  
**Model**: `NilaiMahasiswa`, `MataKuliah`, `Mahasiswa`

## 📊 Test Results

- **Total Tests**: 14
- **Passing**: 14 (100%)
- **Test File**: `tests/Feature/Mahasiswa/KhsTest.php`
- **Execution Time**: 1.22s

---

## 📋 Checklist Testing

### 1. Operations

- [x] **Index**: View KHS all semesters
- [x] **Print**: Generate PDF KHS per semester
- [x] **Filter**: By semester, tahun akademik

### 2. Authorization

- [x] Mahasiswa hanya bisa view KHS sendiri
- [x] Cannot access KHS mahasiswa lain
- [x] All semesters visible (no restriction)

### 3. KHS Display

- [x] List all nilai per semester
- [x] Show: Kode MK, Nama MK, SKS
- [x] Show: Nilai Angka, Nilai Huruf
- [x] Show: Bobot (SKS × Nilai Angka)
- [x] Group by semester

### 4. Semester Summary

- [x] **IPS (IP Semester)**: Average per semester
- [x] **IPK (IP Kumulatif)**: Cumulative GPA
- [x] **Total SKS Lulus**: Sum of passed credits
- [x] **Total SKS Diambil**: Sum of all credits
- [x] **Total SKS Gagal**: Credits with E/F grade

### 5. IPS Calculation

```
IPS = Σ(SKS × Nilai Angka) / Σ(SKS)
```

- [x] Calculate per semester
- [x] Round to 2 decimal places
- [x] Only count graded courses (exclude P/F)

### 6. IPK Calculation

```
IPK = Σ(SKS × Nilai Angka untuk semua semester) / Σ(SKS semua semester)
```

- [x] Calculate cumulative
- [x] Include all semesters
- [x] Update after each semester

### 7. Grade Conversion

- [x] **A**: 4.0 (Nilai >= 85)
- [x] **AB**: 3.5 (Nilai 80-84)
- [x] **B**: 3.0 (Nilai 75-79)
- [x] **BC**: 2.5 (Nilai 70-74)
- [x] **C**: 2.0 (Nilai 65-69)
- [x] **D**: 1.0 (Nilai 55-64)
- [x] **E**: 0.0 (Nilai < 55)

Note: Conversion follows BobotNilai table (configurable by Waket1)

### 8. Passed Credits

- [ ] Nilai huruf >= C = Lulus (2.0)
- [ ] Nilai D/E = Tidak Lulus
- [ ] Count only passed credits for Total SKS Lulus

### 9. Semester Grouping

- [ ] Group nilai by semester
- [ ] Sort: Oldest to newest
- [ ] Show semester label: "Semester 1 - 2023/2024 Ganjil"
- [ ] Collapsible accordion per semester

### 10. Print KHS

- [ ] **Single Semester**: Print 1 semester only
- [ ] **All Semesters**: Print transkrip lengkap
- [ ] **PDF Format**: A4, portrait
- [ ] **Header**: Logo, nama PT, identitas mahasiswa
- [ ] **Footer**: Page number, print date

### 11. PDF Content

- [ ] Identitas mahasiswa (NIM, Nama, Prodi)
- [ ] Semester & Tahun Akademik
- [ ] Table: No, Kode MK, Mata Kuliah, SKS, Nilai Huruf, Bobot
- [ ] Summary: IPS, IPK, Total SKS
- [ ] TTD placeholder (Kaprodi/Waket1)

### 12. Historical Data

- [ ] Show nilai from all past semesters
- [ ] Cannot edit historical nilai
- [ ] Only admin/waket1 can unlock & revise

### 13. Empty State

- [ ] If no nilai yet: "Belum ada nilai tercatat"
- [ ] If semester aktif: "Nilai belum diinput dosen"
- [ ] Show helpful message

### 14. Performance

- [ ] Cache IPK calculation (avoid recalculating every load)
- [ ] Eager load relationships (mata_kuliah)
- [ ] Pagination for semesters (if > 8 semesters)

### 15. Integration

- [ ] Nilai from NilaiMahasiswa table
- [ ] Grade conversion from BobotNilai
- [ ] Mata kuliah info from MataKuliah
- [ ] Used by Yudisium eligibility check

---

## 🔗 Related

- [Manajemen Yudisium (Admin)](../ADMIN/16_MANAJEMEN_YUDISIUM.md)
- [Bobot Nilai (Waket1)](../WAKET1/BOBOT_NILAI.md)
- [Cetak Dokumen (Admin)](../ADMIN/17_CETAK_DOKUMEN_AKADEMIK.md)
