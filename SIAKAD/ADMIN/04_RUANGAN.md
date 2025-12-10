# Ruangan Testing - Module 8

**Status**: ✅ 34/34 passing (100%)  
**Duration**: ~1.71s  
**Controller**: `app/Http/Controllers/RuanganController.php` (Full CRUD + API)

## 🎯 Test Coverage Overview

**Total Tests**: 34

- ✅ Authorization: 2 tests
- ✅ Index/List: 7 tests (display, empty, pagination, search by nama/kode, filter by jenis/gedung)
- ✅ Create: 1 test
- ✅ Store: 5 tests (valid data, auto-generate kode, duplicate prevention, validation, time validation)
- ✅ Show/Detail: 3 tests
- ✅ Edit: 1 test
- ✅ Update: 4 tests (update, auto-generate kode, duplicate prevention, validation)
- ✅ Delete: 2 tests
- ✅ Model: 9 tests (fillable, cast, relationship, scope, 2 static methods, 2 accessor tests)
- ✅ Performance: 1 test

## 🔄 Module Characteristics

**Pattern**: Full CRUD with auto-generation feature

**Business Logic Features**:

- **Auto-generate Kode Ruangan**: Format `Gedung.Lantai.NoRuang` (e.g., `A.3.5`)
- **Duplicate Prevention**: Checks kode_ruangan uniqueness on create/update
- **Time Validation**: `jam_tutup` must be after `jam_buka`
- **Static Helper Methods**: `generateKodeRuangan()`, `parseKodeRuangan()`
- **Accessor**: `waktu_operasional` returns formatted time range or '-'
- **Filtering**: By jenis_ruangan, gedung
- **Search**: By nama_ruangan, kode_ruangan, gedung

**Dependencies**:

- Ruangan: ✅ Has factory
- All required models have factories

## ⚙️ Setup Pattern

```php
beforeEach(function () {
    \Spatie\Permission\Models\Role::create(['name' => 'admin']);
    $this->user = User::factory()->create();
    $this->user->assignRole('admin');
    $this->actingAs($this->user);
});
```

**No complex setup needed** - Ruangan is independent model with factory.

## 🎯 First Run Success!

**All 34 tests passed on first execution** (6.66s initial, 1.71s after formatting) ✅

No debugging required - clean implementation!

## 📊 Key Test Examples

### Auto-Generate Kode Ruangan

```php
test('store auto-generates kode ruangan', function () {
    $data = [
        'nama_ruangan' => 'Ruang Test',
        'gedung' => 'B',
        'lantai' => '2',
        'no_ruang' => '7',
        'jenis_ruangan' => 'Ruang Kelas',
        'kapasitas' => 30,
    ];

    $this->post(route('siakad.ruangan.store'), $data);

    $this->assertDatabaseHas('ruangan', [
        'kode_ruangan' => 'B.2.7',  // Auto-generated
    ]);
});
```

### Duplicate Prevention

```php
test('store prevents duplicate kode ruangan', function () {
    Ruangan::factory()->create([
        'kode_ruangan' => 'A.1.1',
        'gedung' => 'A',
        'lantai' => '1',
        'no_ruang' => '1',
    ]);

    $data = [
        'nama_ruangan' => 'Duplicate Room',
        'gedung' => 'A',  // Same combination
        'lantai' => '1',
        'no_ruang' => '1',
        'jenis_ruangan' => 'Ruang Kelas',
        'kapasitas' => 30,
    ];

    $response = $this->post(route('siakad.ruangan.store'), $data);

    $response->assertSessionHas('error');
    expect(Ruangan::where('nama_ruangan', 'Duplicate Room')->exists())->toBeFalse();
});
```

### Time Validation

```php
test('store validates jam tutup after jam buka', function () {
    $data = [
        'nama_ruangan' => 'Test Room',
        'gedung' => 'A',
        'lantai' => '1',
        'no_ruang' => '1',
        'jenis_ruangan' => 'Ruang Kelas',
        'kapasitas' => 30,
        'jam_buka' => '17:00',
        'jam_tutup' => '08:00',  // Before jam_buka ❌
    ];

    $response = $this->post(route('siakad.ruangan.store'), $data);

    $response->assertInvalid(['jam_tutup']);
});
```

### Static Helper Methods

```php
test('ruangan generate kode ruangan static method works', function () {
    $kode = Ruangan::generateKodeRuangan('A', '3', '5');

    expect($kode)->toBe('A.3.5');
});

test('ruangan parse kode ruangan static method works', function () {
    $parsed = Ruangan::parseKodeRuangan('B.2.7');

    expect($parsed)->toBeArray();
    expect($parsed['gedung'])->toBe('B');
    expect($parsed['lantai'])->toBe('2');
    expect($parsed['no_ruang'])->toBe('7');
});
```

### Accessor Testing

```php
test('ruangan waktu operasional accessor works', function () {
    $ruangan = Ruangan::factory()->create([
        'jam_buka' => '08:00',
        'jam_tutup' => '17:00',
    ]);

    expect($ruangan->waktu_operasional)->toContain('08:00');
    expect($ruangan->waktu_operasional)->toContain('17:00');
});

test('ruangan waktu operasional returns dash when no hours', function () {
    $ruangan = Ruangan::factory()->create([
        'jam_buka' => null,
        'jam_tutup' => null,
    ]);

    expect($ruangan->waktu_operasional)->toBe('-');
});
```

## 🎓 Key Features Tested

### 1. Auto-Generation Logic

Controller automatically generates `kode_ruangan` from:

```php
$validated['kode_ruangan'] = Ruangan::generateKodeRuangan(
    $validated['gedung'],
    $validated['lantai'],
    $validated['no_ruang']
);
```

Format: `{Gedung}.{Lantai}.{NoRuang}` → Always uppercase gedung

### 2. Duplicate Detection

Both create and update check for existing kode:

```php
// Store:
if (Ruangan::where('kode_ruangan', $validated['kode_ruangan'])->exists()) {
    return back()->withInput()->with('error', 'Ruangan dengan kode ... sudah ada!');
}

// Update:
if (Ruangan::where('kode_ruangan', $newKode)->where('id', '!=', $ruangan->id)->exists()) {
    return back()->withInput()->with('error', 'Ruangan dengan kode ... sudah ada!');
}
```

### 3. Time Validation

Uses Laravel's `after:` validation:

```php
'jam_tutup' => 'nullable|date_format:H:i|after:jam_buka',
```

### 4. Static Helper Methods

```php
// Generate kode
public static function generateKodeRuangan($gedung, $lantai, $noRuang)
{
    return strtoupper($gedung) . '.' . $lantai . '.' . $noRuang;
}

// Parse kode
public static function parseKodeRuangan($kode)
{
    $parts = explode('.', $kode);
    if (count($parts) === 3) {
        return [
            'gedung' => $parts[0],
            'lantai' => $parts[1],
            'no_ruang' => $parts[2],
        ];
    }
    return null;
}
```

### 5. Waktu Operasional Accessor

```php
public function getWaktuOperasionalAttribute()
{
    if ($this->jam_buka && $this->jam_tutup) {
        return $this->jam_buka->format('H:i') . ' - ' . $this->jam_tutup->format('H:i');
    }
    return '-';
}
```

## 📈 Progress Summary

**Modules Completed**: 8/15+  
**Total Tests**: 306 (all passing)  
**Cumulative Time**: ~14.64s

| Module             | Tests  | Duration  | Status |
| ------------------ | ------ | --------- | ------ |
| Program Studi      | 38     | 1.7s      | ✅     |
| Dosen              | 49     | 2.0s      | ✅     |
| Mahasiswa          | 46     | 2.3s      | ✅     |
| Mata Kuliah        | 36     | 1.7s      | ✅     |
| Kurikulum          | 33     | 1.74s     | ✅     |
| Formasi Dosen      | 30     | 1.52s     | ✅     |
| Jadwal Perkuliahan | 40     | 1.97s     | ✅     |
| **Ruangan**        | **34** | **1.71s** | ✅     |

**Next**: Continue to Module 9 (Jadwal Ujian, Kalender Pendidikan, etc.)

## 🔍 Model Details

**Ruangan** (`app/Models/Ruangan.php`):

- ✅ HasFactory trait
- ✅ SoftDeletes trait
- **Fillable (12)**: kode_ruangan, nama_ruangan, gedung, lantai, no_ruang, jenis_ruangan, kapasitas, fasilitas, jam_buka, jam_tutup, keterangan, is_active
- **Casts**:
  - is_active → boolean
  - jam_buka → datetime:H:i
  - jam_tutup → datetime:H:i
- **Relationship**: hasMany(JadwalPerkuliahan)
- **Scope**: active()
- **Static Methods**: generateKodeRuangan(), parseKodeRuangan()
- **Accessor**: getWaktuOperasionalAttribute()

## 🚀 Performance

**100 Ruangan Load**: < 2 seconds ✅  
**Clean Code**: No N+1 queries detected  
**First Run Success**: All tests passing without debugging

---

**Verified**: All tests passing, code formatted, ready for next module! 🎉
