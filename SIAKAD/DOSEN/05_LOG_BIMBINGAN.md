# Log Bimbingan (PA Dosen) - Testing Guide

## Overview

**Module**: Log Bimbingan - Academic Guidance Logging  
**Controller**: `App\Http\Controllers\Dosen\LogBimbinganController`  
**Route Prefix**: `/siakad/dosen/log-bimbingan`  
**Test File**: `tests/Feature/Dosen/LogBimbinganTest.php`  
**Total Tests**: 18 tests, 37 assertions  
**Status**: ✅ ALL PASSING (100%)

## Purpose

Enables **PA (Pembimbing Akademik)** dosen to:

- View list of advisee students (mahasiswa bimbingan)
- Record guidance sessions with students
- Track academic advising history per student
- View complete chronological log of all sessions

This module is specifically for dosen acting as **PA** who have been assigned to students via `mahasiswa.dosen_pa_id` relationship.

## Controller Methods

### 1. `index()` - List PA Mahasiswa

**Route**: `GET /siakad/dosen/log-bimbingan` → `siakad.dosen.log-bimbingan.index`

**Logic**:

```php
public function index()
{
    $user = auth()->user();

    // Find dosen by email (dual lookup)
    $dosen = Dosen::where('email_institusi', $user->email)
        ->orWhere('email_pribadi', $user->email)
        ->first();

    if (!$dosen) {
        return view('dosen.log-bimbingan.index', ['mahasiswa' => collect([])]);
    }

    // Only active students where dosen is PA
    $mahasiswa = Mahasiswa::where('dosen_pa_id', $dosen->id)
        ->where('status_mahasiswa', 'Aktif')
        ->paginate(20);

    return view('dosen.log-bimbingan.index', compact('mahasiswa'));
}
```

**Key Features**:

- ✅ Dual email lookup (institusi OR pribadi)
- ✅ Only shows active mahasiswa
- ✅ Only shows own PA students
- ✅ Paginated (20/page)
- ✅ Graceful degradation if no dosen record

### 2. `show($id)` - Show Mahasiswa Logs

**Route**: `GET /siakad/dosen/log-bimbingan/{id}` → `siakad.dosen.log-bimbingan.show`

**Logic**:

```php
public function show($id)
{
    $mahasiswa = Mahasiswa::findOrFail($id);

    // Get all logs ordered by date (newest first)
    $logs = LogBimbingan::where('mahasiswa_id', $id)
        ->orderBy('tanggal', 'desc')
        ->get();

    return view('dosen.log-bimbingan.show', compact('mahasiswa', 'logs'));
}
```

**Key Features**:

- ✅ Returns 404 if mahasiswa not found
- ✅ Logs ordered newest first
- ✅ Complete history displayed

### 3. `create($mahasiswaId)` - Show Create Form

**Route**: `GET /siakad/dosen/log-bimbingan/create/{mahasiswaId}`

**Returns**: Form view for creating new log entry

### 4. `store(Request)` - Save New Log

**Route**: `POST /siakad/dosen/log-bimbingan` → `siakad.dosen.log-bimbingan.store`

**Validation**:

```php
$request->validate([
    'mahasiswa_id' => 'required|exists:mahasiswa,id',
    'tanggal' => 'required|date',
    'topik' => 'required|string|max:255',
    'catatan' => 'required|string',
]);
```

**Logic**:

```php
public function store(Request $request)
{
    $validated = $request->validate([...]);

    $user = auth()->user();
    $dosen = Dosen::where('email_institusi', $user->email)
        ->orWhere('email_pribadi', $user->email)
        ->firstOrFail(); // Throws 404 if not found

    LogBimbingan::create([
        'mahasiswa_id' => $validated['mahasiswa_id'],
        'dosen_id' => $dosen->id, // Auto-assigned
        'tanggal' => $validated['tanggal'],
        'topik' => $validated['topik'],
        'catatan' => $validated['catatan'],
    ]);

    return redirect()
        ->route('siakad.dosen.log-bimbingan.show', $validated['mahasiswa_id'])
        ->with('success', 'Log bimbingan berhasil ditambahkan');
}
```

**Key Features**:

- ✅ Requires dosen record (throws 404 if missing)
- ✅ Auto-assigns dosen_id
- ✅ Redirects to mahasiswa log list
- ✅ Validates all required fields

## Test Coverage (18 tests)

### Authorization (2 tests)

**1. Requires Authentication**

```php
test('requires authentication', function () {
    $response = $this->get(route('siakad.dosen.log-bimbingan.index'));
    $response->assertRedirect(route('login'));
});
```

**2. Requires Dosen Role**

```php
test('requires dosen role', function () {
    $user = User::factory()->create();
    $user->assignRole('mahasiswa');

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.log-bimbingan.index'));

    $response->assertForbidden();
});
```

### Index Tests (7 tests)

**3-9**: View index, empty state, dual email lookup (institusi/pribadi), active-only filter, own PA students only, pagination

### Show Tests (3 tests)

**10. Dosen Can View Mahasiswa Log Bimbingan**

```php
test('dosen can view mahasiswa log bimbingan', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);
    $mahasiswa = Mahasiswa::factory()->create(['dosen_pa_id' => $dosen->id]);

    LogBimbingan::factory()->count(3)->create([
        'mahasiswa_id' => $mahasiswa->id,
        'dosen_id' => $dosen->id,
    ]);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.log-bimbingan.show', $mahasiswa->id));

    $response->assertStatus(200);
    $response->assertViewHas('mahasiswa');
    $response->assertViewHas('logs');
});
```

**11. Show Returns 404 for Non-Existent Mahasiswa**

**12. Show Orders Logs by Tanggal Descending**

```php
test('show orders logs by tanggal descending', function () {
    // Create logs with different dates
    LogBimbingan::factory()->create(['tanggal' => '2025-01-01']);
    LogBimbingan::factory()->create(['tanggal' => '2025-03-01']);
    LogBimbingan::factory()->create(['tanggal' => '2025-02-01']);

    $response = $this->actingAs($user)
        ->get(route('siakad.dosen.log-bimbingan.show', $mahasiswa->id));

    // First should be newest (2025-03-01)
    $response->assertViewHas('logs', function ($logs) {
        return $logs->first()->tanggal->format('Y-m-d') === '2025-03-01';
    });
});
```

### Create Tests (2 tests)

**13. Dosen Can View Create Log Form**
**14. Create Returns 404 for Non-Existent Mahasiswa**

### Store Tests (4 tests)

**15. Dosen Can Store Log Bimbingan**

```php
test('dosen can store log bimbingan', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    $dosen = Dosen::factory()->create(['email_institusi' => $user->email]);
    $mahasiswa = Mahasiswa::factory()->create(['dosen_pa_id' => $dosen->id]);

    $data = [
        'mahasiswa_id' => $mahasiswa->id,
        'tanggal' => '2025-12-10',
        'topik' => 'Konsultasi Skripsi BAB 1',
        'catatan' => 'Pembahasan latar belakang dan rumusan masalah',
    ];

    $response = $this->actingAs($user)
        ->post(route('siakad.dosen.log-bimbingan.store'), $data);

    $response->assertRedirect(route('siakad.dosen.log-bimbingan.show', $mahasiswa->id));
    $response->assertSessionHas('success');

    // Note: Table name is log_bimbingans (plural)
    $this->assertDatabaseHas('log_bimbingans', [
        'mahasiswa_id' => $mahasiswa->id,
        'dosen_id' => $dosen->id,
        'topik' => 'Konsultasi Skripsi BAB 1',
    ]);
});
```

**16. Store Validates Required Fields**

```php
test('store validates required fields', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');

    Dosen::factory()->create(['email_institusi' => $user->email]);

    $response = $this->actingAs($user)
        ->post(route('siakad.dosen.log-bimbingan.store'), []);

    $response->assertInvalid(['mahasiswa_id', 'tanggal', 'topik', 'catatan']);
});
```

**17. Store Validates Mahasiswa Exists**

**18. Store Requires Dosen Record to Exist**

```php
test('store requires dosen record to exist', function () {
    $user = User::factory()->create();
    $user->assignRole('dosen');
    // No dosen record created

    $mahasiswa = Mahasiswa::factory()->create();
    $data = [
        'mahasiswa_id' => $mahasiswa->id,
        'tanggal' => '2025-12-10',
        'topik' => 'Test',
        'catatan' => 'Test catatan',
    ];

    $response = $this->actingAs($user)
        ->post(route('siakad.dosen.log-bimbingan.store'), $data);

    // firstOrFail() throws 404
    $response->assertNotFound();
});
```

## Key Testing Patterns

### 1. PA Relationship Filter

```php
// Only show students assigned to this dosen as PA
Mahasiswa::where('dosen_pa_id', $dosen->id)
    ->where('status_mahasiswa', 'Aktif')
    ->get()
```

### 2. Log Ordering

```php
// Newest logs first
LogBimbingan::where('mahasiswa_id', $id)
    ->orderBy('tanggal', 'desc')
    ->get()
```

### 3. Auto-Assign Dosen Pattern

```php
// Store requires dosen, auto-assigns dosen_id
$dosen = Dosen::where('email_institusi', $user->email)
    ->orWhere('email_pribadi', $user->email)
    ->firstOrFail(); // Throws 404 if not found

LogBimbingan::create([
    'dosen_id' => $dosen->id, // Auto-assigned
    // ...
]);
```

## Factory & Model Setup

**LogBimbinganFactory** (created for tests):

```php
public function definition(): array
{
    return [
        'mahasiswa_id' => \App\Models\Mahasiswa::factory(),
        'dosen_id' => \App\Models\Dosen::factory(),
        'tanggal' => fake()->dateTimeBetween('-6 months', 'now'),
        'topik' => fake()->randomElement([
            'Konsultasi Skripsi BAB 1',
            'Konsultasi Skripsi BAB 2',
            'Konsultasi Skripsi BAB 3',
            'Konsultasi Tugas Akhir',
            'Konsultasi Akademik',
            'Konsultasi KRS',
            'Pembahasan Proposal',
            'Review Progress Penelitian',
        ]),
        'catatan' => fake()->paragraph(),
    ];
}
```

**LogBimbingan Model** (updated):

```php
use Illuminate\Database\Eloquent\Factories\HasFactory;

class LogBimbingan extends Model
{
    use HasFactory; // Added for factory support

    protected $fillable = [
        'mahasiswa_id', 'dosen_id', 'tanggal', 'topik', 'catatan',
    ];

    protected $casts = [
        'tanggal' => 'date',
    ];
}
```

## Dependencies

**Models**: `User`, `Dosen`, `Mahasiswa`, `LogBimbingan`

**Factories**: `UserFactory`, `DosenFactory`, `MahasiswaFactory`, `LogBimbinganFactory`

**Database**: Table `log_bimbingans` (plural), Migration: `2025_12_05_062938_create_log_bimbingans_table.php`

**Roles**: `dosen` (required)

## Running Tests

```bash
# All tests
php artisan test tests/Feature/Dosen/LogBimbinganTest.php

# Specific test
php artisan test --filter="dosen can store log bimbingan"

# With coverage
php artisan test tests/Feature/Dosen/LogBimbinganTest.php --coverage
```

## Common Issues & Solutions

**Issue 1**: Table name mismatch

- **Problem**: Test uses `log_bimbingan` (singular) but table is `log_bimbingans` (plural)
- **Solution**: Use correct plural table name
- **Test**: #15 (store log bimbingan)

**Issue 2**: Dosen not found on store

- **Problem**: No dosen record for authenticated user
- **Solution**: Controller uses `firstOrFail()` → returns 404
- **Test**: #18 (requires dosen record)

**Issue 3**: Seeing other dosen's students

- **Problem**: Missing PA filter
- **Solution**: Filter `where('dosen_pa_id', $dosen->id)`
- **Test**: #8 (only shows own bimbingan mahasiswa)

## Conclusion

PA dosen module for tracking academic guidance sessions:

- ✅ PA relationship filtering (dosen_pa_id)
- ✅ Active status filtering
- ✅ Pagination (20/page)
- ✅ Date ordering (newest first)
- ✅ Required field validation
- ✅ Auto-assign dosen_id

**Status**: ✅ All 18 tests passing (100%)
