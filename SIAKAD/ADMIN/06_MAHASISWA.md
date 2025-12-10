# Testing: Master Data Mahasiswa

**Status**: ✅ [TESTED]  
**Role**: Admin  
**Route Prefix**: `/siakad/mahasiswa`  
**Controller**: `MahasiswaController`  
**Model**: `Mahasiswa`

---

## 📋 Daftar Testing

### 1. Functional Testing - CRUD Operations

#### 1.1 Index/List Mahasiswa

- [ ] **Test**: Menampilkan daftar mahasiswa dengan pagination
- [ ] **Test**: Filter berdasarkan program studi
- [ ] **Test**: Filter berdasarkan angkatan
- [ ] **Test**: Filter berdasarkan status (Aktif, Cuti, Lulus, DO, Mengundurkan Diri)
- [ ] **Test**: Search berdasarkan NIM/Nama/Email
- [ ] **Test**: Sort by NIM, Nama, IPK

#### 1.2 Create Mahasiswa

- [ ] **Test**: Berhasil create dengan data lengkap

  - Data pribadi
  - Data akademik (NIM, Program Studi, Angkatan)
  - Data kontak
  - Data orang tua/wali

- [ ] **Test**: Auto-generate NIM

  - Format: {tahun_masuk}{kode_prodi}{nomor_urut}
  - Example: 2024IF001
  - Verifikasi unique dan sequential

- [ ] **Test**: Assign dosen PA secara otomatis
  - Round-robin assignment
  - Max 30 mahasiswa per dosen

#### 1.3 Show Mahasiswa

- [ ] **Test**: Detail mahasiswa lengkap

  - Biodata
  - Data akademik (IPK, Total SKS)
  - KRS history
  - Transkip nilai
  - Dosen PA

- [ ] **Test**: Kartu Mahasiswa preview
- [ ] **Test**: Chart progress studi (semester vs IPK)

#### 1.4 Edit Mahasiswa

- [ ] **Test**: Update data pribadi
- [ ] **Test**: Update data akademik
- [ ] **Test**: Ganti dosen PA
- [ ] **Test**: Update status mahasiswa (Aktif → Cuti, dll)

#### 1.5 Delete Mahasiswa

- [ ] **Test**: Soft delete mahasiswa tanpa KRS
- [ ] **Test**: Tidak dapat delete jika sudah ada KRS
  - Suggest archive instead of delete

---

### 2. Validation Testing

#### 2.1 Required Fields

- [ ] **Test**: NIM wajib
- [ ] **Test**: Nama Lengkap wajib
- [ ] **Test**: Program Studi wajib
- [ ] **Test**: Angkatan wajib

#### 2.2 Unique Validation

- [ ] **Test**: NIM harus unik
- [ ] **Test**: Email harus unik (jika diisi)

#### 2.3 Format Validation

- [ ] **Test**: NIM format sesuai aturan

  - Pattern: {YYYY}{PRODI}{XXX}
  - Length: 10-12 karakter

- [ ] **Test**: Email format valid
- [ ] **Test**: No HP format valid
- [ ] **Test**: Tanggal lahir tidak di masa depan
- [ ] **Test**: Angkatan range valid (current year - 10 sampai current year)

#### 2.4 Business Logic Validation

- [ ] **Test**: IPK range 0.00 - 4.00
- [ ] **Test**: Semester berjalan 1-14 (max)
- [ ] **Test**: Status mahasiswa enum valid

---

### 3. File Upload Testing

#### 3.1 Upload Foto Mahasiswa

- [ ] **Test**: Upload foto format valid
- [ ] **Test**: Max size 2MB
- [ ] **Test**: Resize to 400x400

#### 3.2 Upload Dokumen

- [ ] **Test**: Upload Ijazah/STTB
- [ ] **Test**: Upload Akte Kelahiran
- [ ] **Test**: Upload KTP
- [ ] **Test**: Upload Kartu Keluarga
- [ ] **Test**: Max 5MB per file
- [ ] **Test**: Format: PDF, JPG, PNG

---

### 4. Import/Export Testing

#### 4.1 Export Mahasiswa

- [ ] **Test**: Export all mahasiswa
- [ ] **Test**: Export filtered (per prodi, per angkatan)
- [ ] **Test**: Export dengan IPK dan total SKS
- [ ] **Test**: Download template import

#### 4.2 Import Mahasiswa

- [ ] **Test**: Import batch mahasiswa baru

  - Upload Excel dengan 100+ rows
  - Verifikasi semua terimport

- [ ] **Test**: Import dengan auto-generate NIM
- [ ] **Test**: Import dengan auto-assign PA
- [ ] **Test**: Import validation per row
- [ ] **Test**: Import skip duplicate NIM
- [ ] **Test**: Background job untuk import besar (queue)

---

### 5. Authorization Testing

- [ ] **Test**: Admin full CRUD
- [ ] **Test**: Akademik dapat view dan edit
- [ ] **Test**: Dosen PA dapat view mahasiswa bimbingan saja
- [ ] **Test**: Mahasiswa dapat view own profile (read-only)

---

### 6. Integration Testing

#### 6.1 Relasi dengan Program Studi

- [ ] **Test**: Mahasiswa belong to program studi
- [ ] **Test**: Filter by program studi efficient

#### 6.2 Relasi dengan Dosen PA

- [ ] **Test**: Assign dosen PA
- [ ] **Test**: Change dosen PA dengan validasi
- [ ] **Test**: Dosen PA tidak boleh dari prodi berbeda

#### 6.3 Relasi dengan KRS

- [ ] **Test**: Mahasiswa dapat register KRS
- [ ] **Test**: IPK calculation dari nilai
- [ ] **Test**: Total SKS tempuh update otomatis

#### 6.4 Auto Create User Account

- [ ] **Test**: Create user saat create mahasiswa

  - Username: NIM
  - Email: email mahasiswa
  - Default password: NIM atau tanggal lahir
  - Role: mahasiswa

- [ ] **Test**: Update email → update user account
- [ ] **Test**: Status Non-Aktif → disable account

---

### 7. Responsive Testing

- [ ] **Test**: Tabel mahasiswa responsive
- [ ] **Test**: Profile card responsive
- [ ] **Test**: Form multi-step responsive (wizard)

---

### 8. UI/UX Testing

#### 8.1 Profile Display

- [ ] **Test**: Foto mahasiswa circular
- [ ] **Test**: Badge status (Aktif = hijau, Cuti = kuning, dst)
- [ ] **Test**: Progress bar semester
- [ ] **Test**: IPK display dengan 2 decimal

#### 8.2 Bulk Actions

- [ ] **Test**: Select multiple mahasiswa
- [ ] **Test**: Bulk update status
- [ ] **Test**: Bulk assign PA
- [ ] **Test**: Bulk export

#### 8.3 Advanced Search

- [ ] **Test**: Multi-criteria search
  - Nama + Prodi + Angkatan
  - Search dengan filter terintegrasi

---

### 9. Performance Testing

- [ ] **Test**: Load 5000+ mahasiswa dengan pagination
- [ ] **Test**: Search performance (< 1s)
- [ ] **Test**: Eager loading (IPK calculation tidak N+1)
- [ ] **Test**: Index optimization (NIM, email, prodi_id)

---

### 10. Edge Cases

- [ ] **Test**: Mahasiswa pindah prodi

  - Update NIM atau keep old NIM?
  - Update KRS history
  - Update dosen PA

- [ ] **Test**: Mahasiswa re-aktif setelah cuti

  - Resume semester
  - Restore PA

- [ ] **Test**: Mahasiswa dengan IPK 4.00
- [ ] **Test**: Mahasiswa dengan IPK 0.00 (belum ada nilai)
- [ ] **Test**: Mahasiswa lulus cepat (< 8 semester)
- [ ] **Test**: Mahasiswa > semester 14 (lebih dari batas normal)

- [ ] **Test**: Nama mahasiswa sangat panjang (255 char)
- [ ] **Test**: Mahasiswa tanpa email (nullable)
- [ ] **Test**: Multiple mahasiswa dari keluarga yang sama

---

## 🧪 Sample Test Code

```php
describe('Mahasiswa - CRUD', function () {
    it('can create mahasiswa with auto-generated NIM', function () {
        $prodi = ProgramStudi::factory()->create(['kode' => 'IF']);

        $data = [
            'nama_lengkap' => 'John Student',
            'email' => 'john.student@gmail.com',
            'program_studi_id' => $prodi->id,
            'angkatan' => 2024,
        ];

        $response = actingAs($this->admin)
            ->post(route('siakad.mahasiswa.store'), $data);

        $response->assertRedirect();

        $mahasiswa = Mahasiswa::where('email', 'john.student@gmail.com')->first();

        expect($mahasiswa->nim)->toStartWith('2024IF');
        expect($mahasiswa->nim)->toHaveLength(10);
    });

    it('auto-assigns dosen PA on create', function () {
        $prodi = ProgramStudi::factory()->create();

        // Create 2 dosen di prodi yang sama
        $dosen1 = Dosen::factory()->for($prodi)->create();
        $dosen2 = Dosen::factory()->for($prodi)->create();

        $data = [
            'nama_lengkap' => 'Student 1',
            'program_studi_id' => $prodi->id,
            'angkatan' => 2024,
        ];

        $response = actingAs($this->admin)
            ->post(route('siakad.mahasiswa.store'), $data);

        $mahasiswa = Mahasiswa::latest()->first();

        expect($mahasiswa->dosen_pa_id)->toBeIn([$dosen1->id, $dosen2->id]);
    });
});

describe('Mahasiswa - Import', function () {
    it('can import mahasiswa from excel with queue', function () {
        Storage::fake('local');

        $file = UploadedFile::fake()->create('mahasiswa.xlsx', 500);

        Queue::fake();

        $response = actingAs($this->admin)
            ->post(route('siakad.mahasiswa.import'), ['file' => $file]);

        $response->assertRedirect();

        Queue::assertPushed(ImportMahasiswaJob::class);
    });
});

describe('Mahasiswa - IPK Calculation', function () {
    it('calculates IPK correctly', function () {
        $mahasiswa = Mahasiswa::factory()->create();

        // Semester 1: 20 SKS, IP 3.5
        // Semester 2: 20 SKS, IP 3.8
        // IPK = (20*3.5 + 20*3.8) / 40 = 3.65

        // Setup nilai...

        expect($mahasiswa->fresh()->ipk)->toBe(3.65);
    });
});

describe('Mahasiswa - User Account Integration', function () {
    it('creates user account on mahasiswa creation', function () {
        $data = [
            'nim' => '2024IF001',
            'nama_lengkap' => 'Test Student',
            'email' => 'test@gmail.com',
            'program_studi_id' => ProgramStudi::factory()->create()->id,
        ];

        $response = actingAs($this->admin)
            ->post(route('siakad.mahasiswa.store'), $data);

        assertDatabaseHas('users', [
            'email' => 'test@gmail.com',
        ]);

        $user = User::where('email', 'test@gmail.com')->first();

        expect($user->hasRole('mahasiswa'))->toBeTrue();
    });
});
```

## 🔗 Related Modules

- [Program Studi](01_MASTER_DATA_PROGRAM_STUDI.md)
- [Dosen PA](04_MASTER_DATA_DOSEN.md)
- [KRS Mahasiswa](15_MONITORING_KRS.md)
- [Manajemen Yudisium](16_MANAJEMEN_YUDISIUM.md)
