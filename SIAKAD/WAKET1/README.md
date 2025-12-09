# Testing Documentation: Waket1 (Wakil Ketua 1 - Bidang Akademik)

## Overview

Dokumentasi testing ini mencakup semua modul yang dapat diakses oleh role **Waket1 (Wakil Ketua 1 - Bidang Akademik)** dalam sistem SIAKAD. Waket1 memiliki tanggung jawab utama dalam:

- **Monitoring Akademik**: Mengawasi performa akademik mahasiswa (IPK, IPS, SKS lulus)
- **Approval System**: Menyetujui/menolak revisi nilai dari dosen
- **Kebijakan Akademik**: Mengelola bobot nilai (grading system) per program studi
- **Komunikasi**: Membuat dan mengelola pengumuman akademik untuk civitas akademika

---

## Module List

### 1. 🟢 Dashboard (Medium Priority)

**File**: [01_DASHBOARD.md](./01_DASHBOARD.md)  
**Route**: `/waket1/dashboard`  
**Fitur Utama**:

- Statistik akademik (Total Prodi, Mahasiswa Aktif, Dosen Aktif)
- Distribusi IPK mahasiswa dengan chart
- Alert mahasiswa berisiko (IPK < 2.00, IPS < 2.00)
- Quick stats revisi nilai pending
- Kalender akademik periode aktif
- Recent activities log

### 2. 🔴 Monitoring Nilai & IPK (Critical Priority)

**File**: [02_MONITORING_NILAI_IPK.md](./02_MONITORING_NILAI_IPK.md)  
**Route**: `/waket1/monitoring`  
**Fitur Utama**:

- Filter & search mahasiswa (Program Studi, Angkatan, Status Akademik)
- Display IPK, IPS, SKS lulus, predikat per mahasiswa
- Highlight mahasiswa berisiko dengan visual warning
- Sorting multi-column (IPK, IPS, Nama)
- Detail perkembangan IPS per semester dengan grafik
- Export Excel & PDF dengan statistik summary

### 3. 🔴 Revisi Nilai - Approval (Critical Priority)

**File**: [03_REVISI_NILAI.md](./03_REVISI_NILAI.md)  
**Route**: `/waket1/revisi-nilai`  
**Fitur Utama**:

- List revisi nilai yang diajukan dosen (status: Pending/Disetujui/Ditolak)
- Filter & search (Status, Program Studi, Dosen Pengaju)
- Approve/Reject revisi dengan catatan
- Auto-update nilai mahasiswa & recalculate IPK saat approved
- Bulk approve/reject multiple revisi
- Notifikasi real-time ke dosen & mahasiswa

### 4. 🟡 Bobot Nilai (High Priority)

**File**: [04_BOBOT_NILAI.md](./04_BOBOT_NILAI.md)  
**Route**: `/waket1/bobot-nilai`  
**Fitur Utama**:

- CRUD bobot nilai (Grade: A, AB, B, BC, C, D, E)
- Set nilai minimum, maksimum, dan bobot per grade
- Template standar (quick setup) dengan auto-suggest
- Apply template ke multiple program studi
- Validasi no overlap & no gap range nilai
- Impact analysis: analisis dampak perubahan ke mahasiswa existing
- Export/Import Excel untuk bulk management

### 5. 🟢 Pengumuman Akademik (Medium Priority)

**File**: [05_PENGUMUMAN_AKADEMIK.md](./05_PENGUMUMAN_AKADEMIK.md)  
**Route**: `/waket1/pengumuman`  
**Fitur Utama**:

- CRUD pengumuman dengan rich text editor
- Kategori: Akademik, Beasiswa, Event, Pengumuman Umum, Penting
- Prioritas: Biasa, Penting, Mendesak (dengan visual indicator)
- Target audience: Semua, Program Studi Tertentu, Angkatan Tertentu
- Upload file lampiran & link eksternal
- Scheduled publishing (Draft → Aktif → Kadaluarsa)
- Kirim notifikasi email & tampilkan di banner homepage
- View tracking & engagement metrics

---

## Testing Priority

### 🔴 Critical (Must Test First)

1. **Monitoring Nilai & IPK** - Core function untuk monitoring performa akademik
2. **Revisi Nilai** - Approval system yang berdampak langsung ke nilai mahasiswa

### 🟡 High (Test Second)

3. **Bobot Nilai** - Setting fundamental untuk grading system

### 🟢 Medium (Test Last)

4. **Dashboard** - Overview yang menggabungkan data dari modul lain
5. **Pengumuman Akademik** - Communication tool, tidak berdampak langsung ke akademik

---

## Authorization Matrix

| Module           | View | Create | Edit | Delete | Approve/Reject | Notes                |
| ---------------- | ---- | ------ | ---- | ------ | -------------- | -------------------- |
| Dashboard        | ✅   | ❌     | ❌   | ❌     | ❌             | Read-only dashboard  |
| Monitoring Nilai | ✅   | ❌     | ❌   | ❌     | ❌             | Read-only monitoring |
| Revisi Nilai     | ✅   | ❌     | ❌   | ❌     | ✅             | Only approve/reject  |
| Bobot Nilai      | ✅   | ✅     | ✅   | ✅     | ❌             | Full CRUD access     |
| Pengumuman       | ✅   | ✅     | ✅   | ✅     | ❌             | Full CRUD access     |

**Key Authorization Rules**:

- ✅ Role `waket1` diperlukan untuk semua operasi
- ❌ Role lain (dosen, mahasiswa, admin) tidak dapat mengakses module Waket1
- 🔒 Revisi Nilai: Hanya approve/reject, tidak bisa create/edit revisi (itu hak dosen)
- 🔓 Bobot Nilai & Pengumuman: Full control karena Waket1 yang mengatur kebijakan

---

## Common Test Patterns

### 1. Authorization Test Pattern

Semua module Waket1 harus test:

```php
test('only waket1 can access', function () {
    // Given: User dengan role waket1
    $waket1 = User::factory()->create();
    $waket1->assignRole('waket1');

    // When: Access route
    $response = actingAs($waket1)->get('/waket1/dashboard');

    // Then: Success
    $response->assertOk();
});

test('non-waket1 cannot access', function () {
    // Given: User dengan role lain
    $dosen = User::factory()->create();
    $dosen->assignRole('dosen');

    // When: Access route waket1
    $response = actingAs($dosen)->get('/waket1/dashboard');

    // Then: Forbidden
    $response->assertForbidden();
});
```

### 2. Approval Flow Test Pattern

Untuk modul Revisi Nilai:

```php
test('waket1 can approve revisi nilai', function () {
    // Given: Revisi nilai dengan status Pending
    $revisi = RevisiNilai::factory()->create(['status' => 'Pending']);
    $waket1 = User::factory()->create();
    $waket1->assignRole('waket1');

    // When: Approve revisi
    $response = actingAs($waket1)
        ->post("/waket1/revisi-nilai/{$revisi->id}/approve", [
            'catatan' => 'Disetujui karena alasan valid'
        ]);

    // Then: Status berubah, nilai mahasiswa ter-update
    $response->assertRedirect();
    expect($revisi->fresh()->status)->toBe('Disetujui');
    expect($revisi->fresh()->approved_by)->toBe($waket1->id);
    // Check nilai mahasiswa ter-update
});
```

### 3. Filter & Search Test Pattern

Untuk modul dengan filtering:

```php
test('can filter by program studi', function () {
    // Given: Data mahasiswa di multiple program studi
    $prodiA = ProgramStudi::factory()->create();
    $prodiB = ProgramStudi::factory()->create();
    Mahasiswa::factory()->count(5)->create(['program_studi_id' => $prodiA->id]);
    Mahasiswa::factory()->count(3)->create(['program_studi_id' => $prodiB->id]);

    $waket1 = User::factory()->create();
    $waket1->assignRole('waket1');

    // When: Filter by program studi A
    $response = actingAs($waket1)
        ->get('/waket1/monitoring?program_studi_id=' . $prodiA->id);

    // Then: Hanya mahasiswa prodi A yang muncul
    $response->assertOk();
    $mahasiswa = $response->viewData('mahasiswa');
    expect($mahasiswa)->toHaveCount(5);
});
```

### 4. Impact Analysis Test Pattern

Untuk modul Bobot Nilai:

```php
test('shows impact analysis before updating bobot', function () {
    // Given: Bobot nilai existing dengan nilai mahasiswa terkait
    $bobotNilai = BobotNilai::factory()->create([
        'grade' => 'A',
        'bobot' => 4.00
    ]);
    // Mahasiswa dengan nilai A
    NilaiMahasiswa::factory()->count(10)->create(['grade' => 'A']);

    $waket1 = User::factory()->create();
    $waket1->assignRole('waket1');

    // When: Ubah bobot A dari 4.00 menjadi 3.80
    $response = actingAs($waket1)
        ->patch("/waket1/bobot-nilai/{$bobotNilai->id}", [
            'bobot' => 3.80
        ]);

    // Then: Warning muncul karena 10 mahasiswa terpengaruh
    // Require confirmation untuk proceed
});
```

---

## Test Coverage Goals

- **Unit Tests**: 60% coverage - Focus pada business logic (calculation, validation)
- **Feature Tests**: 90% coverage - Semua user flow harus di-test (CRUD, approval, filter)
- **Integration Tests**: 70% coverage - Test interaksi antar modul (nilai → IPK, revisi → KHS)

**Priority Coverage**:

1. **Authorization**: 100% - Critical untuk security
2. **Approval Flow**: 95% - Berdampak langsung ke nilai mahasiswa
3. **Data Validation**: 90% - Prevent invalid data masuk database
4. **Filter & Search**: 80% - Important untuk user experience
5. **Export & Print**: 70% - Nice to have

---

## User Flow Testing

### Typical Waket1 Daily Flow

1. **Morning Check**:

   - Login → Dashboard → Lihat revisi nilai pending
   - Lihat alert mahasiswa berisiko baru

2. **Approval Process**:

   - Buka halaman Revisi Nilai
   - Filter status "Pending"
   - Review detail revisi (lihat nilai lama vs baru, alasan dosen)
   - Approve atau Reject dengan catatan
   - Bulk approve jika banyak revisi valid

3. **Monitoring**:

   - Buka halaman Monitoring Nilai & IPK
   - Filter mahasiswa berisiko (IPK < 2.00)
   - View detail perkembangan mahasiswa tertentu
   - Export Excel untuk report ke Rektor

4. **Policy Setting** (Periodic):

   - Buka halaman Bobot Nilai
   - Review atau update bobot nilai per program studi
   - Apply template standar untuk program studi baru

5. **Communication**:
   - Buat pengumuman akademik (beasiswa, event, deadline)
   - Set target audience dan prioritas
   - Publish dan kirim notifikasi email

---

## Critical Test Scenarios

### Scenario 1: Approve Revisi Nilai & Verify IPK Update

**Steps**:

1. Dosen mengajukan revisi nilai mahasiswa A (B → A)
2. Waket1 login dan lihat notifikasi revisi baru
3. Waket1 buka detail revisi, lihat alasan valid
4. Waket1 approve revisi
5. **Verify**: Nilai mahasiswa ter-update di database
6. **Verify**: IPK mahasiswa ter-recalculate dengan benar
7. **Verify**: IPS semester terkait ter-recalculate
8. **Verify**: Mahasiswa dapat melihat nilai baru di KHS
9. **Verify**: Dosen dapat melihat status "Disetujui"

### Scenario 2: Reject Revisi & Notify Dosen

**Steps**:

1. Dosen mengajukan revisi nilai tanpa alasan yang jelas
2. Waket1 login dan review revisi
3. Waket1 reject revisi dengan alasan penolakan
4. **Verify**: Status revisi = "Ditolak"
5. **Verify**: Nilai mahasiswa TIDAK berubah (tetap nilai lama)
6. **Verify**: Dosen dapat melihat status "Ditolak" dengan alasan
7. **Verify**: Notifikasi email terkirim ke dosen

### Scenario 3: Update Bobot Nilai & Impact Analysis

**Steps**:

1. Waket1 buka halaman Bobot Nilai
2. Waket1 edit bobot nilai grade "A" dari 4.00 → 3.80
3. System tampilkan impact analysis: 50 mahasiswa terpengaruh
4. Waket1 confirm perubahan
5. **Verify**: Bobot nilai ter-update di database
6. **Verify**: Background job recalculate IPK 50 mahasiswa (async)
7. **Verify**: Log perubahan disimpan untuk audit trail
8. **Verify**: Mahasiswa dapat melihat IPK baru setelah recalculate

---

## Integration Points

### With Admin Modules

- **Master Data Mahasiswa**: Monitoring menggunakan data mahasiswa aktif
- **Master Data Program Studi**: Filter & grouping berdasarkan program studi
- **Kalender Pendidikan**: Dashboard menampilkan periode akademik aktif

### With Dosen Modules

- **Mata Kuliah Detail**: Revisi nilai diajukan dari modul input nilai dosen
- **Dosen notification**: Notifikasi hasil approval/reject revisi

### With Mahasiswa Modules

- **KHS**: Nilai ter-update di KHS setelah revisi di-approve
- **Mahasiswa notification**: Notifikasi saat nilai berubah atau ada pengumuman baru

---

## Related Documentation

### Main Guides

- [WAKET1_GUIDE.md](../../../WAKET1_GUIDE.md) - User guide lengkap untuk Waket1
- [BOBOT_NILAI_GUIDE.md](../../../BOBOT_NILAI_GUIDE.md) - Guide lengkap untuk manage bobot nilai
- [SYSTEM_ARCHITECTURE.md](../../../SYSTEM_ARCHITECTURE.md) - Architecture overview

### Admin Modules (Reference)

- [../ADMIN/04_MASTER_DATA_MAHASISWA.md](../ADMIN/04_MASTER_DATA_MAHASISWA.md)
- [../ADMIN/09_KALENDER_PENDIDIKAN.md](../ADMIN/09_KALENDER_PENDIDIKAN.md)
- [../ADMIN/13_PRESENSI_MAHASISWA.md](../ADMIN/13_PRESENSI_MAHASISWA.md)

### Dosen Modules (Reference)

- [../DOSEN/03_MATA_KULIAH_DETAIL.md](../DOSEN/03_MATA_KULIAH_DETAIL.md) - Dosen input nilai

### Mahasiswa Modules (Reference)

- [../MAHASISWA/02_KHS.md](../MAHASISWA/02_KHS.md) - Mahasiswa view grades

---

## Notes for Testers

1. **Data Setup**: Sebelum test Waket1 modules, pastikan ada:

   - Program Studi dengan bobot nilai ter-setup
   - Mahasiswa aktif dengan nilai existing
   - Revisi nilai pending dari dosen

2. **Test Order**: Test dengan urutan Critical → High → Medium untuk maximize value

3. **Performance**: Waket1 modules sering query large datasets (semua mahasiswa), test performa dengan data > 1000 records

4. **Security**: Extra attention pada authorization testing - Waket1 punya access high-privilege operations

5. **Email Testing**: Use mail trap (Mailtrap.io atau MailHog) untuk test notifikasi email

---

**Last Updated**: 2025-12-09  
**Total Modules**: 5  
**Total Test Cases**: 200+ checklist items
