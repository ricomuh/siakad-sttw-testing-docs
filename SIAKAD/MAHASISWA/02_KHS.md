# Testing: KHS (Kartu Hasil Studi)

**Status**: ⚪ [TODO]  
**Role**: Mahasiswa  
**Route**: `/mahasiswa/khs`  
**Controller**: `Mahasiswa\KhsController`  
**Model**: `NilaiMahasiswa`, `MataKuliah`, `Mahasiswa`

---

## 📋 Checklist Testing

### 1. Operations

- [ ] **Index**: View KHS all semesters
- [ ] **Print**: Generate PDF KHS per semester
- [ ] **Filter**: By semester, tahun akademik

### 2. Authorization

- [ ] Mahasiswa hanya bisa view KHS sendiri
- [ ] Cannot access KHS mahasiswa lain
- [ ] All semesters visible (no restriction)

### 3. KHS Display

- [ ] List all nilai per semester
- [ ] Show: Kode MK, Nama MK, SKS
- [ ] Show: Nilai Angka, Nilai Huruf
- [ ] Show: Bobot (SKS × Nilai Angka)
- [ ] Group by semester

### 4. Semester Summary

- [ ] **IPS (IP Semester)**: Average per semester
- [ ] **IPK (IP Kumulatif)**: Cumulative GPA
- [ ] **Total SKS Lulus**: Sum of passed credits
- [ ] **Total SKS Diambil**: Sum of all credits
- [ ] **Total SKS Gagal**: Credits with E/F grade

### 5. IPS Calculation

```
IPS = Σ(SKS × Nilai Angka) / Σ(SKS)
```

- [ ] Calculate per semester
- [ ] Round to 2 decimal places
- [ ] Only count graded courses (exclude P/F)

### 6. IPK Calculation

```
IPK = Σ(SKS × Nilai Angka untuk semua semester) / Σ(SKS semua semester)
```

- [ ] Calculate cumulative
- [ ] Include all semesters
- [ ] Update after each semester

### 7. Grade Conversion

- [ ] **A**: 4.0 (Nilai >= 85)
- [ ] **AB**: 3.5 (Nilai 80-84)
- [ ] **B**: 3.0 (Nilai 75-79)
- [ ] **BC**: 2.5 (Nilai 70-74)
- [ ] **C**: 2.0 (Nilai 65-69)
- [ ] **D**: 1.0 (Nilai 55-64)
- [ ] **E**: 0.0 (Nilai < 55)

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
