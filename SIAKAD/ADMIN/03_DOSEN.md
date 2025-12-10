# Testing: Master Data Dosen & Struktural

**Status**: ✅ [TESTED]  
**Role**: Admin  
**Route Prefix**: `/siakad/dosen`  
**Controller**: `DosenController`  
**Model**: `Dosen`, `DosenPendidikan`, `DosenPengalaman`, `DosenDokumen`

---

## 📋 Daftar Testing

### 1. Functional Testing - CRUD Operations

#### 1.1 Index/List Dosen

- [ ] **Test**: Menampilkan daftar dosen dengan pagination
- [ ] **Test**: Filter berdasarkan program studi
- [ ] **Test**: Filter berdasarkan status kepegawaian (PNS, PPPK, Kontrak, Honorer)
- [ ] **Test**: Filter berdasarkan status keaktifan (Aktif, Cuti, Non-Aktif)
- [ ] **Test**: Search berdasarkan NIP/NIDN/Nama/Email
- [ ] **Test**: Sort by nama, NIP, status

#### 1.2 Create Dosen

- [ ] **Test**: Form create menampilkan tabs

  - Tab: Data Pribadi
  - Tab: Data Kepegawaian
  - Tab: Riwayat Pendidikan
  - Tab: Riwayat Pengalaman

- [ ] **Test**: Berhasil create dengan data minimal (required only)

  - NIP Internal, Nama Lengkap, Email
  - Verifikasi tersimpan

- [ ] **Test**: Berhasil create dengan data lengkap

  - Semua field terisi
  - Multiple pendidikan
  - Multiple pengalaman
  - Verifikasi relasi tersimpan

- [ ] **Test**: Auto-generate username dari email
  - Email: john@sttw.ac.id → username: john
  - Verifikasi unique username

#### 1.3 Show Dosen

- [ ] **Test**: Detail dosen menampilkan semua info

  - Data pribadi dengan gelar (Dr. John Doe, S.Kom., M.Kom.)
  - Data kepegawaian
  - Riwayat pendidikan (terbaru di atas)
  - Riwayat pengalaman

- [ ] **Test**: Menampilkan foto dosen (jika ada)
- [ ] **Test**: Menampilkan dokumen uploaded
- [ ] **Test**: Menampilkan mata kuliah yang diampu
- [ ] **Test**: Menampilkan mahasiswa bimbingan (PA)

#### 1.4 Edit Dosen

- [ ] **Test**: Update data pribadi
- [ ] **Test**: Update data kepegawaian
- [ ] **Test**: Tambah riwayat pendidikan baru
- [ ] **Test**: Edit riwayat pendidikan existing
- [ ] **Test**: Hapus riwayat pendidikan
- [ ] **Test**: Tambah/edit/hapus riwayat pengalaman

#### 1.5 Delete Dosen

- [ ] **Test**: Soft delete dosen tanpa relasi
- [ ] **Test**: Tidak dapat delete jika masih mengajar

  - Ada formasi dosen aktif
  - Error message user-friendly

- [ ] **Test**: Tidak dapat delete jika masih jadi PA
  - Ada mahasiswa bimbingan
  - Suggest reassign PA

---

### 2. Validation Testing

#### 2.1 Required Fields

- [ ] **Test**: NIP Internal wajib
- [ ] **Test**: Nama Lengkap wajib
- [ ] **Test**: Email Institusi wajib

#### 2.2 Unique Validation

- [ ] **Test**: NIP Internal harus unik
- [ ] **Test**: NIDN harus unik (jika diisi)
- [ ] **Test**: Email Institusi harus unik
- [ ] **Test**: Email Pribadi harus unik (jika diisi)

#### 2.3 Format Validation

- [ ] **Test**: NIP format angka (10-18 digit)
- [ ] **Test**: NIDN format angka (10 digit)
- [ ] **Test**: Email format valid
- [ ] **Test**: No HP format valid (08xx atau +62)
- [ ] **Test**: Tanggal lahir tidak boleh di masa depan
- [ ] **Test**: Tanggal mulai bekerja <= hari ini

#### 2.4 Enum Validation

- [ ] **Test**: Jenis Kelamin: L, P
- [ ] **Test**: Status Kepegawaian: PNS, PPPK, Kontrak, Honorer
- [ ] **Test**: Status Keaktifan: Aktif, Cuti, Non-Aktif

#### 2.5 Relational Validation

- [ ] **Test**: Program Studi ID harus exist
- [ ] **Test**: Riwayat Pendidikan - Jenjang valid (S3, S2, S1, D4, D3)
- [ ] **Test**: Tahun Lulus tidak di masa depan (max: current year + 10)

---

### 3. File Upload Testing

#### 3.1 Upload Foto Dosen

- [ ] **Test**: Upload foto format valid (jpg, png)
- [ ] **Test**: Max size 2MB
- [ ] **Test**: Resize foto ke ukuran standard (300x300)
- [ ] **Test**: Replace foto existing
- [ ] **Test**: Hapus foto

#### 3.2 Upload Dokumen

- [ ] **Test**: Upload KTP (required)
- [ ] **Test**: Upload Ijazah (multiple files untuk S1, S2, S3)
- [ ] **Test**: Upload SK Pengangkatan
- [ ] **Test**: Upload Sertifikat
- [ ] **Test**: Dokumen max 5MB
- [ ] **Test**: Format: PDF, JPG, PNG
- [ ] **Test**: Validasi dokumen sebelum upload
  - Route: `/siakad/dosen/validate-dokumen` (POST)
  - Return: mime type check, size check

#### 3.3 Dokumen Management

- [ ] **Test**: List dokumen per dosen
- [ ] **Test**: Download dokumen
- [ ] **Test**: Delete dokumen
  - Soft delete record
  - Hapus file dari storage

---

### 4. Import/Export Testing

#### 4.1 Export Dosen

- [ ] **Test**: Export all dosen to Excel

  - Format: XLSX
  - Columns: NIP, NIDN, Nama, Email, Program Studi, Status
  - Verifikasi data lengkap

- [ ] **Test**: Export dengan filter

  - Export hanya dosen aktif
  - Export per program studi

- [ ] **Test**: Download template Excel
  - Route: `/siakad/dosen/download-template`
  - Verifikasi header columns

#### 4.2 Import Dosen

- [ ] **Test**: Import dari Excel valid

  - Upload file XLSX
  - Verifikasi data terimport
  - Verifikasi flash message (X data berhasil, Y gagal)

- [ ] **Test**: Import dengan data duplikat

  - Skip duplicate NIP
  - Log error per row

- [ ] **Test**: Import dengan data invalid

  - Email format salah
  - Required field kosong
  - Verifikasi error report

- [ ] **Test**: Import large file (1000+ rows)
  - Use queue untuk background processing
  - Verifikasi progress tracking

---

### 5. Authorization Testing

#### 5.1 Role-Based Access

- [ ] **Test**: Admin full CRUD
- [ ] **Test**: Akademik dapat view dan edit (tidak delete)
- [ ] **Test**: Dosen dapat view own profile dan edit limited fields
- [ ] **Test**: Mahasiswa tidak dapat akses

---

### 6. Integration Testing

#### 6.1 Relasi dengan Formasi Dosen

- [ ] **Test**: Dosen ter-assign ke mata kuliah
- [ ] **Test**: Tampilkan teaching load (total SKS mengajar)
- [ ] **Test**: Validasi max teaching load (16 SKS)

#### 6.2 Relasi dengan Mahasiswa (PA)

- [ ] **Test**: Assign dosen sebagai PA
- [ ] **Test**: List mahasiswa bimbingan
- [ ] **Test**: Max mahasiswa bimbingan per dosen (30)

#### 6.3 Relasi dengan User Account

- [ ] **Test**: Auto create user account saat create dosen

  - Email sebagai username
  - Default password
  - Assign role "dosen"

- [ ] **Test**: Update email → update username
- [ ] **Test**: Deactivate account saat status Non-Aktif

---

### 7. Responsive Testing

- [ ] **Test**: Tabel dosen responsive (card view di mobile)
- [ ] **Test**: Form multi-tab responsive
- [ ] **Test**: Upload foto responsive

---

### 8. UI/UX Testing

#### 8.1 Form Tabs

- [ ] **Test**: Tab navigation berfungsi
- [ ] **Test**: Save form memvalidasi semua tab
- [ ] **Test**: Error pada tab tertentu → tab highlight merah

#### 8.2 Dynamic Forms

- [ ] **Test**: Tambah riwayat pendidikan (button + field)
- [ ] **Test**: Hapus riwayat pendidikan (button - field)
- [ ] **Test**: Minimal 1 pendidikan required

#### 8.3 Gelar Display

- [ ] **Test**: Display nama dengan gelar
  - Format: {gelar_depan} {nama} {gelar_belakang}
  - Example: "Dr. John Doe, S.Kom., M.Kom."

---

### 9. Performance Testing

- [ ] **Test**: Load 500+ dosen dengan pagination
- [ ] **Test**: Search dengan large dataset (< 1s)
- [ ] **Test**: Eager loading relasi (no N+1)

---

### 10. Edge Cases

- [ ] **Test**: Dosen dengan multiple gelar

  - Gelar depan: "Prof. Dr. Ir."
  - Gelar belakang: "S.T., M.T., Ph.D."

- [ ] **Test**: Dosen tanpa email pribadi (nullable)
- [ ] **Test**: Dosen dengan riwayat pendidikan 5+ (S1, S2, S3, Profesi, dll)
- [ ] **Test**: Dosen pindah program studi

  - Update formasi dosen
  - Update PA assignment

- [ ] **Test**: Concurrent edit
  - 2 admin edit dosen yang sama
  - Last write wins atau optimistic locking?

---

## 🧪 Sample Test Code

```php
describe('Dosen - CRUD', function () {
    it('can create dosen with complete data', function () {
        $programStudi = ProgramStudi::factory()->create();

        $data = [
            'nip_internal' => '1234567890',
            'nidn' => '0101018901',
            'gelar_depan' => 'Dr.',
            'nama_lengkap' => 'John Doe',
            'gelar_belakang' => 'S.Kom., M.Kom.',
            'email_institusi' => 'john@sttw.ac.id',
            'program_studi_id' => $programStudi->id,
            'status_kepegawaian' => 'PNS',
            'pendidikan' => [
                [
                    'jenjang' => 'S1',
                    'institusi' => 'Universitas A',
                    'jurusan_program_studi' => 'Teknik Informatika',
                    'tahun_lulus' => 2010,
                ],
                [
                    'jenjang' => 'S2',
                    'institusi' => 'Universitas B',
                    'jurusan_program_studi' => 'Ilmu Komputer',
                    'tahun_lulus' => 2015,
                ],
            ],
        ];

        $response = actingAs($this->admin)
            ->post(route('siakad.dosen.store'), $data);

        $response->assertRedirect();

        assertDatabaseHas('dosen', [
            'nip_internal' => '1234567890',
            'nama_lengkap' => 'John Doe',
        ]);

        assertDatabaseCount('dosen_pendidikan', 2);
    });
});

describe('Dosen - Import', function () {
    it('can import dosen from excel', function () {
        Storage::fake('local');

        $file = UploadedFile::fake()->create('dosen.xlsx', 100, 'application/vnd.ms-excel');

        $response = actingAs($this->admin)
            ->post(route('siakad.dosen.import'), ['file' => $file]);

        $response->assertRedirect();
        $response->assertSessionHas('success');
    });

    it('validates import file type', function () {
        $file = UploadedFile::fake()->create('dosen.pdf');

        $response = actingAs($this->admin)
            ->post(route('siakad.dosen.import'), ['file' => $file]);

        $response->assertInvalid(['file']);
    });
});

describe('Dosen - File Upload', function () {
    it('can upload dosen photo', function () {
        Storage::fake('public');

        $dosen = Dosen::factory()->create();
        $photo = UploadedFile::fake()->image('photo.jpg');

        $response = actingAs($this->admin)
            ->post(route('siakad.dosen.upload-dokumen', $dosen), [
                'jenis' => 'foto',
                'file' => $photo,
            ]);

        $response->assertRedirect();
        Storage::disk('public')->assertExists('dosen/foto/' . $photo->hashName());
    });
});
```

## 🔗 Related Modules

- [Program Studi](01_MASTER_DATA_PROGRAM_STUDI.md)
- [Formasi Dosen](08_FORMASI_DOSEN.md)
- [Validasi KRS (Dosen PA)](../DOSEN/VALIDASI_KRS.md)
