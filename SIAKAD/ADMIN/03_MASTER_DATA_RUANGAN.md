# Testing: Master Data Alokasi Ruang & Waktu

**Status**: ⚪ [TODO]  
**Role**: Admin  
**Route Prefix**: `/siakad/ruangan`  
**Controller**: `RuanganController`  
**Model**: `Ruangan`

---

## 📋 Daftar Testing

### 1. Functional Testing - CRUD Operations

#### 1.1 Index/List Ruangan

-   [ ] **Test**: Admin dapat melihat daftar ruangan
-   [ ] **Test**: Pagination berfungsi
-   [ ] **Test**: Filter berdasarkan gedung
-   [ ] **Test**: Filter berdasarkan kapasitas
-   [ ] **Test**: Filter berdasarkan status (Tersedia, Maintenance)
-   [ ] **Test**: Search berdasarkan kode/nama ruangan

#### 1.2 Create Ruangan

-   [ ] **Test**: Berhasil membuat ruangan dengan data lengkap

    -   Kode ruangan, nama, gedung, lantai, kapasitas
    -   Fasilitas (AC, Proyektor, Whiteboard, dll)
    -   Verifikasi tersimpan

-   [ ] **Test**: Upload foto ruangan (optional)
    -   Format: JPG, PNG
    -   Max size: 2MB
    -   Verifikasi file tersimpan di storage

#### 1.3 Show Ruangan

-   [ ] **Test**: Detail ruangan menampilkan info lengkap

    -   Data ruangan
    -   Fasilitas yang tersedia
    -   Jadwal penggunaan ruangan

-   [ ] **Test**: Calendar view untuk ketersediaan ruangan
    -   Week view
    -   Day view
    -   Verifikasi jadwal bentrok ter-highlight

#### 1.4 Edit Ruangan

-   [ ] **Test**: Update data ruangan
-   [ ] **Test**: Update fasilitas ruangan
-   [ ] **Test**: Update foto ruangan

#### 1.5 Delete Ruangan

-   [ ] **Test**: Soft delete ruangan kosong
-   [ ] **Test**: Tidak dapat delete jika ada jadwal aktif
-   [ ] **Test**: Error message jika ada constraint

---

### 2. Validation Testing

#### 2.1 Required Fields

-   [ ] **Test**: Kode ruangan wajib
-   [ ] **Test**: Nama ruangan wajib
-   [ ] **Test**: Gedung wajib
-   [ ] **Test**: Kapasitas wajib

#### 2.2 Format Validation

-   [ ] **Test**: Kode ruangan unik
-   [ ] **Test**: Kapasitas harus angka positif (min 1, max 500)
-   [ ] **Test**: Lantai harus angka (-2 sampai 20)
    -   Support basement (minus)
    -   Max 20 lantai

#### 2.3 File Upload Validation

-   [ ] **Test**: Foto harus format image (jpg, png, jpeg)
-   [ ] **Test**: Foto max 2MB
-   [ ] **Test**: Reject file exe, pdf, dll

---

### 3. Availability Checking (Real-time)

#### 3.1 API Check Availability

-   [ ] **Test**: API `/api/ruangan/available` berfungsi

    -   Parameter: hari, waktu_mulai, waktu_selesai
    -   Response: list ruangan available

-   [ ] **Test**: Check availability dengan filter kapasitas

    -   Min capacity 30 → hanya ruangan >= 30

-   [ ] **Test**: Check availability dengan filter fasilitas
    -   Butuh proyektor → filter ruangan punya proyektor

#### 3.2 Conflict Detection

-   [ ] **Test**: Detect jadwal bentrok

    -   Ruangan A jam 08:00-10:00 → tidak available
    -   Cek overlap time ranges

-   [ ] **Test**: Buffer time consideration
    -   Jadwal selesai 10:00, mulai 10:00 → bentrok
    -   Perlu buffer 15 menit

---

### 4. Integration Testing

#### 4.1 Relasi dengan Jadwal Perkuliahan

-   [ ] **Test**: Ruangan ter-assign ke jadwal
-   [ ] **Test**: Double booking prevention
-   [ ] **Test**: Cascade effect saat update kapasitas
    -   Kapasitas dikurangi < mahasiswa terdaftar → warning

#### 4.2 Relasi dengan Jadwal Ujian

-   [ ] **Test**: Ruangan untuk ujian
-   [ ] **Test**: Ujian dan kuliah tidak bentrok

---

### 5. Responsive Testing

-   [ ] **Test**: Tabel ruangan responsive
-   [ ] **Test**: Calendar view responsive di mobile
-   [ ] **Test**: Form create/edit responsive

---

### 6. UI/UX Testing

#### 6.1 Calendar/Schedule View

-   [ ] **Test**: Week calendar view
-   [ ] **Test**: Day calendar view
-   [ ] **Test**: Navigate antar minggu
-   [ ] **Test**: Click slot → detail jadwal

#### 6.2 Availability Indicator

-   [ ] **Test**: Badge "Tersedia" warna hijau
-   [ ] **Test**: Badge "Terpakai" warna merah
-   [ ] **Test**: Badge "Maintenance" warna kuning

---

### 7. Error Handling

-   [ ] **Test**: Handle file upload error
-   [ ] **Test**: Handle database constraint error
-   [ ] **Test**: Handle API timeout

---

### 8. Performance Testing

-   [ ] **Test**: Load 100+ ruangan tanpa lag
-   [ ] **Test**: Calendar rendering performance
-   [ ] **Test**: Real-time availability check < 500ms

---

### 9. Edge Cases

-   [ ] **Test**: Ruangan dengan nama sangat panjang
-   [ ] **Test**: Kapasitas 0 → error
-   [ ] **Test**: Lantai basement (-1, -2)
-   [ ] **Test**: Ruangan outdoor (tanpa lantai)
-   [ ] **Test**: Update kapasitas saat ada kelas penuh
-   [ ] **Test**: Multiple ruangan dengan kode mirip (A101, A101A)

---

## 🧪 Sample Test Code

```php
describe('Ruangan - Availability Check', function () {
    it('returns available rooms for given time slot', function () {
        $ruangan1 = Ruangan::factory()->create(['kapasitas' => 30]);
        $ruangan2 = Ruangan::factory()->create(['kapasitas' => 50]);

        // Ruangan2 sudah terpakai jam 08:00-10:00
        JadwalPerkuliahan::factory()->create([
            'ruangan_id' => $ruangan2->id,
            'hari' => 'Senin',
            'waktu_mulai' => '08:00',
            'waktu_selesai' => '10:00',
        ]);

        $response = actingAs($this->admin)
            ->get(route('siakad.api.ruangan.available', [
                'hari' => 'Senin',
                'waktu_mulai' => '08:00',
                'waktu_selesai' => '10:00',
            ]));

        $response->assertOk()
            ->assertJsonCount(1); // Only ruangan1

        expect($response->json()[0]['id'])->toBe($ruangan1->id);
    });

    it('filters by minimum capacity', function () {
        Ruangan::factory()->create(['kapasitas' => 20]);
        $large = Ruangan::factory()->create(['kapasitas' => 50]);

        $response = actingAs($this->admin)
            ->get(route('siakad.api.ruangan.available', [
                'min_kapasitas' => 30,
            ]));

        $response->assertJsonCount(1);
        expect($response->json()[0]['id'])->toBe($large->id);
    });
});

describe('Ruangan - Conflict Detection', function () {
    it('detects time overlap', function () {
        $ruangan = Ruangan::factory()->create();

        // Existing: 08:00 - 10:00
        JadwalPerkuliahan::factory()->create([
            'ruangan_id' => $ruangan->id,
            'waktu_mulai' => '08:00',
            'waktu_selesai' => '10:00',
        ]);

        // Try create: 09:00 - 11:00 (overlap)
        $available = $ruangan->isAvailable('Senin', '09:00', '11:00');

        expect($available)->toBeFalse();
    });
});
```

## 🔗 Related Modules

-   [Jadwal Perkuliahan](10_JADWAL_PERKULIAHAN.md)
-   [Jadwal Ujian](11_JADWAL_UJIAN.md)
