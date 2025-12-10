# Mata Kuliah Detail (E-Learning) - Testing Guide

## Overview

**Module**: Mata Kuliah Detail - E-Learning & Course Management  
**Controller**: `App\Http\Controllers\Dosen\MataKuliahDetailController`  
**Route Prefix**: `/siakad/dosen/mata-kuliah/{jadwal}`  
**Test File**: `tests/Feature/Dosen/MataKuliahDetailTest.php`  
**Status**: ✅ **COMPLETED**

## 📊 Test Results

**Total Tests**: 35 tests  
**Passing**: 35 tests (100%)  
**Assertions**: 109 assertions  
**Duration**: 1.98 seconds  
**Pass Rate**: ✅ 100%

## Purpose

Complete e-learning platform for dosen to manage course content and student assessments:

- **Deskripsi Tab**: Edit course info, WA group link, view enrolled students
- **Materi Tab**: Upload weekly materials (PDF, PPT, DOC), embed videos
- **Tugas Tab**: Create assignments, grade submissions
- **Penilaian Tab**: Define grade components (UTS, UAS, Tugas), batch input grades
- **Mahasiswa Tab**: View enrolled students list

## Tab Structure & Features

### 1. Deskripsi Tab

- Update course description
- Set WhatsApp group link
- List enrolled students (KRS Disetujui)

### 2. Materi Tab (Weekly Materials)

- Upload materials by week (Minggu 1-16)
- File types: PDF, PPT, PPTX, DOC, DOCX (max 10MB)
- Video links (YouTube/Vimeo)
- CRUD operations

### 3. Tugas Tab (Assignments)

- Create assignments with deadlines
- View submissions
- Grade individual submissions
- CRUD operations

### 4. Penilaian Tab (Grading)

- Define grade components (nama, bobot)
- Batch update student grades
- Validate total bobot ≤ 100%
- Lock detection (yudisium period)

### 5. Mahasiswa Tab

- List students with approved KRS
- Track enrollment

## Controller Methods Summary

**Show Method**: `GET /siakad/dosen/mata-kuliah/{jadwal}?tab={tab}`

- Loads different data based on tab parameter
- Eager loads relationships (formasiDosen, mataKuliah, programStudi, dosen, ruangan)

**Update Deskripsi**: `PATCH /siakad/dosen/mata-kuliah/{jadwal}/deskripsi`

**Materi CRUD**: 5 methods (create, store, edit, update, destroy)

**Tugas CRUD**: 6 methods (create, store, show, edit, update, destroy)

- Additional: `gradeTugasSubmission()` for grading

**Komponen CRUD**: 5 methods (create, store, edit, update, destroy)

- Validation: Total bobot cannot exceed 100%

**Batch Nilai**: `POST /siakad/dosen/mata-kuliah/{jadwal}/nilai`

- Batch update grades for all students
- Lock detection prevents updates during yudisium

## Test Coverage (35 tests)

### Authorization (2 tests)

1. Requires authentication
2. Requires dosen role

### Show Method (9 tests)

3. Dosen can view mata kuliah detail
4. Show returns 404 for non-existent jadwal
5. Show loads all relationships
6. Show with deskripsi tab loads mahasiswa list
7. Show with mahasiswa tab loads student list
8. Show with materi tab loads materi list
9. Show with tugas tab loads tugas list
10. Show with penilaian tab loads komponen and mahasiswa
11. Show detects nilai terkunci from periode yudisium

### Update Deskripsi (2 tests)

12. Dosen can update deskripsi kelas
13. Update deskripsi validates link grup wa as url

### Materi CRUD (6 tests)

14. Dosen can view create materi form
15. Dosen can store materi with files
16. Store materi validates required fields
17. Dosen can view edit materi form
18. Dosen can update materi
19. Dosen can delete materi

### Tugas CRUD (8 tests)

20. Dosen can view create tugas form
21. Dosen can store tugas with attachments
22. Store tugas validates required fields
23. Dosen can view edit tugas form
24. Dosen can update tugas
25. Dosen can delete tugas
26. Dosen can view tugas detail with submissions
27. Dosen can grade tugas submission

### Komponen Penilaian CRUD (6 tests)

28. Dosen can view create komponen form
29. Dosen can store komponen penilaian
30. Store komponen validates bobot total tidak lebih dari 100
31. Dosen can view edit komponen form
32. Dosen can update komponen penilaian
33. Dosen can delete komponen penilaian

### Nilai Mahasiswa (2 tests)

34. Dosen can batch update nilai mahasiswa
35. Cannot update nilai when periode yudisium locked

## Key Testing Patterns

### 1. Tab-Based Loading

```php
// Different data loaded based on ?tab= query parameter
$response = $this->get(route('siakad.dosen.mata-kuliah.show', [$jadwal->id, 'tab' => 'materi']));
$response->assertViewHas('materi');
$response->assertViewHas('activeTab', 'materi');
```

### 2. File Upload Testing

```php
Storage::fake('public');
$file = UploadedFile::fake()->create('materi.pdf', 1024); // 1MB PDF

$response = $this->post(route('...'), ['file' => $file, ...]);

Storage::disk('public')->assertExists('materi/' . $file->hashName());
```

### 3. Bobot Validation (Max 100%)

```php
// Existing komponen total: 60% (UTS 30% + UAS 30%)
// Try to add new komponen with 50% bobot
$totalBobot = KomponenPenilaian::where('mata_kuliah_id', $id)->sum('bobot'); // 60
if ($totalBobot + $request->bobot > 100) { // 60 + 50 = 110 > 100
    return back()->withErrors(['bobot' => 'Total bobot tidak boleh lebih dari 100%']);
}
```

### 4. Yudisium Lock Detection

```php
// Check if grades are locked during yudisium period
$locked = PeriodeYudisium::where('status', 'locked')
    ->where('tanggal_mulai', '<=', now())
    ->where('tanggal_selesai', '>=', now())
    ->exists();

if ($locked) {
    return back()->withErrors(['error' => 'Nilai sudah terkunci untuk periode yudisium']);
}
```

### 5. Batch Nilai Update

```php
// Update multiple students' grades at once
$data = [
    'nilai' => [
        ['mahasiswa_id' => 1, 'komponen_id' => 1, 'nilai' => 85],
        ['mahasiswa_id' => 1, 'komponen_id' => 2, 'nilai' => 90],
        ['mahasiswa_id' => 2, 'komponen_id' => 1, 'nilai' => 75],
    ]
];

foreach ($validated['nilai'] as $nilai) {
    NilaiMahasiswa::updateOrCreate(
        ['mahasiswa_id' => $nilai['mahasiswa_id'], 'komponen_id' => $nilai['komponen_id']],
        ['nilai' => $nilai['nilai']]
    );
}
```

## Validation Rules

### Materi

```php
'minggu' => 'required|integer|min:1|max:16',
'topik' => 'required|string|max:255',
'deskripsi' => 'nullable|string',
'file' => 'nullable|file|mimes:pdf,ppt,pptx,doc,docx|max:10240', // 10MB
'video_link' => 'nullable|url',
```

### Tugas

```php
'judul' => 'required|string|max:255',
'deskripsi' => 'required|string',
'deadline' => 'required|date|after:now',
'bobot' => 'required|integer|min:0|max:100',
'file' => 'nullable|file|max:10240',
```

### Komponen Penilaian

```php
'nama' => 'required|string|max:100', // UTS, UAS, Tugas, Partisipasi
'bobot' => 'required|integer|min:0|max:100',
'deskripsi' => 'nullable|string',
// Additional: Total all bobot ≤ 100%
```

### Batch Nilai

```php
'nilai' => 'required|array',
'nilai.*.mahasiswa_id' => 'required|exists:mahasiswa,id',
'nilai.*.komponen_id' => 'required|exists:komponen_penilaian,id',
'nilai.*.nilai' => 'required|numeric|min:0|max:100',
```

## Dependencies

**Models**: `JadwalPerkuliahan`, `FormasiDosen`, `MataKuliah`, `Dosen`, `Mahasiswa`, `Krs`, `MateriSap`, `Tugas`, `TugasSubmission`, `KomponenPenilaian`, `NilaiMahasiswa`, `PeriodeYudisium`, `ProgramStudi`, `Kurikulum`, `Ruangan`, `User`

**Factories**: All corresponding model factories

**Storage**: `Storage::fake('public')` for file upload testing

**Roles**: `dosen` (required)

## Running Tests

```bash
# All tests
php artisan test tests/Feature/Dosen/MataKuliahDetailTest.php

# Specific test groups
php artisan test --filter="materi"
php artisan test --filter="tugas"
php artisan test --filter="komponen"
php artisan test --filter="nilai"

# With coverage
php artisan test tests/Feature/Dosen/MataKuliahDetailTest.php --coverage
```

## Common Issues & Solutions

**Issue 1**: File upload fails in tests

- **Solution**: Use `Storage::fake('public')` before tests
- **Test**: #15 (store materi with files)

**Issue 2**: Total bobot exceeds 100%

- **Solution**: Controller validates sum before save
- **Test**: #30 (validates bobot total)

**Issue 3**: Cannot update nilai (locked)

- **Solution**: Check PeriodeYudisium lock status
- **Test**: #35 (cannot update when locked)

**Issue 4**: Wrong tab data loaded

- **Solution**: Check `?tab=` parameter in URL
- **Tests**: #6-10 (all tab tests)

## Conclusion

Largest Dosen module with 35 tests covering complete e-learning platform:

- ✅ Multi-tab course management
- ✅ File upload handling
- ✅ CRUD for 3 sub-features (Materi, Tugas, Komponen)
- ✅ Batch grade input with locking
- ✅ Bobot validation
- ✅ Complex relationship loading

**Status**: ✅ All 35 tests passing (100%)
