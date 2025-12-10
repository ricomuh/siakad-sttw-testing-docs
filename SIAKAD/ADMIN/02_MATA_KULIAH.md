# Testing: Master Data Mata Kuliah

**Status**: ✅ [TESTED]  
**Role**: Admin  
**Route Prefix**: `/siakad/mata-kuliah`  
**Controller**: `MataKuliahController`  
**Model**: `MataKuliah`

---

## 📋 Daftar Testing

### 1. Functional Testing - CRUD Operations

#### 1.1 Index/List Mata Kuliah

- [ ] **Test**: Admin dapat melihat daftar mata kuliah

  - Verifikasi halaman index accessible
  - Verifikasi data mata kuliah ditampilkan
  - Verifikasi pagination (10 per halaman)
  - Verifikasi relasi dengan kurikulum ditampilkan

- [ ] **Test**: Filter mata kuliah berdasarkan program studi

  - Test dropdown filter program studi
  - Verifikasi hasil filter sesuai
  - Test reset filter

- [ ] **Test**: Filter mata kuliah berdasarkan semester

  - Filter semester 1-8
  - Verifikasi hasil akurat

- [ ] **Test**: Pencarian mata kuliah
  - Pencarian by kode mata kuliah
  - Pencarian by nama mata kuliah
  - Pencarian by dosen pengampu
  - Test kombinasi filter + search

#### 1.2 Create Mata Kuliah

- [ ] **Test**: Berhasil membuat mata kuliah dengan data lengkap

  - Isi semua field (kode, nama, sks, semester, dll)
  - Verifikasi tersimpan di database
  - Verifikasi redirect dengan flash message

- [ ] **Test**: Berhasil membuat dengan data minimal (required only)

  - Hanya isi field wajib
  - Field optional tetap null/default

- [ ] **Test**: Form create menampilkan dropdown dependencies
  - Dropdown kurikulum dari program studi
  - Dropdown program studi
  - Verifikasi data populated

#### 1.3 Show/Detail Mata Kuliah

- [ ] **Test**: Detail mata kuliah menampilkan informasi lengkap

  - Data mata kuliah
  - Relasi dengan kurikulum
  - Daftar dosen pengampu (jika ada)
  - Jadwal perkuliahan (jika ada)

- [ ] **Test**: Detail menampilkan silabus (jika ada)
  - RPS (Rencana Pembelajaran Semester)
  - Capaian Pembelajaran (CP)
  - Referensi

#### 1.4 Edit Mata Kuliah

- [ ] **Test**: Update mata kuliah berhasil

  - Edit nama, SKS, semester
  - Verifikasi data terupdate
  - Verifikasi flash message

- [ ] **Test**: Update dengan kode yang sama (data sendiri)
  - Tidak error unique validation

#### 1.5 Delete Mata Kuliah

- [ ] **Test**: Soft delete mata kuliah tanpa relasi

  - Verifikasi deleted_at terisi
  - Verifikasi data tidak muncul di index

- [ ] **Test**: Tidak dapat delete jika ada jadwal aktif

  - Mata kuliah yang sedang berjalan
  - Verifikasi error message

- [ ] **Test**: Tidak dapat delete jika ada mahasiswa terdaftar (KRS)
  - Verifikasi constraint check
  - Verifikasi error message user-friendly

---

### 2. Validation Testing

#### 2.1 Required Fields

- [ ] **Test**: Kode mata kuliah wajib diisi
- [ ] **Test**: Nama mata kuliah wajib diisi
- [ ] **Test**: SKS wajib diisi
- [ ] **Test**: Semester wajib diisi
- [ ] **Test**: Kurikulum ID wajib diisi

#### 2.2 Format & Range Validation

- [ ] **Test**: Kode mata kuliah unik per kurikulum

  - Same kode di kurikulum berbeda → OK
  - Same kode di kurikulum sama → Error

- [ ] **Test**: SKS harus angka positif (1-6)

  - Test dengan 0 → error
  - Test dengan 7 → error (melebihi max)
  - Test dengan decimal (2.5) → OK untuk SKS praktek

- [ ] **Test**: Semester harus 1-8

  - Test dengan 0 → error
  - Test dengan 9 → error

- [ ] **Test**: Maksimal karakter
  - Kode: max 10
  - Nama: max 255
  - Deskripsi: text (unlimited)

#### 2.3 Relational Validation

- [ ] **Test**: Kurikulum harus exist

  - Test dengan kurikulum_id invalid → error
  - Foreign key constraint validation

- [ ] **Test**: Mata kuliah prasyarat harus exist
  - Test dengan ID prasyarat yang tidak ada
  - Verifikasi error message

#### 2.4 Business Logic Validation

- [ ] **Test**: Mata kuliah tidak bisa jadi prasyarat diri sendiri

  - Set prasyarat = mata kuliah itu sendiri
  - Verifikasi error

- [ ] **Test**: SKS teori + praktek = total SKS
  - Test SKS calculation
  - Verifikasi consistency

---

### 3. Authorization Testing

#### 3.1 Role-Based Access

- [ ] **Test**: Admin full access
- [ ] **Test**: Akademik dapat view dan edit (tidak delete)
- [ ] **Test**: Dosen hanya view mata kuliah yang diampu
- [ ] **Test**: Mahasiswa tidak dapat akses master data

#### 3.2 Permission Testing

- [ ] **Test**: Permission `view-mata-kuliah`
- [ ] **Test**: Permission `create-mata-kuliah`
- [ ] **Test**: Permission `edit-mata-kuliah`
- [ ] **Test**: Permission `delete-mata-kuliah`

---

### 4. Responsive Testing

#### 4.1 Mobile Responsiveness

- [ ] **Test**: Tabel mata kuliah scroll horizontal di mobile
- [ ] **Test**: Form input responsive
- [ ] **Test**: Dropdown kurikulum dan prasyarat accessible

#### 4.2 Tablet & Desktop

- [ ] **Test**: Grid layout 2 kolom untuk form
- [ ] **Test**: Tabel full-width tanpa scroll

---

### 5. Integration Testing

#### 5.1 Relasi dengan Kurikulum

- [ ] **Test**: Dropdown kurikulum filtered by program studi

  - Pilih program studi → kurikulum ter-filter
  - AJAX/dynamic loading berfungsi

- [ ] **Test**: Mata kuliah tampil sesuai kurikulum
  - Group by semester
  - Order by urutan

#### 5.2 Relasi dengan Formasi Dosen

- [ ] **Test**: Assign dosen ke mata kuliah
  - Multiple dosen (team teaching)
  - Verifikasi relasi many-to-many

#### 5.3 Relasi dengan Jadwal Perkuliahan

- [ ] **Test**: Mata kuliah yang sudah dijadwalkan tidak bisa dihapus
  - Constraint check
  - Error handling

#### 5.4 Prasyarat Mata Kuliah

- [ ] **Test**: Set mata kuliah prasyarat

  - Single prasyarat
  - Multiple prasyarat (AND logic)
  - Verifikasi chain validation (A → B → C)

- [ ] **Test**: Circular dependency detection
  - A prasyarat B, B prasyarat A → Error
  - Verifikasi anti-circular logic

---

### 6. UI/UX Testing

#### 6.1 Dynamic Dropdowns

- [ ] **Test**: Dropdown kurikulum updates saat pilih program studi
  - AJAX request berfungsi
  - Loading state ditampilkan
  - Options ter-populate

#### 6.2 Autocomplete Search

- [ ] **Test**: Search mata kuliah prasyarat dengan autocomplete
  - Type minimal 3 karakter
  - Suggestions muncul
  - Select from list berfungsi

#### 6.3 Modal/Dialog

- [ ] **Test**: Modal detail silabus
  - Buka modal
  - Konten loaded
  - Close berfungsi

#### 6.4 Flash Messages

- [ ] **Test**: Success create
- [ ] **Test**: Success update
- [ ] **Test**: Success delete
- [ ] **Test**: Error messages dengan detail

---

### 7. Error Handling

#### 7.1 Validation Errors

- [ ] **Test**: Multiple validation errors ditampilkan semua
  - Error summary di top
  - Inline error per field

#### 7.2 Database Errors

- [ ] **Test**: Duplicate kode mata kuliah
  - User-friendly message
  - Tidak expose technical error

#### 7.3 Relationship Errors

- [ ] **Test**: Delete cascade handling
  - Error jika ada dependent records

---

### 8. Performance Testing

#### 8.1 Query Optimization

- [ ] **Test**: Eager loading untuk relasi

  - with(['kurikulum', 'prasyarat'])
  - Verifikasi tidak ada N+1

- [ ] **Test**: Index dengan 1000+ mata kuliah
  - Page load < 2s
  - Pagination efficient

#### 8.2 Search Performance

- [ ] **Test**: Search dengan large dataset
  - Full-text search performance
  - Index optimization

---

### 9. Edge Cases

- [ ] **Test**: Mata kuliah dengan nama sangat panjang (255 char)
- [ ] **Test**: Kode dengan karakter spesial
- [ ] **Test**: SKS dengan nilai desimal (2.5, 3.5)
- [ ] **Test**: Mata kuliah tanpa deskripsi (null)
- [ ] **Test**: Mata kuliah lintas semester (semester genap/ganjil)
- [ ] **Test**: Update semester mata kuliah yang sudah ada KRS
  - Impact analysis
  - Confirmation dialog

---

## 📊 Test Coverage Target

- **Minimum Coverage**: 85%
- **Critical Paths**: 100% (Create, Edit, Delete with constraints)

## 🧪 Sample Test Code

```php
<?php

use App\Models\MataKuliah;
use App\Models\Kurikulum;
use App\Models\User;

beforeEach(function () {
    $this->admin = User::factory()->create();
    $this->admin->assignRole('admin');
});

describe('Mata Kuliah - CRUD', function () {
    it('can create mata kuliah with valid data', function () {
        $kurikulum = Kurikulum::factory()->create();

        $data = [
            'kode' => 'IF101',
            'nama' => 'Algoritma dan Pemrograman',
            'sks_teori' => 2,
            'sks_praktek' => 2,
            'semester' => 1,
            'kurikulum_id' => $kurikulum->id,
        ];

        $response = actingAs($this->admin)
            ->post(route('siakad.mata-kuliah.store'), $data);

        $response->assertRedirect();
        assertDatabaseHas('mata_kuliah', ['kode' => 'IF101']);
    });

    it('validates SKS range', function () {
        $kurikulum = Kurikulum::factory()->create();

        $response = actingAs($this->admin)
            ->post(route('siakad.mata-kuliah.store'), [
                'kode' => 'IF101',
                'nama' => 'Test',
                'sks_teori' => 7, // Melebihi max
                'semester' => 1,
                'kurikulum_id' => $kurikulum->id,
            ]);

        $response->assertInvalid(['sks_teori']);
    });
});

describe('Mata Kuliah - Prasyarat', function () {
    it('can set prerequisite mata kuliah', function () {
        $mataKuliah = MataKuliah::factory()->create(['semester' => 2]);
        $prasyarat = MataKuliah::factory()->create(['semester' => 1]);

        $response = actingAs($this->admin)
            ->put(route('siakad.mata-kuliah.update', $mataKuliah), [
                'prasyarat' => [$prasyarat->id],
            ]);

        expect($mataKuliah->fresh()->prasyarat)->toHaveCount(1);
    });

    it('prevents circular dependency', function () {
        $mkA = MataKuliah::factory()->create();
        $mkB = MataKuliah::factory()->create(['prasyarat' => [$mkA->id]]);

        // Coba set A prasyarat B (circular)
        $response = actingAs($this->admin)
            ->put(route('siakad.mata-kuliah.update', $mkA), [
                'prasyarat' => [$mkB->id],
            ]);

        $response->assertInvalid();
    });
});

describe('Mata Kuliah - Integration', function () {
    it('cannot delete if has active schedule', function () {
        $mataKuliah = MataKuliah::factory()
            ->has(JadwalPerkuliahan::factory()->active())
            ->create();

        $response = actingAs($this->admin)
            ->delete(route('siakad.mata-kuliah.destroy', $mataKuliah));

        $response->assertSessionHasErrors();
        assertDatabaseHas('mata_kuliah', ['id' => $mataKuliah->id]);
    });
});
```

---

## 🔗 Related Modules

- [Program Studi](01_MASTER_DATA_PROGRAM_STUDI.md)
- [Struktur Kurikulum](07_STRUKTUR_KURIKULUM.md)
- [Formasi Dosen](08_FORMASI_DOSEN.md)
- [Jadwal Perkuliahan](10_JADWAL_PERKULIAHAN.md)
