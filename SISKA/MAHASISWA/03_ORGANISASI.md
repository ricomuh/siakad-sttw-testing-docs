# Testing: Keaktifan Organisasi

**Status**: ✅ [COMPLETED]
**Role**: Mahasiswa
**Route Prefix**: `/mahasiswa/organisasi`
**Controller**: `Mahasiswa\OrganisasiController`
**Model**: `AnggotaOrganisasi`
**Test File**: `tests/Feature/Mahasiswa/SiskaTest.php`

---

## 📋 Daftar Testing

### 1. Functional Testing

#### 1.1 View List Organisasi

- [x] **Test**: Mahasiswa dapat melihat daftar organisasi yang diikuti
  - **Route**: `mahasiswa.organisasi.index`
  - **Method**: `GET`
  - **Expectation**: Status 200 OK, menampilkan data organisasi

#### 1.2 Daftar Organisasi

- [x] **Test**: Mahasiswa dapat mendaftar ke organisasi
  - **Route**: `mahasiswa.organisasi.store`
  - **Method**: `POST`
  - **Expectation**: Status redirect, data pendaftaran tersimpan
