# Testing Documentation - Dosen Role

**Role**: Dosen (Lecturer)  
**Purpose**: Testing untuk fitur-fitur yang digunakan oleh Dosen  
**Total Modules**: 6  
**Completed**: 2/6 (33%)  
**Tests**: 43 tests (all passing ✅)

---

## 📚 Module List

### 1. [Jadwal Mengajar](01_JADWAL_MENGAJAR.md) ✅

**Status**: ✅ [COMPLETE]  
**Route**: `/siakad/dosen/jadwal-mengajar`  
**Tests**: 23 tests, 49 assertions  
**Features**:

- View jadwal mengajar (with dual email lookup)
- Buka/tutup presensi (with geo-restriction)
- Manage sesi kelas (with attendance statistics)
- View mahasiswa per kelas (approved only)

**Key Patterns**:

- Dual email lookup (email_institusi OR email_pribadi)
- Active periode with fallback to latest
- withCount for attendance statistics
- Geo-restriction (configurable)
- Graceful degradation (empty collection when dosen not found)

### 2. [Presensi Mahasiswa](02_PRESENSI_MAHASISWA.md) ✅

**Status**: ✅ [COMPLETE]  
**Route**: `/siakad/dosen/presensi-mahasiswa`  
**Tests**: 20 tests, 52 assertions  
**Features**:

- Auto-generate presensi for enrolled students
- Bulk input presensi (multiple students)
- waktu_absen logic (set for Hadir, clear for others)
- Statistics calculation (total, hadir, izin, sakit, alpa, percentage)

**Key Patterns**:

- Auto-generation with firstOrCreate (no duplicates)
- Only approved KRS (status: Disetujui)
- DB transaction for bulk updates
- Security: Skip presensi from different session
- Division by zero protection (percentage calculation)

---

### 2. [Presensi Mahasiswa](02_PRESENSI_MAHASISWA.md) ⏳

**Status**: ⏳ [PENDING]  
**Route**: `/siakad/dosen/presensi-mahasiswa`  
**Features**:

- Input presensi per sesi
- Bulk input (mark all)
- Track attendance percentage
- Export rekap presensi

---

### 3. [Mata Kuliah Detail](03_MATA_KULIAH_DETAIL.md) ⏳

**Status**: ⏳ [PENDING]  
**Route**: `/siakad/dosen/mata-kuliah/{id}`  
**Features**:

- Manage materi SAP
- Create & grade tugas
- Setup komponen penilaian
- Input nilai mahasiswa
- Export nilai to Excel

---

### 4. [Validasi KRS](04_VALIDASI_KRS.md) ⏳

**Status**: ⏳ [PENDING]  
**Route**: `/siakad/dosen/validasi-krs`  
**Features**:

- View KRS mahasiswa bimbingan (Dosen PA)
- Approve/reject KRS
- Validate max SKS, prerequisites
- Partial approval per mata kuliah

---

### 5. [Log Bimbingan](05_LOG_BIMBINGAN.md) ⏳

**Status**: ⏳ [PENDING]  
**Route**: `/siakad/dosen/log-bimbingan`  
**Features**:

- Create log bimbingan mahasiswa
- Track by topik (Akademik, Skripsi, Karir, dll)
- Follow-up status
- Reports & statistics

---

## 🎯 Testing Priority

### High Priority (Must Test First)

1. **Jadwal Mengajar** - Core feature, most used
2. **Presensi Mahasiswa** - Critical for attendance tracking
3. **Mata Kuliah Detail** - Nilai input is critical

### Medium Priority

4. **Validasi KRS** - Important but only during KRS period

### Low Priority

5. **Log Bimbingan** - Important but not time-sensitive

---

## 🔐 Authorization Matrix

| Module             | Role  | Can View | Can Create | Can Edit | Can Delete |
| ------------------ | ----- | -------- | ---------- | -------- | ---------- |
| Jadwal Mengajar    | Dosen | Own      | No         | No       | No         |
| Presensi Mahasiswa | Dosen | Own      | Yes        | Limited  | No         |
| Mata Kuliah Detail | Dosen | Own      | Yes        | Yes      | Yes        |
| Validasi KRS       | Dosen | Mentees  | No         | No       | No         |
| Log Bimbingan      | Dosen | Own      | Yes        | No       | No         |

**Notes**:

- **Own**: Hanya data dosen yang login
- **Mentees**: Mahasiswa bimbingan (dosen_pa_id)
- **Limited**: Hanya dalam periode tertentu (24 jam setelah sesi)

---

## 🧪 Common Test Patterns

### 1. Authorization Test Template

```php
it('dosen can only access own data', function () {
    $dosen1 = Dosen::factory()->create();
    $dosen2 = Dosen::factory()->create();

    $jadwal = JadwalPerkuliahan::factory()->create([
        'formasi_dosen_id' => FormasiDosen::factory()->create([
            'dosen_id' => $dosen2->id
        ])
    ]);

    $response = actingAs($dosen1->user)
        ->get(route('dosen.jadwal-mengajar.show', $jadwal->id));

    $response->assertForbidden();
});
```

### 2. Validation Test Template

```php
it('validates required fields', function () {
    $dosen = Dosen::factory()->create();

    $response = actingAs($dosen->user)
        ->post(route('dosen.log-bimbingan.store'), []);

    $response->assertInvalid(['mahasiswa_id', 'tanggal', 'topik']);
});
```

### 3. Integration Test Template

```php
it('creates presensi and updates attendance stats', function () {
    $dosen = Dosen::factory()->create();
    $mahasiswa = Mahasiswa::factory()->create();
    $sesi = SesiKelas::factory()->create(['is_open' => true]);

    actingAs($dosen->user)
        ->post(route('dosen.presensi-mahasiswa.store'), [
            'sesi_id' => $sesi->id,
            'mahasiswa_id' => $mahasiswa->id,
            'status' => 'Hadir',
        ]);

    assertDatabaseHas('presensi_mahasiswa', [
        'mahasiswa_id' => $mahasiswa->id,
        'status' => 'Hadir',
    ]);

    expect($mahasiswa->fresh()->attendancePercentage())->toBe(100.0);
});
```

---

## 📊 Test Coverage Goals

- **Unit Tests**: 70% coverage
- **Feature Tests**: 80% coverage
- **Integration Tests**: 60% coverage

---

## 🔗 Related Documentation

- [Admin Testing Docs](../ADMIN/README.md)
- [Mahasiswa Testing Docs](../MAHASISWA/README.md) (if exists)
- [System Architecture](../../../SYSTEM_ARCHITECTURE.md)
- [Developer Guide](../../../DEVELOPER_GUIDE.md)

---

**Last Updated**: 2025-01-17  
**Created By**: AI Assistant  
**Status**: Ready for Implementation
