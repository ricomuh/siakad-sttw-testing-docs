# Testing: Kuesioner Evaluasi Dosen

**Status**: ✅ **COMPLETED**  
**Role**: Mahasiswa  
**Route**: `/mahasiswa/kuesioner/{periode}/{jenis}`  
**Controller**: `Mahasiswa\KuesionerController`  
**Model**: `KuesionerPertanyaan`, `KuesionerJawaban`, `KuesionerKategori`

## 📊 Test Results

- **Total Tests**: 22
- **Passing**: 22 (100%)
- **Test File**: `tests/Feature/Mahasiswa/KuesionerTest.php`
- **Execution Time**: 2.19s

---

## 📋 Checklist Testing

### 1. Operations

- [x] **Form**: Display kuesioner per periode
- [x] **Submit**: Simpan jawaban kuesioner
- [x] **Validation**: Check if already submitted

### 2. Authorization

- [x] Mahasiswa hanya bisa isi kuesioner periode aktif
- [x] Cannot submit multiple times (per periode)
- [x] Must be enrolled in classes to evaluate

### 3. Periode Kuesioner

- [x] **UTS**: Mid-semester evaluation
- [x] **UAS**: End-semester evaluation
- [x] Periode defined by PeriodeAkademik
- [x] Case insensitive jenis ujian handling

### 4. Jenis Kuesioner

- [x] **Evaluasi Dosen**: Per dosen pengampu (UTS)
- [x] **Evaluasi Umum**: General evaluation (UAS)
- [x] Different data structure for UTS vs UAS
- [x] UTS includes dosen_id, UAS does not

### 5. Kuesioner Structure

- [x] Multiple categories with aktif scope
- [x] Multiple questions per category
- [x] Question types: Scale (1-5), Choice, Text
- [x] Questions ordered by urutan scope

### 6. Question Categories

- [x] **Category loading**: Using aktif and jenisUjian scopes
- [x] **Question loading**: Eager loaded with pertanyaan relation
- [x] **Validation**: Redirects if no kategori or empty questions
- [x] **Active check**: Only shows active categories

### 7. Answer Types

- [x] **Skala**: Numeric scale (1-5) saved to nilai_skala
- [x] **Pilihan**: Choice option saved to jawaban_pilihan
- [x] **Text**: Text input saved to jawaban_text
- [x] Type detection based on answer value

### 8. Form Display

- [x] List dosen & mata kuliah untuk UTS
- [x] Empty dosen list for UAS
- [x] Show: Kategori with pertanyaan
- [x] View variables: mahasiswa, periode, jenisUjian, kategori, dosenList

### 9. Validation

- [x] All questions required (jawaban array required)
- [x] Jawaban must be array
- [x] Each answer item required
- [x] Mahasiswa record must exist

### 10. Submit Kuesioner

- [x] **Validation**: Check all questions answered
- [x] **Save**: UpdateOrCreate to kuesioner_jawaban
- [x] **Transaction**: DB transaction for data integrity
- [x] **UTS**: Nested loop (pertanyaan → dosen)
- [x] **UAS**: Single loop (pertanyaan only)
- [x] **Lock**: Can update existing answers (updateOrCreate pattern)

### 11. Submission Check

- [x] Check if mahasiswa already submitted via KartuUjianAkses
- [x] Query: kuesioner_selesai flag
- [x] Redirect to exam card if already completed
- [x] Can resubmit (updateOrCreate allows updates)

### 12. Integration with KartuUjian

- [x] Sets kuesioner_selesai = true after submit
- [x] Unlocks exam card access
- [x] Redirects to kartu-ujian.show after success
- [x] Seamless integration flow

### 13. UTS Specific Features

- [x] Query approved KRS for dosen list
- [x] Match semester using LIKE pattern
- [x] Build array of [dosen, mata_kuliah] pairs
- [x] Nested answer structure with dosen_id

### 14. UAS Specific Features

- [x] Empty dosen list (no lecturer evaluation)
- [x] Flat answer structure (no dosen_id)
- [x] General evaluation only
- [x] Simpler submission logic

### 15. Integration

- [x] Dosen list from Krs approved with FormasiDosen
- [x] Mata kuliah from KRS
- [x] Results saved to kuesioner_jawaban
- [x] KartuUjianAkses updated for completion tracking

### 16. Edge Cases

- [x] Mahasiswa not found (redirect to dashboard)
- [x] Invalid jenis ujian (abort 404)
- [x] Already completed (redirect to exam card)
- [x] No active kategori (redirect with error)
- [x] Empty pertanyaan (redirect with error)
- [x] Transaction rollback on error

---

## ✅ Test Coverage

### Authorization Tests (4)

- ✅ requires authentication to view kuesioner form
- ✅ requires mahasiswa role to view kuesioner form
- ✅ requires authentication to submit kuesioner
- ✅ requires mahasiswa role to submit kuesioner

### Form Display Tests (9)

- ✅ mahasiswa can view kuesioner form for UTS
- ✅ mahasiswa can view kuesioner form for UAS
- ✅ form redirects if mahasiswa not found
- ✅ form validates jenis ujian parameter
- ✅ form redirects if kuesioner already completed
- ✅ form redirects if no active kuesioner available
- ✅ form redirects if kategori has no active questions
- ✅ form includes dosen list for UTS
- ✅ form has empty dosen list for UAS

### Submission Tests (9)

- ✅ mahasiswa can submit UAS kuesioner
- ✅ mahasiswa can submit UTS kuesioner with dosen evaluation
- ✅ submit redirects if mahasiswa not found
- ✅ submit validates jawaban is required
- ✅ submit validates jawaban is array
- ✅ submit can handle text answers
- ✅ submit can handle choice answers
- ✅ submit updates existing answers on resubmit
- ✅ submit handles case insensitive exam type

---

## 🏭 Factories Created

```php
database/factories/KuesionerKategoriFactory.php
database/factories/KuesionerPertanyaanFactory.php
database/factories/KuesionerJawabanFactory.php
```

### KuesionerKategoriFactory

- States: `uts()`, `uas()`, `inactive()`
- Default: active with random jenis_ujian

### KuesionerPertanyaanFactory

- States: `skala()`, `pilihan()`, `text()`, `inactive()`, `optional()`
- Default: active, required, random type

### KuesionerJawabanFactory

- States: `uts()`, `uas()`, `skala()`, `pilihan()`, `text()`
- Default: random answer type

---

## 🔑 Key Differences: UTS vs UAS

### UTS (Mid-Semester)

```php
// Nested structure: evaluate each lecturer
$request->jawaban = [
    pertanyaan_id => [
        dosen_id => answer_value,
        dosen_id => answer_value,
    ]
];

// Saved with dosen_id
KuesionerJawaban::updateOrCreate([
    'dosen_id' => $dosenId, // ← HAS dosen_id
    ...
]);
```

### UAS (End-Semester)

```php
// Flat structure: general evaluation
$request->jawaban = [
    pertanyaan_id => answer_value
];

// Saved without dosen_id
KuesionerJawaban::updateOrCreate([
    'dosen_id' => null, // ← NO dosen_id
    ...
]);
```

---

## 🔗 Related

- [Jadwal Mengajar (Dosen)](../DOSEN/01_JADWAL_MENGAJAR.md)
- [KRS](01_KRS.md)
- [Kalender Pendidikan (Admin)](../ADMIN/09_KALENDER_PENDIDIKAN.md)
