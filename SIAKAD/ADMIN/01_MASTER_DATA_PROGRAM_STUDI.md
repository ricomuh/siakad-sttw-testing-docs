# Testing: Master Data Program Studi

**Status**: ⚪ [TODO]  
**Role**: Admin  
**Route Prefix**: `/siakad/program-studi`  
**Controller**: `ProgramStudiController`  
**Model**: `ProgramStudi`

---

## 📋 Daftar Testing

### 1. Functional Testing - CRUD Operations

#### 1.1 Index/List Program Studi

-   [ ] **Test**: Admin dapat melihat daftar program studi

    -   Verifikasi halaman index dapat diakses
    -   Verifikasi data program studi ditampilkan dalam tabel
    -   Verifikasi pagination berfungsi
    -   Verifikasi jumlah data per halaman (default 10)

-   [ ] **Test**: Daftar program studi kosong menampilkan pesan yang sesuai

    -   Verifikasi pesan "Tidak ada data" muncul
    -   Verifikasi tidak ada error saat data kosong

-   [ ] **Test**: Filter dan pencarian berfungsi
    -   Test pencarian berdasarkan kode program
    -   Test pencarian berdasarkan nama program
    -   Test filter berdasarkan jenjang (S1, D3, S2, dst)
    -   Test kombinasi filter dan pencarian

#### 1.2 Create Program Studi

-   [ ] **Test**: Admin dapat mengakses halaman create

    -   Verifikasi route `program-studi.create` accessible
    -   Verifikasi form ditampilkan dengan lengkap
    -   Verifikasi semua input field tersedia

-   [ ] **Test**: Berhasil membuat program studi dengan data valid

    -   Submit form dengan data lengkap dan valid
    -   Verifikasi redirect ke halaman show/index
    -   Verifikasi data tersimpan di database
    -   Verifikasi flash message sukses muncul

-   [ ] **Test**: Dapat membuat program studi dengan data minimal
    -   Hanya isi field yang required
    -   Verifikasi tetap berhasil disimpan

#### 1.3 Show/Detail Program Studi

-   [ ] **Test**: Admin dapat melihat detail program studi

    -   Akses halaman detail dengan ID valid
    -   Verifikasi semua data ditampilkan
    -   Verifikasi relasi ditampilkan (jumlah mahasiswa, dosen, dll)

-   [ ] **Test**: Detail program studi tidak ditemukan
    -   Akses dengan ID yang tidak ada
    -   Verifikasi error 404
    -   Verifikasi pesan error yang sesuai

#### 1.4 Edit Program Studi

-   [ ] **Test**: Admin dapat mengakses halaman edit

    -   Verifikasi form edit ditampilkan
    -   Verifikasi data existing terisi di form

-   [ ] **Test**: Berhasil update program studi

    -   Update data dengan nilai baru
    -   Verifikasi redirect setelah update
    -   Verifikasi data berubah di database
    -   Verifikasi flash message sukses

-   [ ] **Test**: Update dengan data yang sama tidak error
    -   Submit tanpa mengubah data
    -   Verifikasi tidak ada error

#### 1.5 Delete Program Studi

-   [ ] **Test**: Berhasil soft delete program studi

    -   Delete program studi yang tidak memiliki relasi
    -   Verifikasi soft delete (deleted_at terisi)
    -   Verifikasi redirect ke index
    -   Verifikasi flash message sukses

-   [ ] **Test**: Tidak dapat delete program studi yang memiliki relasi
    -   Program studi yang sudah memiliki mahasiswa
    -   Program studi yang sudah memiliki mata kuliah
    -   Verifikasi error message muncul
    -   Verifikasi data tidak terhapus

---

### 2. Validation Testing

#### 2.1 Required Fields

-   [ ] **Test**: Kode program wajib diisi

    -   Submit form tanpa kode
    -   Verifikasi error message: "Kode program harus diisi"

-   [ ] **Test**: Nama program wajib diisi

    -   Submit form tanpa nama
    -   Verifikasi error message yang sesuai

-   [ ] **Test**: Jenjang pendidikan wajib diisi
    -   Submit form tanpa jenjang
    -   Verifikasi error validation

#### 2.2 Format Validation

-   [ ] **Test**: Kode program harus unik

    -   Coba buat dengan kode yang sudah ada
    -   Verifikasi error: "Kode sudah digunakan"

-   [ ] **Test**: Kode program format yang benar

    -   Test dengan karakter spesial (@#$%)
    -   Test dengan angka saja
    -   Test dengan huruf dan angka (valid)
    -   Verifikasi format yang diterima

-   [ ] **Test**: Maksimal karakter untuk setiap field
    -   Kode: maks 10 karakter
    -   Nama: maks 255 karakter
    -   Test input melebihi batas
    -   Verifikasi error message

#### 2.3 Enum/Select Validation

-   [ ] **Test**: Jenjang harus dari pilihan yang valid

    -   Test dengan nilai di luar enum (S3, Profesi, dst)
    -   Verifikasi hanya nilai valid diterima: S1, D3, S2

-   [ ] **Test**: Status program valid
    -   Test enum: Aktif, Non-Aktif
    -   Verifikasi validasi berfungsi

#### 2.4 Unique Validation

-   [ ] **Test**: Kode program unik saat create

    -   Verifikasi tidak boleh duplikat

-   [ ] **Test**: Kode program unik saat edit (kecuali data sendiri)
    -   Edit tanpa ubah kode → valid
    -   Edit dengan kode program lain yang sudah ada → error

---

### 3. Authorization Testing

#### 3.1 Role-Based Access

-   [ ] **Test**: Admin dapat mengakses semua fitur

    -   Index, create, show, edit, delete
    -   Verifikasi semua route accessible

-   [ ] **Test**: Non-admin tidak dapat mengakses

    -   Test dengan role: dosen
    -   Test dengan role: mahasiswa
    -   Test dengan role: akademik
    -   Verifikasi redirect atau 403 Forbidden

-   [ ] **Test**: Unauthenticated user redirect ke login
    -   Akses tanpa login
    -   Verifikasi redirect ke `/login`

#### 3.2 Permission-Based Access

-   [ ] **Test**: User dengan permission `view-program-studi` dapat index

    -   Verifikasi permission system berfungsi

-   [ ] **Test**: User tanpa permission tidak dapat akses
    -   Verifikasi 403 atau redirect

---

### 4. Responsive Testing

#### 4.1 Mobile View (< 640px)

-   [ ] **Test**: Tabel program studi responsive di mobile

    -   Verifikasi scroll horizontal atau card view
    -   Verifikasi tombol aksi tetap accessible

-   [ ] **Test**: Form create/edit responsive di mobile
    -   Input fields full-width
    -   Button tidak terpotong
    -   Label dan error message terbaca

#### 4.2 Tablet View (768px - 1024px)

-   [ ] **Test**: Layout 2 kolom form berfungsi
    -   Verifikasi spacing yang baik

#### 4.3 Desktop View (> 1024px)

-   [ ] **Test**: Tabel ditampilkan dengan optimal
    -   Semua kolom visible tanpa scroll

---

### 5. Integration Testing

#### 5.1 Relasi dengan Modul Lain

-   [ ] **Test**: Program studi terhubung dengan mahasiswa

    -   Saat delete program studi, cek impact ke mahasiswa
    -   Verifikasi constraint database

-   [ ] **Test**: Program studi terhubung dengan kurikulum

    -   Verifikasi relasi hasMany berfungsi
    -   Eager loading bekerja dengan baik

-   [ ] **Test**: Program studi terhubung dengan dosen
    -   Verifikasi dosen dapat di-assign ke program studi

#### 5.2 Export/Import

-   [ ] **Test**: Export data program studi ke Excel

    -   Verifikasi file terdownload
    -   Verifikasi format Excel benar
    -   Verifikasi data lengkap

-   [ ] **Test**: Import data program studi dari Excel
    -   Upload file template yang valid
    -   Verifikasi data terimport dengan benar
    -   Test dengan file corrupt → error handling

---

### 6. UI/UX Testing

#### 6.1 Loading States

-   [ ] **Test**: Loading indicator muncul saat submit form
    -   Verifikasi button disabled saat loading
    -   Verifikasi spinner/loading text

#### 6.2 Flash Messages

-   [ ] **Test**: Success message muncul setelah create

    -   Verifikasi warna hijau (success)
    -   Verifikasi message: "Program studi berhasil ditambahkan"

-   [ ] **Test**: Success message muncul setelah update

    -   Message: "Program studi berhasil diperbarui"

-   [ ] **Test**: Success message muncul setelah delete

    -   Message: "Program studi berhasil dihapus"

-   [ ] **Test**: Error message muncul saat validasi gagal
    -   Verifikasi warna merah (error)
    -   Verifikasi list error ditampilkan

#### 6.3 Confirmation Dialogs

-   [ ] **Test**: Konfirmasi muncul sebelum delete
    -   Verifikasi modal/alert konfirmasi
    -   Test cancel → data tidak terhapus
    -   Test confirm → data terhapus

---

### 7. Error Handling Testing

#### 7.1 Database Errors

-   [ ] **Test**: Handle constraint violation
    -   Foreign key constraint
    -   Unique constraint
    -   Verifikasi user-friendly error message

#### 7.2 Network Errors

-   [ ] **Test**: Handle timeout saat submit form
    -   Simulate slow network
    -   Verifikasi retry mechanism atau error message

#### 7.3 File Upload Errors

-   [ ] **Test**: Handle file size melebihi limit
    -   Upload file > max size
    -   Verifikasi error message

---

### 8. Performance Testing

#### 8.1 Query Optimization

-   [ ] **Test**: N+1 query problem tidak terjadi

    -   Gunakan Laravel Debugbar
    -   Verifikasi jumlah query optimal dengan eager loading

-   [ ] **Test**: Pagination performance dengan data banyak
    -   Seed 10,000 data
    -   Verifikasi halaman tetap load cepat (< 2s)

#### 8.2 Caching

-   [ ] **Test**: Cache berfungsi untuk data statis
    -   Verifikasi query cache
    -   Verifikasi cache invalidation saat update

---

### 9. Edge Cases Testing

-   [ ] **Test**: Concurrent edit oleh 2 user

    -   User A dan B edit data yang sama
    -   Verifikasi data consistency

-   [ ] **Test**: Program studi dengan nama sangat panjang

    -   Test dengan 255 karakter
    -   Verifikasi tampilan tidak rusak

-   [ ] **Test**: Karakter special dalam nama

    -   Test dengan karakter unicode (中文, العربية)
    -   Test dengan emoji
    -   Verifikasi tersimpan dan ditampilkan dengan benar

-   [ ] **Test**: Bulk operations
    -   Delete multiple program studi sekaligus
    -   Update multiple records

---

## 📊 Test Coverage Target

-   **Minimum Coverage**: 80%
-   **Recommended Coverage**: 95%

## 🧪 Sample Test Code

```php
<?php

use App\Models\ProgramStudi;
use App\Models\User;
use function Pest\Laravel\{actingAs, assertDatabaseHas, get, post, put, delete};

beforeEach(function () {
    $this->admin = User::factory()->create();
    $this->admin->assignRole('admin');
});

describe('Program Studi - Index', function () {
    it('can view program studi list', function () {
        ProgramStudi::factory()->count(5)->create();

        $response = actingAs($this->admin)
            ->get(route('siakad.program-studi.index'));

        $response->assertOk()
            ->assertViewHas('programStudi')
            ->assertSee('Program Studi');
    });

    it('shows empty message when no data', function () {
        $response = actingAs($this->admin)
            ->get(route('siakad.program-studi.index'));

        $response->assertSee('Tidak ada data');
    });
});

describe('Program Studi - Create', function () {
    it('can create program studi with valid data', function () {
        $data = [
            'kode' => 'IF',
            'nama_program' => 'Teknik Informatika',
            'jenjang' => 'S1',
            'akreditasi' => 'A',
            'status' => 'Aktif',
        ];

        $response = actingAs($this->admin)
            ->post(route('siakad.program-studi.store'), $data);

        $response->assertRedirect();
        assertDatabaseHas('program_studi', ['kode' => 'IF']);
    });

    it('validates required fields', function () {
        $response = actingAs($this->admin)
            ->post(route('siakad.program-studi.store'), []);

        $response->assertInvalid(['kode', 'nama_program', 'jenjang']);
    });

    it('validates unique kode', function () {
        ProgramStudi::factory()->create(['kode' => 'IF']);

        $response = actingAs($this->admin)
            ->post(route('siakad.program-studi.store'), [
                'kode' => 'IF',
                'nama_program' => 'Test',
                'jenjang' => 'S1',
            ]);

        $response->assertInvalid(['kode']);
    });
});

describe('Program Studi - Authorization', function () {
    it('denies access for non-admin users', function () {
        $dosen = User::factory()->create();
        $dosen->assignRole('dosen');

        $response = actingAs($dosen)
            ->get(route('siakad.program-studi.index'));

        $response->assertForbidden();
    });

    it('redirects to login for guest', function () {
        $response = get(route('siakad.program-studi.index'));

        $response->assertRedirect(route('login'));
    });
});
```

---

## 📝 Notes

-   Gunakan factories untuk setup data test
-   Gunakan `RefreshDatabase` trait untuk isolasi test
-   Pastikan semua test independent (tidak bergantung pada test lain)
-   Gunakan descriptive test names
-   Group related tests menggunakan `describe()`

## 🔗 Related Modules

-   [Mata Kuliah](02_MASTER_DATA_MATA_KULIAH.md)
-   [Kurikulum](07_STRUKTUR_KURIKULUM.md)
-   [Mahasiswa](06_MASTER_DATA_MAHASISWA.md)
