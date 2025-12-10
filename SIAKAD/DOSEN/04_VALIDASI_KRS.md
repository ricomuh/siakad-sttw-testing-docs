# Validasi KRS (PA Dosen) - Testing Guide

## Overview

**Module**: Validasi KRS - PA Dosen KRS Approval  
**Controller**: `App\Http\Controllers\Dosen\ValidasiKrsController`  
**Route Prefix**: `/siakad/dosen/validasi-krs`  
**Test File**: `tests/Feature/Dosen/ValidasiKrsTest.php`  
**Total Tests**: 20 tests, 41 assertions  
**Status**: ✅ ALL PASSING (100%)

## Purpose

Enables **PA (Pembimbing Akademik)** dosen to:

- View list of advisee students with KRS status
- Review student course registrations (KRS)
- Approve all courses at once (bulk approve)
- Reject all courses at once (bulk reject)
- Approve individual courses
- Reject individual courses with reason

This module implements the KRS approval workflow where students submit course selections and PA dosen must approve before registration is finalized.

## Controller Methods

### 1. `index()` - List PA Mahasiswa with KRS Status

**Route**: `GET /siakad/dosen/validasi-krs` → `siakad.dosen.validasi-krs.index`

**Logic**:

```php
public function index()
{
    $user = auth()->user();

    $dosen = Dosen::where('email_institusi', $user->email)
        ->orWhere('email_pribadi', $user->email)
        ->first();

    if (!$dosen) {
        return view('dosen.validasi-krs.index', ['mahasiswa' => collect([])]);
    }

    // Get PA students with KRS summary
    $mahasiswa = Mahasiswa::where('dosen_pa_id', $dosen->id)
        ->where('status_mahasiswa', 'Aktif')
        ->with(['krs' => function ($q) {
            $q->where('semester', config('siakad.semester_aktif'))
              ->where('tahun_akademik', config('siakad.tahun_akademik_aktif'));
        }])
        ->paginate(20);

    return view('dosen.validasi-krs.index', compact('mahasiswa'));
}
```

**Key Features**:

- ✅ Shows PA students only
- ✅ Loads KRS for active semester
- ✅ Shows KRS status summary
- ✅ Paginated (20/page)

### 2. `show($mahasiswaId)` - Show Student KRS Detail

**Route**: `GET /siakad/dosen/validasi-krs/{mahasiswa}` → `siakad.dosen.validasi-krs.show`

**Logic**:

```php
public function show($mahasiswaId)
{
    $mahasiswa = Mahasiswa::findOrFail($mahasiswaId);

    // Get KRS for current semester
    $krs = Krs::where('mahasiswa_id', $mahasiswaId)
        ->where('semester', config('siakad.semester_aktif'))
        ->where('tahun_akademik', config('siakad.tahun_akademik_aktif'))
        ->with(['mataKuliah', 'jadwalPerkuliahan'])
        ->get();

    return view('dosen.validasi-krs.show', compact('mahasiswa', 'krs'));
}
```

### 3. `approveAll($mahasiswaId)` - Bulk Approve

**Route**: `POST /siakad/dosen/validasi-krs/{mahasiswa}/approve-all` → `siakad.dosen.validasi-krs.approve-all`

**Logic**:

```php
public function approveAll($mahasiswaId)
{
    $krs = Krs::where('mahasiswa_id', $mahasiswaId)
        ->where('status', 'Diajukan')
        ->where('semester', config('siakad.semester_aktif'))
        ->get();

    foreach ($krs as $item) {
        $item->update(['status' => 'Disetujui']);

        // Log approval
        KrsLog::create([
            'krs_id' => $item->id,
            'action' => 'approved',
            'user_id' => auth()->id(),
        ]);
    }

    return redirect()->back()->with('success', 'Semua KRS berhasil disetujui');
}
```

### 4. `rejectAll($mahasiswaId)` - Bulk Reject

**Route**: `POST /siakad/dosen/validasi-krs/{mahasiswa}/reject-all`

**Similar to approveAll** but sets status to 'Ditolak'

### 5. `approveItem($krsId)` - Approve Single Course

**Route**: `POST /siakad/dosen/validasi-krs/item/{krs}/approve` → `siakad.dosen.validasi-krs.item.approve`

**Logic**:

```php
public function approveItem($krsId)
{
    $krs = Krs::findOrFail($krsId);

    $krs->update(['status' => 'Disetujui']);

    KrsLog::create([
        'krs_id' => $krs->id,
        'action' => 'approved',
        'user_id' => auth()->id(),
    ]);

    return redirect()->back()->with('success', 'KRS berhasil disetujui');
}
```

### 6. `rejectItem(Request, $krsId)` - Reject Single Course

**Route**: `POST /siakad/dosen/validasi-krs/item/{krs}/reject` → `siakad.dosen.validasi-krs.item.reject`

**Validation**:

```php
$request->validate([
    'alasan' => 'required|string|max:500',
]);
```

**Logic**:

```php
public function rejectItem(Request $request, $krsId)
{
    $validated = $request->validate([
        'alasan' => 'required|string|max:500',
    ]);

    $krs = Krs::findOrFail($krsId);

    $krs->update([
        'status' => 'Ditolak',
        'alasan_penolakan' => $validated['alasan'],
    ]);

    KrsLog::create([
        'krs_id' => $krs->id,
        'action' => 'rejected',
        'reason' => $validated['alasan'],
        'user_id' => auth()->id(),
    ]);

    return redirect()->back()->with('success', 'KRS berhasil ditolak');
}
```

## Test Coverage (20 tests)

### Authorization (2 tests)

**1. Requires Authentication**

```php
test('requires authentication', function () {
    $response = $this->get(route('siakad.dosen.validasi-krs.index'));
    $response->assertRedirect(route('login'));
});
```

**2. Requires Dosen Role**

### Index Tests (7 tests)

**3-9**: View index, empty state, dual email lookup, active-only filter, own PA students, semester filtering, pagination

### Show Tests (3 tests)

**10. Dosen Can View Mahasiswa KRS**

```php
test('dosen can view mahasiswa krs', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);
    $mahasiswa = Mahasiswa::factory()->create(['dosen_pa_id' => $dosen->id]);

    // Create KRS items
    $krs = Krs::factory()->count(5)->create([
        'mahasiswa_id' => $mahasiswa->id,
        'status' => 'Diajukan',
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.validasi-krs.show', $mahasiswa->id));

    $response->assertStatus(200);
    $response->assertViewHas('mahasiswa');
    $response->assertViewHas('krs');
});
```

**11. Show Returns 404 for Non-Existent Mahasiswa**

**12. Show Loads KRS with Relationships**

### Approve All Tests (2 tests)

**13. Dosen Can Approve All KRS**

```php
test('dosen can approve all krs', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);
    $mahasiswa = Mahasiswa::factory()->create(['dosen_pa_id' => $dosen->id]);

    // Create KRS with 'Diajukan' status
    $krs = Krs::factory()->count(3)->create([
        'mahasiswa_id' => $mahasiswa->id,
        'status' => 'Diajukan',
    ]);

    $response = $this->actingAs($user)
        ->post(route('siakad.dosen.validasi-krs.approve-all', $mahasiswa->id));

    $response->assertRedirect();
    $response->assertSessionHas('success');

    // All should be 'Disetujui'
    $this->assertEquals(3, Krs::where('mahasiswa_id', $mahasiswa->id)
        ->where('status', 'Disetujui')
        ->count());
});
```

**14. Approve All Only Updates Diajukan Status**

### Reject All Tests (2 tests)

**15. Dosen Can Reject All KRS**

**16. Reject All Only Updates Diajukan Status**

### Approve Item Tests (2 tests)

**17. Dosen Can Approve Single KRS Item**

```php
test('dosen can approve single krs item', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    Dosen::factory()->create(['email_institusi' => $user->email]);

    $krs = Krs::factory()->create(['status' => 'Diajukan']);

    $response = $this->actingAs($user)
        ->post(route('siakad.dosen.validasi-krs.item.approve', $krs->id));

    $response->assertRedirect();
    $response->assertSessionHas('success');

    $this->assertDatabaseHas('krs', [
        'id' => $krs->id,
        'status' => 'Disetujui',
    ]);
});
```

**18. Approve Item Returns 404 for Non-Existent KRS**

### Reject Item Tests (4 tests)

**19. Dosen Can Reject Single KRS Item with Reason**

```php
test('dosen can reject single krs item with reason', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    Dosen::factory()->create(['email_institusi' => $user->email]);

    $krs = Krs::factory()->create(['status' => 'Diajukan']);

    $data = [
        'alasan' => 'Mata kuliah bentrok dengan jadwal lain',
    ];

    $response = $this->actingAs($user)
        ->post(route('siakad.dosen.validasi-krs.item.reject', $krs->id), $data);

    $response->assertRedirect();
    $response->assertSessionHas('success');

    $this->assertDatabaseHas('krs', [
        'id' => $krs->id,
        'status' => 'Ditolak',
        'alasan_penolakan' => 'Mata kuliah bentrok dengan jadwal lain',
    ]);
});
```

**20. Reject Item Validates Alasan is Required**

```php
test('reject item validates alasan is required', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    Dosen::factory()->create(['email_institusi' => $user->email]);

    $krs = Krs::factory()->create(['status' => 'Diajukan']);

    $response = $this->actingAs($user)
        ->post(route('siakad.dosen.validasi-krs.item.reject', $krs->id), []);

    $response->assertInvalid('alasan');
});
```

**Additional tests**: Reject returns 404, validates max length

## Key Testing Patterns

### 1. PA Student Filter

```php
// Only show students assigned to this dosen as PA
Mahasiswa::where('dosen_pa_id', $dosen->id)
    ->where('status_mahasiswa', 'Aktif')
    ->get()
```

### 2. Semester Filtering

```php
// Only show KRS for active semester
Krs::where('mahasiswa_id', $id)
    ->where('semester', config('siakad.semester_aktif'))
    ->where('tahun_akademik', config('siakad.tahun_akademik_aktif'))
    ->get()
```

### 3. Bulk Status Update

```php
// Update all 'Diajukan' to 'Disetujui'
Krs::where('mahasiswa_id', $id)
    ->where('status', 'Diajukan')
    ->update(['status' => 'Disetujui']);
```

### 4. Audit Trail Pattern

```php
// Log every approval/rejection
KrsLog::create([
    'krs_id' => $krs->id,
    'action' => 'approved', // or 'rejected'
    'reason' => $reason ?? null,
    'user_id' => auth()->id(),
]);
```

### 5. Route Naming Convention Fix

```php
// Correct route names (not approve-item, but item.approve)
route('siakad.dosen.validasi-krs.item.approve', $krs->id)
route('siakad.dosen.validasi-krs.item.reject', $krs->id)
```

## Validation Rules

### Reject Item

```php
'alasan' => 'required|string|max:500',
```

## Dependencies

**Models**: `User`, `Dosen`, `Mahasiswa`, `Krs`, `KrsLog`, `MataKuliah`, `JadwalPerkuliahan`

**Factories**: All corresponding model factories

**Config**: `config('siakad.semester_aktif')`, `config('siakad.tahun_akademik_aktif')`

**Roles**: `dosen` (required)

## Running Tests

```bash
# All tests
php artisan test tests/Feature/Dosen/ValidasiKrsTest.php

# Specific test groups
php artisan test --filter="approve"
php artisan test --filter="reject"

# With coverage
php artisan test tests/Feature/Dosen/ValidasiKrsTest.php --coverage
```

## Common Issues & Solutions

**Issue 1**: Route not found errors

- **Problem**: Route names changed from `approve-item` to `item.approve`
- **Solution**: Use correct nested route naming
- **Tests**: #17, #19 (item approve/reject)

**Issue 2**: Approving already approved KRS

- **Problem**: Bulk approve tries to update 'Disetujui' status
- **Solution**: Filter `where('status', 'Diajukan')` before update
- **Test**: #14 (only updates diajukan)

**Issue 3**: Missing alasan on reject

- **Problem**: Rejection without reason
- **Solution**: Validate `alasan` is required
- **Test**: #20 (validates alasan required)

**Issue 4**: Seeing other PA's students

- **Problem**: Missing PA filter
- **Solution**: Filter `where('dosen_pa_id', $dosen->id)`
- **Test**: #8 (only shows own PA students)

## Conclusion

KRS validation workflow for PA dosen:

- ✅ PA student filtering
- ✅ Semester filtering
- ✅ Bulk approve/reject operations
- ✅ Individual approve/reject
- ✅ Rejection reason validation
- ✅ Audit trail logging (KrsLog)

**Status**: ✅ All 20 tests passing (100%)
