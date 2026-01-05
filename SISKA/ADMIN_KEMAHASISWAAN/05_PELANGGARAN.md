# Testing: Manajemen Pelanggaran

**Status**: ✅ [COMPLETED]
**Role**: Admin Kemahasiswaan
**Route Prefix**: `/admin-kemahasiswaan/pelanggaran`
**Controller**: `AdminKemahasiswaan\PelanggaranController`
**Model**: `PelanggaranMahasiswa`
**Test File**: `tests/Feature/AdminKemahasiswaan/SiskaTest.php`

---

## 📋 Daftar Testing

### 1. Functional Testing

#### 1.1 View List Pelanggaran

- [x] **Test**: Admin Kemahasiswaan dapat melihat daftar pelanggaran
  - **Route**: `admin-kemahasiswaan.pelanggaran.index`
  - **Method**: `GET`
  - **Expectation**: Status 200 OK, menampilkan data pelanggaran

#### 1.2 Input Pelanggaran

- [x] **Test**: Admin Kemahasiswaan dapat menginput pelanggaran mahasiswa
  - **Route**: `admin-kemahasiswaan.pelanggaran.store`
  - **Method**: `POST`
  - **Expectation**: Status redirect, data tersimpan di database
