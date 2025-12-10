# Dokumentasi Testing SIAKAD STTW

> **Framework Testing**: Pest PHP v3.x  
> **Laravel Version**: 12  
> **Last Updated**: 10 Desember 2025  
> **Test Progress**: 13/15 Admin modules tested (86.7%)  
> **Total Tests**: 425 passing | 1017 assertions

## 📊 Quick Stats

-   ✅ **Master Data**: 6/6 modules tested (122 tests)
-   ✅ **Manajemen Akademik**: 5/5 modules tested (123 tests)
-   ✅ **Perkuliahan**: 2/3 modules tested (36 tests)
-   🟡 **KRS & Nilai**: 1/2 modules tested (22 tests)
-   ⚪ **PDDikti**: 0/1 modules tested
-   **Staf Module**: Controller belum dibuat

## 📋 Daftar Isi

-   [Overview](#overview)
-   [Struktur Dokumentasi](#struktur-dokumentasi)
-   [Scope Testing Pest PHP](#scope-testing-pest-php)
-   [Cara Menjalankan Testing](#cara-menjalankan-testing)
-   [Progress Testing](#progress-testing)

## Overview

Dokumentasi ini berisi persiapan dan rencana testing komprehensif untuk sistem SIAKAD STTW menggunakan Pest PHP. Setiap modul memiliki dokumentasi terpisah yang mencakup:

-   ✅ **Functional Testing** - Testing fungsionalitas CRUD dan business logic
-   ✅ **Validation Testing** - Testing validasi input dan form
-   ✅ **Authorization Testing** - Testing role-based access control
-   ✅ **Responsive Testing** - Testing tampilan di berbagai ukuran layar
-   ✅ **Integration Testing** - Testing integrasi antar modul
-   ✅ **Error Handling** - Testing penanganan error dan edge cases

## Struktur Dokumentasi

```
TESTING_DOCS/
├── README.md (file ini)
├── SIAKAD/
│   └── ADMIN/
│       ├── 01_MASTER_DATA_PROGRAM_STUDI.md     ✅ TESTED (24 tests)
│       ├── 02_MATA_KULIAH.md                   ✅ TESTED (23 tests)
│       ├── 03_DOSEN.md                         ✅ TESTED (26 tests)
│       ├── 04_RUANGAN.md                       ✅ TESTED (20 tests)
│       ├── 05_STAF.md                          ⚪ TODO
│       ├── 06_MAHASISWA.md                     ✅ TESTED (29 tests)
│       ├── 07_KURIKULUM.md                     ✅ TESTED (26 tests)
│       ├── 08_FORMASI_DOSEN.md                 ✅ TESTED (26 tests)
│       ├── 09_KALENDER_PENDIDIKAN.md           ✅ TESTED (22 tests)
│       ├── 10_JADWAL_PERKULIAHAN.md            ✅ TESTED (27 tests)
│       ├── 11_JADWAL_UJIAN.md                  ✅ TESTED (22 tests)
│       ├── 12_ROMBONGAN_BELAJAR.md             ✅ TESTED (18 tests)
│       ├── 13_PRESENSI_MAHASISWA.md            ⚪ TODO (Sudah ada di Dosen)
│       ├── 14_PRESENSI_DOSEN.md                ✅ TESTED (18 tests)
│       ├── 15_MONITORING_KRS.md                ⚪ TODO
│       ├── 16_MANAJEMEN_YUDISIUM.md            ✅ TESTED (22 tests)
│       └── 17_NEO_FEEDER_SYNC.md               ⚪ TODO
```

## Scope Testing Pest PHP

Pest PHP v3.x mendukung berbagai jenis testing:

### 1. **Feature Testing**

Testing end-to-end untuk fitur lengkap dengan real database dan HTTP requests.

```php
it('can create program studi', function () {
    $admin = createAdminUser();

    $response = actingAs($admin)->post(route('siakad.program-studi.store'), [
        'kode' => 'IF',
        'nama_program' => 'Teknik Informatika',
    ]);

    $response->assertRedirect();
    assertDatabaseHas('program_studi', ['kode' => 'IF']);
});
```

### 2. **Unit Testing**

Testing komponen terisolasi seperti model methods, helpers, atau business logic.

```php
it('calculates ipk correctly', function () {
    $mahasiswa = Mahasiswa::factory()->create();
    // ... setup nilai

    expect($mahasiswa->ipk)->toBe(3.5);
});
```

### 3. **Browser Testing** (Dusk/Pest Plugin)

Testing interaksi user di browser untuk fitur kompleks seperti drag-drop, AJAX, dll.

### 4. **Parallel Testing**

Menjalankan test secara paralel untuk mempercepat eksekusi.

```bash
php artisan test --parallel
```

### 5. **Coverage Testing**

Mengukur code coverage untuk memastikan semua kode ter-test.

```bash
php artisan test --coverage
```

### 6. **Snapshot Testing**

Membandingkan output dengan snapshot sebelumnya.

```php
it('renders correct html', function () {
    $html = view('siakad.program-studi.index')->render();
    expect($html)->toMatchSnapshot();
});
```

## Cara Menjalankan Testing

### Menjalankan Semua Test

```bash
php artisan test
```

### Menjalankan Test Spesifik File

```bash
php artisan test tests/Feature/ProgramStudiTest.php
```

### Menjalankan Test dengan Filter

```bash
php artisan test --filter=program_studi
```

### Menjalankan Test dengan Coverage

```bash
php artisan test --coverage --min=80
```

### Menjalankan Test Paralel

```bash
php artisan test --parallel --processes=4
```

## Progress Testing

### Status Legend

-   ✅ **[TESTED]** - Test sudah dibuat dan passing
-   🟡 **[IN-PROGRESS]** - Test sedang dalam pengerjaan
-   ⚪ **[TODO]** - Test belum dibuat

### SIAKAD - Admin Role (13/15 Modul Tested - 86.7%)

#### Master Data (6/6 Modul) - ✅ COMPLETE

| No  | Modul         | Status      | Tests | File Test                                  |
| --- | ------------- | ----------- | ----- | ------------------------------------------ |
| 1   | Program Studi | ✅ [TESTED] | 24/24 | `tests/Feature/Admin/ProgramStudiTest.php` |
| 2   | Mata Kuliah   | ✅ [TESTED] | 23/23 | `tests/Feature/Admin/MataKuliahTest.php`   |
| 3   | Ruangan       | ✅ [TESTED] | 20/20 | `tests/Feature/Admin/RuanganTest.php`      |
| 4   | Dosen         | ✅ [TESTED] | 26/26 | `tests/Feature/Admin/DosenTest.php`        |
| 5   | Staf          | ⚪ [TODO]   | -     | -                                          |
| 6   | Mahasiswa     | ✅ [TESTED] | 29/29 | `tests/Feature/Admin/MahasiswaTest.php`    |

#### Manajemen Akademik (5/5 Modul) - ✅ COMPLETE

| No  | Modul              | Status      | Tests | File Test                                        |
| --- | ------------------ | ----------- | ----- | ------------------------------------------------ |
| 7   | Kurikulum          | ✅ [TESTED] | 26/26 | `tests/Feature/Admin/KurikulumTest.php`          |
| 8   | Formasi Dosen      | ✅ [TESTED] | 26/26 | `tests/Feature/Admin/FormasiDosenTest.php`       |
| 9   | Kalender Akademik  | ✅ [TESTED] | 22/22 | `tests/Feature/Admin/KalenderPendidikanTest.php` |
| 10  | Jadwal Perkuliahan | ✅ [TESTED] | 27/27 | `tests/Feature/Admin/JadwalPerkuliahanTest.php`  |
| 11  | Jadwal Ujian       | ✅ [TESTED] | 22/22 | `tests/Feature/Admin/JadwalUjianTest.php`        |

#### Perkuliahan (3/3 Modul) - ✅ COMPLETE

| No  | Modul              | Status      | Tests | File Test                                      |
| --- | ------------------ | ----------- | ----- | ---------------------------------------------- |
| 12  | Rombongan Belajar  | ✅ [TESTED] | 18/18 | `tests/Feature/Admin/RombonganBelajarTest.php` |
| 13  | Presensi Mahasiswa | ⚪ [TODO]   | -     | _Sudah ada di Dosen folder_                    |
| 14  | Presensi Dosen     | ✅ [TESTED] | 18/18 | `tests/Feature/Admin/PresensiDosenTest.php`    |

#### Manajemen KRS & Nilai (1/2 Modul) - 🟡 IN PROGRESS

| No  | Modul              | Status      | Tests | File Test                                       |
| --- | ------------------ | ----------- | ----- | ----------------------------------------------- |
| 15  | Monitoring KRS     | ⚪ [TODO]   | -     | _Controller: 227 lines_                         |
| 16  | Manajemen Yudisium | ✅ [TESTED] | 22/22 | `tests/Feature/Admin/ManajemenYudisiumTest.php` |

#### Pelaporan PDDikti (0/1 Modul)

| No  | Modul                   | Status    | Tests | File Test               |
| --- | ----------------------- | --------- | ----- | ----------------------- |
| 17  | Sinkronisasi Neo Feeder | ⚪ [TODO] | -     | _Controller: 284 lines_ |

**Total Progress**: 13/15 controllers tested (86.7%) | **425 tests passing**

---

## 🎯 Next Steps

Modul yang masih perlu testing:

1. **Monitoring KRS** (MonitoringKrsController - 227 lines)

    - Route: `/siakad/monitoring-krs`
    - Fitur: Monitoring status KRS mahasiswa, approval, laporan

2. **Sinkronisasi Neo Feeder** (NeoFeederSyncController - 284 lines)
    - Route: `/siakad/neo-feeder-sync`
    - Fitur: Sinkronisasi data dengan PDDikti via Neo Feeder API

**Note**: Staf module tidak perlu testing karena controller belum dibuat di codebase.

---

## Kontribusi

Setiap modul testing harus mengikuti standar:

1. Mencakup semua aspek yang didokumentasikan
2. Menggunakan Pest syntax yang benar
3. Mengikuti Laravel best practices
4. Code coverage minimal 80%
5. Semua test harus passing sebelum commit

## Resources

-   [Pest PHP Documentation](https://pestphp.com/docs)
-   [Laravel Testing Documentation](https://laravel.com/docs/testing)
-   [SIAKAD Developer Guide](../DEVELOPER_GUIDE.md)
-   [SIAKAD Components Guide](../COMPONENTS.md)
