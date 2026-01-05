# Testing: Bimbingan Akademik & Non-Akademik

**Status**: ✅ [COMPLETED]
**Role**: Dosen
**Route Prefix**: `/dosen/bimbingan`
**Controller**: `Dosen\BimbinganController`
**Model**: `BimbinganMahasiswa`
**Test File**: `tests/Feature/Dosen/SiskaTest.php`

---

## 📋 Daftar Testing

### 1. Functional Testing

#### 1.1 View List Bimbingan

- [x] **Test**: Dosen dapat melihat daftar mahasiswa bimbingan
  - **Route**: `dosen.bimbingan.index`
  - **Method**: `GET`
  - **Expectation**: Status 200 OK, menampilkan data mahasiswa bimbingan

#### 1.2 Input Catatan Bimbingan

- [x] **Test**: Dosen dapat mengisi catatan bimbingan
  - **Route**: `dosen.bimbingan.store`
  - **Method**: `POST`
  - **Expectation**: Status redirect, data catatan tersimpan
