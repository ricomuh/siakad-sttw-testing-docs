# Testing: Master Data Staf

**Status**: ⚪ [TODO]  
**Role**: Admin, HRM  
**Route Prefix**: `/siakad/staf`  
**Controller**: `StafController`  
**Model**: `Staf`

---

## 📋 Daftar Testing

### 1. Functional Testing - CRUD Operations

#### 1.1 Index/List Staf

-   [ ] **Test**: Menampilkan daftar staf
-   [ ] **Test**: Filter berdasarkan departemen/unit
-   [ ] **Test**: Filter berdasarkan status kepegawaian
-   [ ] **Test**: Search berdasarkan NIP/Nama/Email

#### 1.2 Create Staf

-   [ ] **Test**: Berhasil create dengan data lengkap
    -   Data pribadi
    -   Data kepegawaian
    -   Data kontak

#### 1.3 Show Staf

-   [ ] **Test**: Detail staf lengkap
-   [ ] **Test**: Riwayat jabatan

#### 1.4 Edit & Delete

-   [ ] **Test**: Update data staf
-   [ ] **Test**: Soft delete staf

---

### 2. Validation Testing

-   [ ] **Test**: NIP unik
-   [ ] **Test**: Email unik
-   [ ] **Test**: Required fields validation
-   [ ] **Test**: Format validation (email, phone)

---

### 3. File Upload

-   [ ] **Test**: Upload foto staf
-   [ ] **Test**: Upload dokumen (KTP, SK, dll)
-   [ ] **Test**: File type validation
-   [ ] **Test**: File size validation (max 5MB)

---

### 4. Import/Export

-   [ ] **Test**: Export staf to Excel
-   [ ] **Test**: Download template
-   [ ] **Test**: Import from Excel
-   [ ] **Test**: Validation saat import

---

### 5. Authorization

-   [ ] **Test**: Admin full access
-   [ ] **Test**: HRM full access
-   [ ] **Test**: Other roles tidak dapat akses

---

### 6. Integration

-   [ ] **Test**: Auto create user account
-   [ ] **Test**: Assign role "staf" secara otomatis

---

## 🧪 Sample Test Code

```php
describe('Staf - CRUD', function () {
    it('can create staf', function () {
        $data = [
            'nip' => '199001012020011001',
            'nama_lengkap' => 'Jane Doe',
            'email' => 'jane@sttw.ac.id',
            'departemen' => 'Akademik',
            'jabatan' => 'Staff Administrasi',
            'status_kepegawaian' => 'PNS',
        ];

        $response = actingAs($this->admin)
            ->post(route('siakad.staf.store'), $data);

        $response->assertRedirect();
        assertDatabaseHas('staf', ['nip' => '199001012020011001']);
    });
});

describe('Staf - Authorization', function () {
    it('allows access for HRM role', function () {
        $hrm = User::factory()->create();
        $hrm->assignRole('hrm');

        $response = actingAs($hrm)
            ->get(route('siakad.staf.index'));

        $response->assertOk();
    });

    it('denies access for dosen role', function () {
        $dosen = User::factory()->create();
        $dosen->assignRole('dosen');

        $response = actingAs($dosen)
            ->get(route('siakad.staf.index'));

        $response->assertForbidden();
    });
});
```

## 🔗 Related Modules

-   [Data Dosen](04_MASTER_DATA_DOSEN.md)
-   HRM Module (future)
