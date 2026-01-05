# Testing: Pendaftaran Beasiswa

**Status**: ✅ [COMPLETED]
**Role**: Mahasiswa
**Route Prefix**: `/mahasiswa/beasiswa`
**Controller**: `Mahasiswa\BeasiswaController`
**Model**: `PendaftarBeasiswa`
**Test File**: `tests/Feature/Mahasiswa/SiskaTest.php`

---

## 📋 Daftar Testing

### 1. Functional Testing

#### 1.1 View List Beasiswa

- [x] **Test**: Mahasiswa dapat melihat daftar beasiswa yang tersedia
  - **Route**: `mahasiswa.beasiswa.index`
  - **Method**: `GET`
  - **Expectation**: Status 200 OK, menampilkan data beasiswa

#### 1.2 Apply Beasiswa

- [x] **Test**: Mahasiswa dapat mendaftar beasiswa
  - **Route**: `mahasiswa.beasiswa.store`
  - **Method**: `POST`
  - **Expectation**: Status redirect, data pendaftaran tersimpan
