# 06. Formasi Dosen Testing

**Test File**: `tests/Feature/Admin/FormasiDosenTest.php`  
**Controller**: `app/Http/Controllers/FormasiDosenController.php`  
**Model**: `app/Models/FormasiDosen.php`

---

## Results

✅ **30/30 tests passing** (1.52s, 58 assertions)

---

## Module Characteristics

### Read-Only Controller Pattern

- **Available**: index(), show() only
- **Disabled**: create, store, edit, update, destroy (all return abort 404)
- **Unique**: Show route uses Dosen ID, not FormasiDosen ID

### Business Logic

- Index lists Dosen (not FormasiDosen directly)
- Calculates SKS: teori, praktek, total from JadwalPerkuliahan
- Filters by semester_aktif, search by nama_lengkap
- Show displays all Formasi for selected Dosen

---

## Test Coverage

### Authorization (2)

- ✅ Authenticated access required
- ✅ Guest redirect to login

### Index/List (7)

- ✅ Display Dosen with formasi relationships
- ✅ Empty state when no active Dosen
- ✅ Pagination (10 per page)
- ✅ Search by nama_lengkap
- ✅ Filter by semester
- ✅ **SKS calculation accuracy** (teori + praktek = total)
- ✅ Eager loading prevents N+1

### Show/Detail (4)

- ✅ View Dosen formasi detail
- ✅ Display with formasi list
- ✅ 404 for non-existent Dosen
- ✅ Filter by semester

### CRUD Disabled (5)

- ✅ Create returns 404
- ✅ Store returns 404
- ✅ Edit returns 404
- ✅ Update returns 404
- ✅ Destroy returns 404

### Model (8)

- ✅ Fillable fields (12 attributes)
- ✅ is_active cast to boolean
- ✅ Relationships: MataKuliah, Dosen, ProgramStudi, JadwalPerkuliahan
- ✅ Scopes: active(), semester()
- ✅ Accessor: nama_kelas (computed)

### Factory (2)

- ✅ Creates valid data
- ✅ Multiple instances

### Performance (2)

- ✅ 100 Dosen < 2s
- ✅ N+1 prevention

---

## Key Implementation Details

### JadwalPerkuliahan Creation

- **No Factory Available**: Model lacks HasFactory trait
- **Direct Creation**: Use `JadwalPerkuliahan::create([...])`
- **Required Field**: `ruangan_id` (NOT NULL constraint)

### Route Parameters

- Index: No parameters
- Show: `route('siakad.formasi-dosen.show', $dosen)` - Uses Dosen ID

### Test Data Structure

```php
beforeEach(function () {
    Role::create(['name' => 'admin']);
    $this->user = User::factory()->create();
    $this->user->assignRole('admin');
    $this->actingAs($this->user);

    $this->programStudi = ProgramStudi::factory()->create();
    $this->dosen = Dosen::factory()->create();
    $this->mataKuliah = MataKuliah::factory()->create();
    $this->ruangan = Ruangan::factory()->create(); // Required for JadwalPerkuliahan
});
```

---

## Notable Tests

### SKS Calculation Test

```php
test('index calculates sks correctly', function () {
    $formasi = FormasiDosen::factory()->create([
        'dosen_id' => $this->dosen->id,
    ]);

    // Create Teori (2 SKS) + Praktek (1 SKS)
    JadwalPerkuliahan::create([
        'formasi_dosen_id' => $formasi->id,
        'ruangan_id' => $this->ruangan->id,
        'jenis' => 'Teori',
        'sks' => 2,
    ]);

    JadwalPerkuliahan::create([
        'formasi_dosen_id' => $formasi->id,
        'ruangan_id' => $this->ruangan->id,
        'jenis' => 'Praktek',
        'sks' => 1,
    ]);

    $response = $this->get(route('siakad.formasi-dosen.index'));

    // Verify: 2 Teori + 1 Praktek = 3 Total
    $dosen = $response->viewData('dosenList')->first();
    expect($dosen->sks_teori)->toBe(2);
    expect($dosen->sks_praktek)->toBe(1);
    expect($dosen->sks_total)->toBe(3);
});
```

### CRUD Disabled Pattern

```php
test('create returns 404', function () {
    $response = $this->get(route('siakad.formasi-dosen.create'));
    $response->assertNotFound();
});
```

---

## Test Execution

```bash
# Run all tests
php artisan test tests/Feature/Admin/FormasiDosenTest.php

# Run specific test
php artisan test --filter="index calculates sks correctly"

# With coverage
php artisan test tests/Feature/Admin/FormasiDosenTest.php --coverage
```

---

## Lessons Learned

1. **Read-Only Controllers**: Adapt test structure - no CRUD tests, add "CRUD Disabled" section
2. **Route Parameters**: Verify controller expectations (Dosen ID here, not FormasiDosen ID)
3. **Model Factories**: Check HasFactory trait before using `Model::factory()`
4. **Database Constraints**: NOT NULL fields caught by tests (ruangan_id required)
5. **Business Logic Testing**: SKS calculations verify controller aggregation logic

---

**Date**: 2025-01-17  
**Module**: Admin - Formasi Dosen (Teaching Assignment)  
**Status**: ✅ Complete (30/30 passing)
