# Testing: Manajemen Pengajuan Surat

**Status**: ✅ [COMPLETED]
**Role**: Admin Kemahasiswaan
**Route Prefix**: `/admin-kemahasiswaan/pengajuan-surat`
**Controller**: `AdminKemahasiswaan\PengajuanSuratController`
**Model**: `PengajuanSurat`
**Test File**: `tests/Feature/AdminKemahasiswaan/SiskaTest.php`

---

## 📋 Daftar Testing

### 1. Functional Testing

#### 1.1 View List Pengajuan

- [x] **Test**: Admin Kemahasiswaan dapat melihat daftar pengajuan surat
  - **Route**: `admin-kemahasiswaan.pengajuan-surat.index`
  - **Method**: `GET`
  - **Expectation**: Status 200 OK, menampilkan data pengajuan

#### 1.2 Approval Pengajuan

- [x] **Test**: Admin Kemahasiswaan dapat menyetujui pengajuan surat

  - **Route**: `admin-kemahasiswaan.pengajuan-surat.approve`
  - **Method**: `POST`
  - **Expectation**: Status redirect, status pengajuan berubah menjadi 'Disetujui'

- [x] **Test**: Admin Kemahasiswaan dapat menolak pengajuan surat
  - **Route**: `admin-kemahasiswaan.pengajuan-surat.reject`
  - **Method**: `POST`
  - **Expectation**: Status redirect, status pengajuan berubah menjadi 'Ditolak'
