# Testing Documentation: Ketua (Ketua Sekolah Tinggi)

## Overview

Dokumentasi testing ini mencakup semua modul yang dapat diakses oleh role **Ketua (Ketua Sekolah Tinggi)** dalam sistem SIAKAD. Ketua memiliki tanggung jawab tertinggi dalam institusi dengan fokus utama pada:

- **Kepemimpinan Institusi**: Mengawasi seluruh operasional akademik dan non-akademik
- **Final Approval**: Memberikan persetujuan akhir untuk yudisium dan kelulusan mahasiswa
- **Strategic Oversight**: Monitoring KPI institusi dan pengambilan keputusan strategis
- **Quality Assurance**: Memastikan standar akademik dan kualitas lulusan terjaga

---

## Module List

### 1. 🟢 Dashboard (Medium Priority)

**File**: [01_DASHBOARD.md](./01_DASHBOARD.md)  
**Route**: `/ketua/dashboard`  
**Fitur Utama**:

- Statistik institusi menyeluruh (Total Prodi, Mahasiswa, Dosen)
- Quick stats approval yudisium pending dengan badge alert
- Distribusi mahasiswa dan dosen per program studi
- Chart trend mahasiswa aktif dan kelulusan per tahun
- KPI institusi: Akreditasi, Tingkat Kelulusan, Rata-rata IPK, Rasio Dosen:Mahasiswa
- Kalender akademik periode aktif
- Recent activities institusi

### 2. 🔴 Approval Yudisium & KHS (Critical Priority)

**File**: [02_APPROVAL_YUDISIUM.md](./02_APPROVAL_YUDISIUM.md)  
**Route**: `/ketua/approval`  
**Fitur Utama**:

- List mahasiswa yang mengajukan yudisium (Pending/Disetujui/Ditolak)
- Filter & search (Status, Program Studi, Predikat)
- Detail mahasiswa: Transkrip nilai lengkap, IPK, SKS, masa studi
- Verifikasi kelengkapan: Checklist syarat yudisium (IPK, SKS, no tunggakan)
- **Approve**: Update status mahasiswa → "Lulus", generate nomor ijazah, cetak transkrip & SKL
- **Reject**: Tolak dengan alasan, kirim notifikasi ke mahasiswa untuk perbaikan
- Bulk approve multiple mahasiswa sekaligus
- Notifikasi real-time & email ke mahasiswa dan Kaprodi

---

## Testing Priority

### 🔴 Critical (Must Test First)

1. **Approval Yudisium** - Final authority untuk kelulusan mahasiswa, berdampak langsung ke status akademik

### 🟢 Medium (Test Second)

2. **Dashboard** - Overview institusi, tidak berdampak langsung tapi penting untuk monitoring

---

## Authorization Matrix

| Module            | View | Approve | Reject | Generate Dokumen | Notes                           |
| ----------------- | ---- | ------- | ------ | ---------------- | ------------------------------- |
| Dashboard         | ✅   | ❌      | ❌     | ❌               | Read-only dashboard             |
| Approval Yudisium | ✅   | ✅      | ✅     | ✅               | Final authority untuk kelulusan |

**Key Authorization Rules**:

- ✅ Role `ketua` diperlukan untuk semua operasi
- ❌ Role lain (waket1, dosen, mahasiswa, admin) tidak dapat mengakses module Ketua
- 🔒 **Highest Authority**: Ketua adalah final approver untuk yudisium (tidak ada approval di atasnya)
- 🔓 **Cannot Edit**: Ketua tidak bisa edit data mahasiswa/nilai, hanya approve/reject yudisium

---

## Common Test Patterns

### 1. Authorization Test Pattern

```php
test('only ketua can access approval', function () {
    // Given: User dengan role ketua
    $ketua = User::factory()->create();
    $ketua->assignRole('ketua');

    // When: Access route ketua
    $response = actingAs($ketua)->get('/ketua/approval');

    // Then: Success
    $response->assertOk();
});

test('non-ketua cannot access', function () {
    // Given: User dengan role lain
    $waket1 = User::factory()->create();
    $waket1->assignRole('waket1');

    // When: Access route ketua
    $response = actingAs($waket1)->get('/ketua/approval');

    // Then: Forbidden
    $response->assertForbidden();
});
```

### 2. Approval Flow Test Pattern

```php
test('ketua can approve yudisium and update mahasiswa status', function () {
    // Given: Approval yudisium dengan status Pending
    $mahasiswa = Mahasiswa::factory()->create([
        'status_akademik' => 'Aktif',
        'ipk' => 3.65
    ]);
    $approval = ApprovalYudisium::factory()->create([
        'mahasiswa_id' => $mahasiswa->id,
        'status' => 'Pending'
    ]);

    $ketua = User::factory()->create();
    $ketua->assignRole('ketua');

    // When: Approve yudisium
    $response = actingAs($ketua)
        ->post("/ketua/approval/{$approval->id}/approve", [
            'catatan' => 'Memenuhi syarat yudisium'
        ]);

    // Then: Status berubah, mahasiswa jadi Lulus
    $response->assertRedirect();
    expect($approval->fresh()->status)->toBe('Disetujui');
    expect($approval->fresh()->approved_by)->toBe($ketua->id);
    expect($mahasiswa->fresh()->status_akademik)->toBe('Lulus');
    expect($mahasiswa->fresh()->tanggal_lulus)->not->toBeNull();
    expect($mahasiswa->fresh()->predikat_kelulusan)->toBe('Cum Laude');
    expect($mahasiswa->fresh()->nomor_ijazah)->not->toBeNull();
});
```

### 3. Rejection Flow Test Pattern

```php
test('ketua can reject yudisium with reason', function () {
    // Given: Approval yudisium dengan status Pending
    $mahasiswa = Mahasiswa::factory()->create(['status_akademik' => 'Aktif']);
    $approval = ApprovalYudisium::factory()->create([
        'mahasiswa_id' => $mahasiswa->id,
        'status' => 'Pending'
    ]);

    $ketua = User::factory()->create();
    $ketua->assignRole('ketua');

    // When: Reject yudisium
    $response = actingAs($ketua)
        ->post("/ketua/approval/{$approval->id}/reject", [
            'alasan' => 'Masih ada tunggakan administrasi yang belum diselesaikan'
        ]);

    // Then: Status approval = Ditolak, mahasiswa tetap Aktif
    $response->assertRedirect();
    expect($approval->fresh()->status)->toBe('Ditolak');
    expect($approval->fresh()->rejected_by)->toBe($ketua->id);
    expect($approval->fresh()->alasan_penolakan)->toContain('tunggakan');
    expect($mahasiswa->fresh()->status_akademik)->toBe('Aktif'); // Tidak berubah
    expect($mahasiswa->fresh()->tanggal_lulus)->toBeNull();
});
```

### 4. Validation Test Pattern

```php
test('cannot approve if ipk below minimum', function () {
    // Given: Mahasiswa dengan IPK < 2.00
    $mahasiswa = Mahasiswa::factory()->create(['ipk' => 1.85]);
    $approval = ApprovalYudisium::factory()->create([
        'mahasiswa_id' => $mahasiswa->id,
        'status' => 'Pending'
    ]);

    $ketua = User::factory()->create();
    $ketua->assignRole('ketua');

    // When: Try to approve
    $response = actingAs($ketua)
        ->post("/ketua/approval/{$approval->id}/approve");

    // Then: Validation error
    $response->assertSessionHasErrors();
    expect($approval->fresh()->status)->toBe('Pending'); // Tidak berubah
});
```

---

## Test Coverage Goals

- **Unit Tests**: 50% coverage - Focus pada calculation logic (IPK, predikat, masa studi)
- **Feature Tests**: 95% coverage - Semua approval flow harus di-test (approve, reject, validation)
- **Integration Tests**: 80% coverage - Test interaksi dengan modul lain (mahasiswa status, notifikasi, dokumen)

**Priority Coverage**:

1. **Authorization**: 100% - Critical untuk security (hanya Ketua yang bisa approve)
2. **Approval Flow**: 95% - Berdampak langsung ke status kelulusan mahasiswa
3. **Validation**: 90% - Prevent invalid approval (IPK, SKS, tunggakan)
4. **Document Generation**: 85% - Ijazah, transkrip, SKL harus ter-generate dengan benar
5. **Notifikasi**: 80% - Important untuk inform stakeholders

---

## User Flow Testing

### Typical Ketua Daily Flow

1. **Morning Check**:

   - Login → Dashboard → Lihat approval yudisium pending dengan badge alert
   - Review KPI institusi (akreditasi, tingkat kelulusan, rasio dosen:mahasiswa)

2. **Approval Process**:

   - Buka halaman Approval Yudisium
   - Filter status "Pending"
   - Review detail mahasiswa satu per satu:
     - Lihat transkrip nilai lengkap
     - Cek IPK dan SKS lulus
     - Verifikasi checklist kelengkapan (no tunggakan, dll)
   - **Decision**:
     - **Approve**: Jika semua syarat terpenuhi → Generate nomor ijazah, update status Lulus
     - **Reject**: Jika ada yang kurang → Tulis alasan penolakan untuk feedback ke mahasiswa
   - Bulk approve jika banyak mahasiswa yang sudah memenuhi syarat

3. **Monitoring**:

   - Lihat statistik kelulusan bulan ini
   - Review rata-rata IPK lulusan
   - Export laporan untuk rapat dengan Rektor

4. **Follow-up**:
   - Notifikasi email otomatis terkirim ke mahasiswa (approval/rejection)
   - Notifikasi juga ke Kaprodi untuk koordinasi wisuda

---

## Critical Test Scenarios

### Scenario 1: Approve Yudisium & Generate Complete Documents

**Steps**:

1. Mahasiswa dengan IPK 3.65 (Cum Laude) mengajukan yudisium
2. Admin/Akademik membuat approval yudisium (status Pending)
3. Ketua login dan buka halaman approval
4. Ketua klik "Detail" untuk review transkrip mahasiswa
5. Ketua verifikasi semua checklist terpenuhi (✓ IPK ≥ 2.00, ✓ SKS 144, ✓ No tunggakan)
6. Ketua klik "Approve" dan confirm dengan catatan
7. **Verify**: Status approval = "Disetujui"
8. **Verify**: Mahasiswa status_akademik = "Lulus"
9. **Verify**: Tanggal lulus ter-set dengan timestamp approval
10. **Verify**: Predikat kelulusan = "Cum Laude"
11. **Verify**: Nomor ijazah ter-generate (format: NOMOR/TAHUN/PRODI)
12. **Verify**: Transkrip PDF ter-generate dan tersimpan di storage
13. **Verify**: Surat Keterangan Lulus PDF ter-generate
14. **Verify**: Notifikasi email terkirim ke mahasiswa dengan ucapan selamat
15. **Verify**: Notifikasi juga terkirim ke Kaprodi

### Scenario 2: Reject Yudisium & Notify for Correction

**Steps**:

1. Mahasiswa dengan IPK 2.85 tapi masih punya tunggakan perpustakaan mengajukan yudisium
2. Admin membuat approval yudisium (status Pending)
3. Ketua login dan review detail mahasiswa
4. Ketua lihat checklist "Tidak Ada Tunggakan Perpustakaan" = ✗ (tidak terpenuhi)
5. Ketua klik "Reject" dan tulis alasan: "Masih ada tunggakan 3 buku di perpustakaan"
6. Ketua confirm rejection
7. **Verify**: Status approval = "Ditolak"
8. **Verify**: Mahasiswa status_akademik tetap "Aktif" (tidak berubah)
9. **Verify**: Alasan penolakan tersimpan di database
10. **Verify**: Notifikasi email terkirim ke mahasiswa dengan detail alasan
11. **Verify**: Mahasiswa dapat melihat alasan rejection di dashboard mereka
12. **Verify**: Setelah mahasiswa selesaikan tunggakan, bisa mengajukan yudisium ulang

### Scenario 3: Bulk Approve Multiple Mahasiswa

**Steps**:

1. Ada 10 mahasiswa dengan status approval "Pending"
2. Semua mahasiswa sudah memenuhi syarat yudisium
3. Ketua login dan buka halaman approval
4. Ketua select checkbox untuk 10 mahasiswa
5. Ketua klik "Approve Selected"
6. Modal konfirmasi bulk approve muncul dengan list 10 mahasiswa
7. Ketua confirm bulk approval
8. **Verify**: Background job process 10 approval secara asynchronous
9. **Verify**: Semua 10 approval status = "Disetujui"
10. **Verify**: Semua 10 mahasiswa status = "Lulus"
11. **Verify**: Nomor ijazah ter-generate untuk semua (unik, tidak ada duplikat)
12. **Verify**: Dokumen (transkrip + SKL) ter-generate untuk semua 10 mahasiswa
13. **Verify**: Notifikasi email terkirim ke semua 10 mahasiswa
14. **Verify**: Success message: "10 approval berhasil disetujui"

---

## Integration Points

### With Waket1 Modules

- **Monitoring Nilai**: Data IPK dan predikat mahasiswa digunakan untuk approval yudisium
- **Bobot Nilai**: Predikat kelulusan dihitung berdasarkan bobot nilai yang diset Waket1

### With Admin Modules

- **Master Data Mahasiswa**: Approval update status mahasiswa dari "Aktif" → "Lulus"
- **Manajemen Yudisium**: Admin membuat approval yudisium, Ketua yang approve
- **Kalender Pendidikan**: Periode yudisium diatur di kalender akademik

### With Mahasiswa Modules

- **KHS**: Transkrip nilai mahasiswa digunakan untuk verifikasi kelengkapan
- **Dashboard Mahasiswa**: Notifikasi approval/rejection muncul di dashboard mahasiswa

---

## Related Documentation

### Main Guides

- [SYSTEM_ARCHITECTURE.md](../../../SYSTEM_ARCHITECTURE.md) - Architecture overview
- [DATABASE_SCHEMA.md](../../../DATABASE_SCHEMA.md) - Database structure

### Admin Modules (Reference)

- [../ADMIN/04_MASTER_DATA_MAHASISWA.md](../ADMIN/04_MASTER_DATA_MAHASISWA.md)
- [../ADMIN/16_MANAJEMEN_YUDISIUM.md](../ADMIN/16_MANAJEMEN_YUDISIUM.md)
- [../ADMIN/09_KALENDER_PENDIDIKAN.md](../ADMIN/09_KALENDER_PENDIDIKAN.md)

### Waket1 Modules (Reference)

- [../WAKET1/02_MONITORING_NILAI_IPK.md](../WAKET1/02_MONITORING_NILAI_IPK.md)
- [../WAKET1/04_BOBOT_NILAI.md](../WAKET1/04_BOBOT_NILAI.md)

### Mahasiswa Modules (Reference)

- [../MAHASISWA/02_KHS.md](../MAHASISWA/02_KHS.md)

---

## Notes for Testers

1. **Data Setup**: Sebelum test Ketua modules, pastikan ada:

   - Mahasiswa dengan status "Aktif" dan IPK ≥ 2.00
   - Approval yudisium dengan status "Pending"
   - Bobot nilai ter-setup untuk calculate predikat
   - Kalender akademik dengan periode yudisium aktif

2. **Test Order**:

   - Test Approval Yudisium first (critical)
   - Test Dashboard second (dependency pada data approval)

3. **Performance**:

   - Test bulk approve dengan 50+ mahasiswa untuk ensure queue job berfungsi
   - Test generate dokumen tidak blocking (async job)

4. **Security**:

   - **Extra critical**: Pastikan HANYA Ketua yang bisa approve yudisium
   - Test concurrent approval (dua Ketua approve bersamaan) dengan locking mechanism

5. **Document Generation**:

   - Test PDF generation (transkrip, SKL) dengan format resmi
   - Test signature & QR code untuk verifikasi dokumen
   - Test storage path terstruktur (per tahun lulus, per program studi)

6. **Email Testing**:
   - Use mail trap (Mailtrap.io atau MailHog) untuk test notifikasi email
   - Test email template (HTML format, professional)
   - Test email content: ucapan selamat vs alasan rejection

---

## Business Rules Summary

### IPK & Predikat Mapping

- **Cum Laude**: IPK ≥ 3.50
- **Sangat Memuaskan**: IPK 3.00 - 3.49
- **Memuaskan**: IPK 2.50 - 2.99
- **Cukup**: IPK 2.00 - 2.49
- **Kurang**: IPK < 2.00 (tidak boleh lulus)

### Syarat Yudisium (Checklist)

1. ✓ Semua Mata Kuliah Wajib Lulus (tidak ada nilai E)
2. ✓ IPK ≥ 2.00
3. ✓ Total SKS Lulus ≥ 144 SKS (untuk S1, sesuai kurikulum)
4. ✓ Tidak Ada Tunggakan Administrasi (keuangan)
5. ✓ Tidak Ada Tunggakan Perpustakaan (semua buku dikembalikan)
6. ✓ Maksimal 1 nilai D di mata kuliah wajib (kebijakan institusi)

### Nomor Ijazah Format

**Format**: `[NOMOR]/[TAHUN]/[KODE_PRODI]`  
**Contoh**: `001/2024/TIF` (Mahasiswa ke-1 tahun 2024 dari Teknik Informatika)

---

**Last Updated**: 2025-12-09  
**Total Modules**: 2  
**Total Test Cases**: 170+ checklist items
