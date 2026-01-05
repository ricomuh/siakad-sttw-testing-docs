# Testing: Manajemen Beasiswa

**Status**: ✅ [COMPLETED]
**Role**: Admin Kemahasiswaan
**Route Prefix**: `/admin-kemahasiswaan/beasiswa`
**Controller**: `AdminKemahasiswaan\BeasiswaController`
**Model**: `Beasiswa`
**Test File**: `tests/Feature/AdminKemahasiswaan/SiskaTest.php`

---

## 📋 Daftar Testing

### 1. Functional Testing

#### 1.1 View List Beasiswa

- [x] **Test**: Admin Kemahasiswaan dapat melihat daftar beasiswa
  - **Route**: `admin-kemahasiswaan.beasiswa.index`
  - **Method**: `GET`
  - **Expectation**: Status 200 OK, menampilkan data beasiswa

#### 1.2 Create Beasiswa

- [x] **Test**: Admin Kemahasiswaan dapat menambah data beasiswa
  - **Route**: `admin-kemahasiswaan.beasiswa.store`
  - **Method**: `POST`
  - **Expectation**: Status redirect, data tersimpan di database

#### 1.3 Seleksi Penerima

- [x] **Test**: Admin Kemahasiswaan dapat melakukan seleksi penerima beasiswa
  - **Route**: `admin-kemahasiswaan.beasiswa.seleksi`
  - **Method**: `POST`
  - **Expectation**: Status redirect, status pendaftar berubah
