# Dokumentasi Testing SIAKAD STTW

> **Framework Testing**: Pest PHP v3.x  
> **Laravel Version**: 12  
> **Last Updated**: 9 Desember 2025

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
│       ├── 01_MASTER_DATA_PROGRAM_STUDI.md
│       ├── 02_MASTER_DATA_MATA_KULIAH.md
│       ├── 03_MASTER_DATA_RUANGAN.md
│       ├── 04_MASTER_DATA_DOSEN.md
│       ├── 05_MASTER_DATA_STAF.md
│       ├── 06_MASTER_DATA_MAHASISWA.md
│       ├── 07_STRUKTUR_KURIKULUM.md
│       ├── 08_FORMASI_DOSEN.md
│       ├── 09_KALENDER_PENDIDIKAN.md
│       ├── 10_JADWAL_PERKULIAHAN.md
│       ├── 11_JADWAL_UJIAN.md
│       ├── 12_ROMBONGAN_BELAJAR.md
│       ├── 13_PRESENSI_MAHASISWA.md
│       ├── 14_PRESENSI_DOSEN.md
│       ├── 15_MONITORING_KRS.md
│       ├── 16_MANAJEMEN_YUDISIUM.md
│       ├── 17_CETAK_DOKUMEN_AKADEMIK.md
│       └── 18_NEO_FEEDER_SYNC.md
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

### SIAKAD - Admin Role

#### Master Data (6 Modul)

| No  | Modul                   | Status    | Coverage | File Test |
| --- | ----------------------- | --------- | -------- | --------- |
| 1   | Program Studi           | ⚪ [TODO] | -        | -         |
| 2   | Mata Kuliah             | ⚪ [TODO] | -        | -         |
| 3   | Alokasi Ruang & Waktu   | ⚪ [TODO] | -        | -         |
| 4   | Data Dosen & Struktural | ⚪ [TODO] | -        | -         |
| 5   | Data Staf               | ⚪ [TODO] | -        | -         |
| 6   | Data Mahasiswa          | ⚪ [TODO] | -        | -         |

#### Manajemen Akademik (5 Modul)

| No  | Modul                       | Status    | Coverage | File Test |
| --- | --------------------------- | --------- | -------- | --------- |
| 7   | Struktur Kurikulum          | ⚪ [TODO] | -        | -         |
| 8   | Formasi Dosen               | ⚪ [TODO] | -        | -         |
| 9   | Kalender Pendidikan Dinamis | ⚪ [TODO] | -        | -         |
| 10  | Jadwal Perkuliahan          | ⚪ [TODO] | -        | -         |
| 11  | Jadwal Ujian                | ⚪ [TODO] | -        | -         |

#### Perkuliahan (3 Modul)

| No  | Modul              | Status    | Coverage | File Test |
| --- | ------------------ | --------- | -------- | --------- |
| 12  | Rombongan Belajar  | ⚪ [TODO] | -        | -         |
| 13  | Presensi Mahasiswa | ⚪ [TODO] | -        | -         |
| 14  | Presensi Dosen     | ⚪ [TODO] | -        | -         |

#### Manajemen KRS & Nilai (3 Modul)

| No  | Modul                  | Status    | Coverage | File Test |
| --- | ---------------------- | --------- | -------- | --------- |
| 15  | Monitoring KRS         | ⚪ [TODO] | -        | -         |
| 16  | Manajemen Yudisium     | ⚪ [TODO] | -        | -         |
| 17  | Cetak Dokumen Akademik | ⚪ [TODO] | -        | -         |

#### Pelaporan PDDikti (1 Modul)

| No  | Modul                   | Status    | Coverage | File Test |
| --- | ----------------------- | --------- | -------- | --------- |
| 18  | Sinkronisasi Neo Feeder | ⚪ [TODO] | -        | -         |

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
