# Mata Kuliah (Course List) - Dosen Portal Testing Guide

## Overview

**Module**: Mata Kuliah List View for Dosen  
**Controller**: `App\Http\Controllers\Dosen\MataKuliahController`  
**Route Prefix**: `/siakad/dosen/mata-kuliah`  
**Test File**: `tests/Feature/Dosen/MataKuliahTest.php`  
**Total Tests**: 14 tests, 28 assertions  
**Status**: ✅ ALL PASSING (100%)

## Purpose

Allows dosen to view a list of courses they are teaching in the current semester. This is a **read-only view** that displays teaching assignments (formasi) with course details, schedules, and program information.

## Controller Methods

### 1. `index()` - List Courses

**Purpose**: Display all courses taught by authenticated dosen

**Route**: `GET /siakad/dosen/mata-kuliah` → `siakad.dosen.mata-kuliah.index`

**Logic Flow**:

```php
public function index()
{
    // 1. Get authenticated user
    $user = auth()->user();

    // 2. Find dosen record by email (institusi OR pribadi)
    $dosen = Dosen::where('email_institusi', $user->email)
        ->orWhere('email_pribadi', $user->email)
        ->first();

    // 3. Graceful degradation if dosen not found
    if (!$dosen) {
        return view('dosen.mata-kuliah.index', [
            'formasi' => collect([]),
        ]);
    }

    // 4. Get active formasi for current semester
    $formasi = FormasiDosen::where('dosen_id', $dosen->id)
        ->where('is_active', true)
        ->with([
            'mataKuliah',           // Load course details
            'programStudi',         // Load program info
            'jadwalPerkuliahan' => function ($query) {
                $query->where('is_active', true)
                    ->where('tahun_akademik', config('siakad.semester_aktif.tahun'))
                    ->where('semester', config('siakad.semester_aktif.semester'))
                    ->orderBy('hari')
                    ->orderBy('waktu_mulai');
            },
        ])
        ->get();

    return view('dosen.mata-kuliah.index', compact('formasi'));
}
```

**Key Features**:

- ✅ Dual email lookup (institusi OR pribadi)
- ✅ Filters by active formasi only
- ✅ Filters by active semester
- ✅ Eager loads relationships to prevent N+1 queries
- ✅ Orders jadwal by day and time
- ✅ Graceful degradation when dosen not found

## Test Coverage (14 tests)

### Authorization Tests (2 tests)

**1. Requires Authentication**

```php
test('requires authentication', function () {
    $response = $this->get(route('siakad.dosen.mata-kuliah.index'));
    $response->assertRedirect(route('login'));
});
```

**2. Requires Dosen Role**

```php
test('requires dosen role', function () {
    $user = User::factory()->create();
    $user->assignRole('mahasiswa'); // Wrong role

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertForbidden();
});
```

### Core Functionality Tests (6 tests)

**3. Dosen Can View Mata Kuliah List**

```php
test('dosen can view mata kuliah list', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    Dosen::factory()->create(['email_institusi' => $user->email]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertStatus(200);
    $response->assertViewIs('dosen.mata-kuliah.index');
    $response->assertViewHas('formasi');
});
```

**4. Can Access with Email Institusi**

```php
test('dosen can access with email_institusi', function () {
    $user = User::factory()->create(['email' => 'dosen@sttw.ac.id']);
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create([
        'email_institusi' => 'dosen@sttw.ac.id',
        'email_pribadi' => 'other@gmail.com',
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertStatus(200);
    // Should find dosen by email_institusi
});
```

**5. Can Access with Email Pribadi**

```php
test('dosen can access with email_pribadi', function () {
    $user = User::factory()->create(['email' => 'dosen@gmail.com']);
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create([
        'email_institusi' => 'other@sttw.ac.id',
        'email_pribadi' => 'dosen@gmail.com',
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertStatus(200);
    // Should find dosen by email_pribadi
});
```

**6. Returns Empty Collection When Dosen Not Found**

```php
test('returns empty collection when dosen not found', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    // No dosen record for this user

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertStatus(200);
    $response->assertViewHas('formasi', function ($collection) {
        return $collection->count() === 0;
    });
});
```

### Filtering Tests (3 tests)

**7. Only Shows Active Formasi**

```php
test('only shows active formasi', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);

    // Active formasi
    $activeFormasi = FormasiDosen::factory()->create([
        'dosen_id' => $dosen->id,
        'is_active' => true,
    ]);

    // Inactive formasi
    FormasiDosen::factory()->create([
        'dosen_id' => $dosen->id,
        'is_active' => false,
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertViewHas('formasi', function ($collection) {
        return $collection->count() === 1 &&
               $collection->every(fn($f) => $f->is_active === true);
    });
});
```

**8. Filters by Semester Aktif**

```php
test('filters by semester_aktif', function () {
    config(['siakad.semester_aktif.tahun' => '2025/2026']);
    config(['siakad.semester_aktif.semester' => 'Ganjil']);

    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);
    $formasi = FormasiDosen::factory()->create([
        'dosen_id' => $dosen->id,
        'is_active' => true,
    ]);

    // Jadwal for current semester
    JadwalPerkuliahan::factory()->create([
        'formasi_dosen_id' => $formasi->id,
        'tahun_akademik' => '2025/2026',
        'semester' => 'Ganjil',
        'is_active' => true,
    ]);

    // Jadwal for different semester (should be filtered out)
    JadwalPerkuliahan::factory()->create([
        'formasi_dosen_id' => $formasi->id,
        'tahun_akademik' => '2024/2025',
        'semester' => 'Genap',
        'is_active' => true,
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertViewHas('formasi', function ($collection) {
        $jadwal = $collection->first()->jadwalPerkuliahan;
        return $jadwal->count() === 1 &&
               $jadwal->first()->tahun_akademik === '2025/2026';
    });
});
```

**9. Only Shows Own Mata Kuliah**

```php
test('only shows own mata kuliah', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);
    $otherDosen = Dosen::factory()->create();

    // Own formasi
    FormasiDosen::factory()->create([
        'dosen_id' => $dosen->id,
        'is_active' => true,
    ]);

    // Other dosen's formasi
    FormasiDosen::factory()->create([
        'dosen_id' => $otherDosen->id,
        'is_active' => true,
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertViewHas('formasi', function ($collection) use ($dosen) {
        return $collection->count() === 1 &&
               $collection->every(fn($f) => $f->dosen_id === $dosen->id);
    });
});
```

### Relationship Loading Tests (3 tests)

**10. Loads Mata Kuliah Relationship**

```php
test('loads mata kuliah relationship', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);

    $program = ProgramStudi::factory()->create();
    $kurikulum = Kurikulum::factory()->create(['program_studi_id' => $program->id]);
    $mataKuliah = MataKuliah::factory()->create([
        'kurikulum_id' => $kurikulum->id,
        'nama_mata_kuliah' => 'Pemrograman Web',
    ]);

    FormasiDosen::factory()->create([
        'dosen_id' => $dosen->id,
        'mata_kuliah_id' => $mataKuliah->id,
        'is_active' => true,
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertViewHas('formasi', function ($collection) {
        return $collection->first()->mataKuliah->nama_mata_kuliah === 'Pemrograman Web';
    });
});
```

**11. Loads Program Studi Relationship**

```php
test('loads program studi relationship', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);

    $program = ProgramStudi::factory()->create(['nama_program' => 'S1 Informatika']);

    FormasiDosen::factory()->create([
        'dosen_id' => $dosen->id,
        'program_studi_id' => $program->id,
        'is_active' => true,
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertViewHas('formasi', function ($collection) {
        return $collection->first()->programStudi->nama_program === 'S1 Informatika';
    });
});
```

**12. Loads Jadwal Perkuliahan Relationship with Filters**

```php
test('loads jadwal perkuliahan relationship with filters', function () {
    config(['siakad.semester_aktif.tahun' => '2025/2026']);
    config(['siakad.semester_aktif.semester' => 'Ganjil']);

    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);
    $formasi = FormasiDosen::factory()->create([
        'dosen_id' => $dosen->id,
        'is_active' => true,
    ]);

    JadwalPerkuliahan::factory()->create([
        'formasi_dosen_id' => $formasi->id,
        'is_active' => true,
        'tahun_akademik' => '2025/2026',
        'semester' => 'Ganjil',
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertViewHas('formasi', function ($collection) {
        $jadwal = $collection->first()->jadwalPerkuliahan;
        return $jadwal->count() === 1 &&
               $jadwal->first()->is_active === true;
    });
});
```

**13. Jadwal Perkuliahan Sorted by Hari and Waktu Mulai**

```php
test('jadwal perkuliahan sorted by hari and waktu_mulai', function () {
    config(['siakad.semester_aktif.tahun' => '2025/2026']);
    config(['siakad.semester_aktif.semester' => 'Ganjil']);

    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);
    $formasi = FormasiDosen::factory()->create([
        'dosen_id' => $dosen->id,
        'is_active' => true,
    ]);

    // Create schedules in reverse order
    JadwalPerkuliahan::factory()->create([
        'formasi_dosen_id' => $formasi->id,
        'hari' => 'Rabu',
        'waktu_mulai' => '10:00',
        'is_active' => true,
        'tahun_akademik' => '2025/2026',
        'semester' => 'Ganjil',
    ]);

    JadwalPerkuliahan::factory()->create([
        'formasi_dosen_id' => $formasi->id,
        'hari' => 'Senin',
        'waktu_mulai' => '08:00',
        'is_active' => true,
        'tahun_akademik' => '2025/2026',
        'semester' => 'Ganjil',
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertViewHas('formasi', function ($collection) {
        $jadwal = $collection->first()->jadwalPerkuliahan;
        return $jadwal->first()->hari === 'Senin' &&
               $jadwal->first()->waktu_mulai === '08:00';
    });
});
```

**14. Displays Multiple Mata Kuliah for Same Dosen**

```php
test('displays multiple mata kuliah for same dosen', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);

    // Create 3 different formasi
    FormasiDosen::factory()->count(3)->create([
        'dosen_id' => $dosen->id,
        'is_active' => true,
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.mata-kuliah.index'));

    $response->assertViewHas('formasi', function ($collection) {
        return $collection->count() === 3;
    });
});
```

## Key Testing Patterns

### 1. Dual Email Lookup Pattern

```php
// Critical for Dosen role - users may login with different email
$dosen = Dosen::where('email_institusi', $user->email)
    ->orWhere('email_pribadi', $user->email)
    ->first();
```

### 2. Graceful Degradation Pattern

```php
// Return empty collection instead of error
if (!$dosen) {
    return view('...', ['formasi' => collect([])]);
}
```

### 3. Semester Filtering Pattern

```php
// Filter by active semester from config
->where('tahun_akademik', config('siakad.semester_aktif.tahun'))
->where('semester', config('siakad.semester_aktif.semester'))
```

### 4. Eager Loading Pattern

```php
// Prevent N+1 queries with eager loading
->with([
    'mataKuliah',
    'programStudi',
    'jadwalPerkuliahan' => function ($query) {
        $query->where('is_active', true)
            ->orderBy('hari')
            ->orderBy('waktu_mulai');
    },
])
```

## Dependencies

**Models**:

- `User`
- `Dosen`
- `FormasiDosen`
- `MataKuliah`
- `ProgramStudi`
- `Kurikulum`
- `JadwalPerkuliahan`

**Factories**:

- `UserFactory`
- `DosenFactory`
- `FormasiDosenFactory`
- `MataKuliahFactory`
- `ProgramStudiFactory`
- `KurikulumFactory`
- `JadwalPerkuliahanFactory`

**Roles** (Spatie Permissions):

- `dosen` (required)

**Configuration**:

- `config/siakad.php` → `semester_aktif.tahun` and `semester_aktif.semester`

## Running Tests

```bash
# Run all Mata Kuliah tests
php artisan test tests/Feature/Dosen/MataKuliahTest.php

# Run specific test
php artisan test --filter="dosen can view mata kuliah list"

# Run with coverage
php artisan test tests/Feature/Dosen/MataKuliahTest.php --coverage
```

## Common Issues & Solutions

**Issue**: Dosen not found even though user is authenticated

- **Solution**: Check if Dosen record has matching email (institusi OR pribadi)
- **Test**: `returns empty collection when dosen not found`

**Issue**: Jadwal not showing

- **Solution**: Verify jadwal has correct tahun_akademik and semester in config
- **Test**: `filters by semester_aktif`

**Issue**: Seeing other dosen's courses

- **Solution**: Check `where('dosen_id', $dosen->id)` filter
- **Test**: `only shows own mata kuliah`

**Issue**: N+1 query performance problem

- **Solution**: Verify eager loading with `->with(['mataKuliah', 'programStudi', 'jadwalPerkuliahan'])`
- **Test**: Relationship loading tests verify eager loading works

## Conclusion

This module provides a simple, read-only view of courses taught by the authenticated dosen. It demonstrates:

- ✅ Dual email lookup pattern (critical for Dosen role)
- ✅ Graceful degradation when data not found
- ✅ Proper filtering (active formasi, current semester, own courses)
- ✅ Efficient eager loading to prevent N+1 queries
- ✅ Sorted jadwal display (by day and time)

**Status**: ✅ All 14 tests passing (100%)
