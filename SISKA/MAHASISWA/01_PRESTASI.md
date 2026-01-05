# Testing: Input Prestasi Mahasiswa

**Status**: ✅ [COMPLETED]
**Role**: Mahasiswa
**Route Prefix**: `/mahasiswa/prestasi`
**Controller**: `Mahasiswa\PrestasiController`
**Model**: `PrestasiMahasiswa`
**Test File**: `tests/Feature/Mahasiswa/SiskaTest.php`

---

## 📋 Daftar Testing

### 1. Functional Testing

#### 1.1 View List Prestasi

- [x] **Test**: Mahasiswa dapat melihat daftar prestasi sendiri
  - **Route**: `mahasiswa.prestasi.index`
  - **Method**: `GET`
  - **Expectation**: Status 200 OK, menampilkan data prestasi

#### 1.2 Input Prestasi

- [x] **Test**: Mahasiswa dapat menginput data prestasi
  - **Route**: `mahasiswa.prestasi.store`
  - **Method**: `POST`
  - **Expectation**: Status redirect, data tersimpan di database
