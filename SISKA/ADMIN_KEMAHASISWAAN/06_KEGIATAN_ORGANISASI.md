# Testing: Monitoring Kegiatan Organisasi

**Status**: ✅ [COMPLETED]
**Role**: Admin Kemahasiswaan
**Route Prefix**: `/admin-kemahasiswaan/kegiatan`
**Controller**: `AdminKemahasiswaan\KegiatanController`
**Model**: `Kegiatan`
**Test File**: `tests/Feature/AdminKemahasiswaan/SiskaTest.php`

---

## 📋 Daftar Testing

### 1. Functional Testing

#### 1.1 View List Kegiatan

- [x] **Test**: Admin Kemahasiswaan dapat melihat daftar kegiatan organisasi
  - **Route**: `admin-kemahasiswaan.kegiatan.index`
  - **Method**: `GET`
  - **Expectation**: Status 200 OK, menampilkan data kegiatan

#### 1.2 Approval Kegiatan

- [x] **Test**: Admin Kemahasiswaan dapat menyetujui proposal kegiatan
  - **Route**: `admin-kemahasiswaan.kegiatan.approve`
  - **Method**: `POST`
  - **Expectation**: Status redirect, status proposal berubah menjadi 'Disetujui'
