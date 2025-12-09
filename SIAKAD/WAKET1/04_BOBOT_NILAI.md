# Testing: Bobot Nilai (Kebijakan)

**Status**: 🟡 In Progress  
**Role**: Waket1 (Wakil Ketua 1 - Bidang Akademik)  
**Route**: `/waket1/bobot-nilai`  
**Controller**: `App\Http\Controllers\Waket1\BobotNilaiController`  
**Model**: BobotNilai, ProgramStudi

---

## Checklist Testing

### 1. Operations to Test

- [ ] **Index Bobot Nilai**

  - Method: `GET /waket1/bobot-nilai`
  - Action: `BobotNilaiController@index`
  - Menampilkan daftar bobot nilai per program studi
  - Response: View dengan list bobot nilai

- [ ] **Create Bobot Nilai**

  - Method: `GET /waket1/bobot-nilai/create`
  - Action: `BobotNilaiController@create`
  - Menampilkan form tambah bobot nilai
  - Response: View form create

- [ ] **Store Bobot Nilai**

  - Method: `POST /waket1/bobot-nilai`
  - Action: `BobotNilaiController@store`
  - Menyimpan bobot nilai baru
  - Response: Redirect dengan success message

- [ ] **Edit Bobot Nilai**

  - Method: `GET /waket1/bobot-nilai/{bobotNilai}/edit`
  - Action: `BobotNilaiController@edit`
  - Menampilkan form edit bobot nilai
  - Response: View form edit

- [ ] **Update Bobot Nilai**

  - Method: `PATCH /waket1/bobot-nilai/{bobotNilai}`
  - Action: `BobotNilaiController@update`
  - Mengupdate bobot nilai
  - Response: Redirect dengan success message

- [ ] **Delete Bobot Nilai**
  - Method: `DELETE /waket1/bobot-nilai/{bobotNilai}`
  - Action: `BobotNilaiController@destroy`
  - Menghapus bobot nilai
  - Response: Redirect dengan success message

### 2. Authorization Testing

- [ ] **Access Control**
  - [x] User dengan role `waket1` dapat mengakses semua operasi
  - [x] User tanpa role `waket1` mendapat 403 Forbidden
  - [x] Guest user redirect ke halaman login
  - [x] User dengan role `dosen` tidak dapat mengakses (read-only via sistem)
  - [x] User dengan role `mahasiswa` tidak dapat mengakses

### 3. Index View - Display

- [ ] **Table Display**

  - [x] Column "Program Studi" ditampilkan
  - [x] Column "Grade" (A, AB, B, BC, C, D, E) ditampilkan
  - [x] Column "Nilai Minimum" ditampilkan (misal: 80.00)
  - [x] Column "Nilai Maksimum" ditampilkan (misal: 100.00)
  - [x] Column "Bobot" ditampilkan (misal: 4.00)
  - [x] Column "Keterangan/Predikat" ditampilkan (misal: "Sangat Baik")
  - [x] Column "Status Aktif" ditampilkan dengan badge (Aktif/Tidak Aktif)
  - [x] Column "Aksi" berisi button Edit dan Delete

- [ ] **Grouping by Program Studi**
  - [x] Data di-group per program studi
  - [x] Header section untuk setiap program studi
  - [x] Collapse/expand per program studi
  - [x] Button "Tambah Grade" per program studi
  - [x] Total grade ditampilkan per program studi (misal: 7 grade)

### 4. Filter & Search

- [ ] **Filter Program Studi**

  - [x] Dropdown program studi tersedia
  - [x] Filtering bobot nilai berdasarkan program studi
  - [x] Option "Semua Program Studi" tersedia
  - [x] Data ter-filter sesuai program studi yang dipilih

- [ ] **Filter Status**
  - [x] Toggle "Tampilkan Hanya Aktif" tersedia
  - [x] Filtering bobot nilai dengan status aktif = true
  - [x] Default menampilkan semua (aktif + tidak aktif)

### 5. Create Bobot Nilai

- [ ] **Form Fields**

  - [x] Dropdown "Program Studi" (required)
  - [x] Input "Grade" (required, misal: A, AB, B, BC, C, D, E)
  - [x] Input "Nilai Minimum" (required, numeric, 0-100)
  - [x] Input "Nilai Maksimum" (required, numeric, 0-100)
  - [x] Input "Bobot" (required, numeric, 0-4, misal: 4.00)
  - [x] Input "Keterangan" (optional, misal: "Sangat Baik")
  - [x] Checkbox "Aktif" (default checked)
  - [x] Button "Simpan" dan "Batal"

- [ ] **Validation Rules**

  - [x] Program Studi wajib dipilih
  - [x] Grade wajib diisi, format uppercase (A, AB, B, BC, C, D, E)
  - [x] Nilai Minimum wajib diisi, numeric, 0-100
  - [x] Nilai Maksimum wajib diisi, numeric, 0-100
  - [x] Nilai Maksimum harus lebih besar dari Nilai Minimum
  - [x] Bobot wajib diisi, numeric, range 0-4, format 2 desimal
  - [x] Kombinasi Program Studi + Grade harus unique (tidak boleh duplikat)

- [ ] **Auto-fill Logic**
  - [x] Jika Grade = "A", auto-suggest: Min 85, Max 100, Bobot 4.00
  - [x] Jika Grade = "AB", auto-suggest: Min 80, Max 84.99, Bobot 3.50
  - [x] Jika Grade = "B", auto-suggest: Min 75, Max 79.99, Bobot 3.00
  - [x] Jika Grade = "BC", auto-suggest: Min 70, Max 74.99, Bobot 2.50
  - [x] Jika Grade = "C", auto-suggest: Min 65, Max 69.99, Bobot 2.00
  - [x] Jika Grade = "D", auto-suggest: Min 55, Max 64.99, Bobot 1.00
  - [x] Jika Grade = "E", auto-suggest: Min 0, Max 54.99, Bobot 0.00
  - [x] User dapat override auto-suggest values

### 6. Edit Bobot Nilai

- [ ] **Form Pre-fill**

  - [x] Form ter-pre-fill dengan data bobot nilai yang akan diedit
  - [x] Program Studi tidak dapat diubah (disabled atau hidden input)
  - [x] Grade tidak dapat diubah (disabled atau hidden input)
  - [x] Nilai Min, Max, Bobot dapat diubah
  - [x] Keterangan dapat diubah
  - [x] Status Aktif dapat diubah

- [ ] **Validation Rules**

  - [x] Nilai Minimum wajib diisi, numeric, 0-100
  - [x] Nilai Maksimum wajib diisi, numeric, 0-100
  - [x] Nilai Maksimum harus lebih besar dari Nilai Minimum
  - [x] Bobot wajib diisi, numeric, range 0-4
  - [x] Perubahan tidak boleh membuat overlap range dengan grade lain di program studi yang sama

- [ ] **Overlap Detection**
  - [x] Validasi tidak ada overlap range nilai dengan grade lain
  - [x] Contoh: Jika A = 85-100, maka AB tidak boleh 84-100 (overlap di 85-100)
  - [x] Error message jelas menunjukkan grade mana yang overlap
  - [x] Suggest nilai yang tidak overlap

### 7. Delete Bobot Nilai

- [ ] **Delete Confirmation**

  - [x] Modal konfirmasi muncul sebelum delete
  - [x] Warning message ditampilkan: "Hapus bobot nilai ini akan mempengaruhi konversi nilai mahasiswa"
  - [x] Info grade yang akan dihapus ditampilkan
  - [x] Button "Ya, Hapus" dan "Batal" tersedia

- [ ] **Delete Validation**

  - [x] Check apakah ada nilai mahasiswa yang menggunakan bobot nilai ini
  - [x] Jika ada nilai mahasiswa, tampilkan warning: "X mahasiswa menggunakan grade ini"
  - [x] Opsi: Soft delete (set aktif = false) instead of hard delete jika ada nilai mahasiswa
  - [x] Hard delete hanya jika tidak ada nilai mahasiswa yang menggunakan

- [ ] **Delete Process**
  - [x] Soft delete: Set `is_active` = false, `deleted_at` = timestamp
  - [x] Hard delete: Hapus record dari database (jika tidak ada nilai mahasiswa)
  - [x] Success message sesuai tipe delete (soft/hard)
  - [x] Redirect ke index page setelah delete

### 8. Apply to Program Studi

- [ ] **Bulk Apply Template**

  - [x] Button "Terapkan Template Standar" tersedia di index
  - [x] Modal konfirmasi muncul saat diklik
  - [x] Dropdown pilih program studi target (atau "Semua Program Studi")
  - [x] Template standar 7 grade (A, AB, B, BC, C, D, E) siap diterapkan
  - [x] Preview template ditampilkan di modal

- [ ] **Apply Process**
  - [x] Check apakah program studi sudah memiliki bobot nilai
  - [x] Jika sudah ada, tampilkan warning: "Program studi ini sudah memiliki bobot nilai. Timpa atau batalkan?"
  - [x] Opsi: "Timpa" (replace semua), "Gabung" (tambahkan yang belum ada), "Batal"
  - [x] Proses apply dengan database transaction
  - [x] Success message menampilkan jumlah grade yang ditambahkan

### 9. Export & Import

- [ ] **Export Excel**

  - [x] Button "Export Excel" tersedia
  - [x] Excel berisi semua bobot nilai, group by program studi
  - [x] Column: Program Studi, Grade, Nilai Min, Nilai Max, Bobot, Keterangan, Status
  - [x] Format excel rapi dengan header bold dan border
  - [x] File naming: `Bobot_Nilai_{TanggalWaktu}.xlsx`

- [ ] **Import Excel**
  - [x] Button "Import Excel" tersedia
  - [x] Upload file excel dengan format template
  - [x] Validasi format excel (column headers harus sesuai)
  - [x] Validasi data per row (nilai min/max, bobot, unique constraint)
  - [x] Preview data sebelum import
  - [x] Opsi: "Timpa" atau "Tambahkan" untuk data yang sudah ada
  - [x] Success message menampilkan jumlah row berhasil dan gagal diimport
  - [x] Error message detail untuk row yang gagal

### 10. History & Audit Log

- [ ] **Change Log**
  - [x] Button "Lihat Riwayat" per bobot nilai
  - [x] Modal menampilkan history perubahan
  - [x] Column: Tanggal, User, Aksi (Create/Update/Delete), Nilai Lama, Nilai Baru
  - [x] Log disimpan di table `bobot_nilai_logs`
  - [x] Pagination untuk log jika banyak

### 11. Impact Analysis

- [ ] **Analisis Dampak Perubahan**
  - [x] Button "Analisis Dampak" tersedia sebelum update bobot nilai
  - [x] Modal menampilkan jumlah mahasiswa yang terpengaruh
  - [x] List mahasiswa yang nilainya akan berubah (jika ada)
  - [x] Perubahan IPK mahasiswa yang terpengaruh ditampilkan (before vs after)
  - [x] Warning jika perubahan berdampak signifikan (> 10 mahasiswa)
  - [x] Require confirmation checkbox: "Saya memahami dampak perubahan ini"

### 12. Default Template

- [ ] **Template Standar**
  - [x] Template default tersedia untuk quick setup:
    - **A**: 85.00 - 100.00 = 4.00 (Sangat Baik)
    - **AB**: 80.00 - 84.99 = 3.50 (Lebih dari Baik)
    - **B**: 75.00 - 79.99 = 3.00 (Baik)
    - **BC**: 70.00 - 74.99 = 2.50 (Lebih dari Cukup)
    - **C**: 65.00 - 69.99 = 2.00 (Cukup)
    - **D**: 55.00 - 64.99 = 1.00 (Kurang)
    - **E**: 0.00 - 54.99 = 0.00 (Sangat Kurang)
  - [x] Template dapat disesuaikan per program studi
  - [x] Template dapat di-export sebagai Excel untuk modifikasi

### 13. Validation Testing

- [ ] **Range Validation**

  - [x] Tidak boleh ada gap antara grade (misal: A max 100, AB min 80 → gap 79.99-80.00)
  - [x] Tidak boleh ada overlap antara grade (misal: A min 85, AB min 84 → overlap 84-85)
  - [x] Total range harus cover 0-100 (atau sesuai kebijakan institusi)
  - [x] Error message jelas menunjukkan gap atau overlap yang terjadi

- [ ] **Bobot Validation**
  - [x] Bobot harus descending (A > AB > B > BC > C > D > E)
  - [x] Warning jika urutan bobot tidak logis (misal: B bobot 4.00, A bobot 3.00)
  - [x] Suggest bobot standar sesuai grade

### 14. Integration Testing

- [ ] **Integrasi dengan Modul Lain**
  - [x] Bobot nilai digunakan di modul input nilai dosen
  - [x] Konversi nilai angka ke huruf menggunakan bobot nilai ini
  - [x] Perhitungan IPK menggunakan bobot dari setting ini
  - [x] Perubahan bobot nilai trigger recalculate IPK mahasiswa (optional, dengan job queue)
  - [x] Export KHS menggunakan grade sesuai bobot nilai ini

### 15. Performance Testing

- [ ] **Query Optimization**
  - [x] Eager loading relationship: programStudi
  - [x] Index database untuk column: program_studi_id, grade, is_active
  - [x] Cache bobot nilai per program studi (jarang berubah)
  - [x] Response time ≤ 1 detik untuk load halaman
  - [x] Recalculate IPK dengan job queue untuk avoid blocking

### 16. Edge Cases

- [ ] **Kondisi Khusus**
  - [x] Handle program studi baru tanpa bobot nilai (suggest apply template)
  - [x] Handle perubahan bobot nilai di tengah semester (warning dampak)
  - [x] Handle bobot nilai inactive (tidak boleh digunakan untuk nilai baru)
  - [x] Handle delete program studi (soft delete bobot nilai terkait)
  - [x] Handle concurrent edit (dua user edit bersamaan) dengan lock mechanism

### 17. Accessibility & UI

- [ ] **Responsive Design**

  - [x] Table responsive di desktop
  - [x] Table scrollable horizontal di tablet
  - [x] Card view untuk mobile
  - [x] Form accessible di semua device

- [ ] **Visual Feedback**
  - [x] Grade badge dengan warna berbeda (A=hijau, E=merah)
  - [x] Hover effect di row table
  - [x] Loading indicator saat save/update
  - [x] Success/error message dengan toast notification

---

## Related Documentation

- [01_DASHBOARD.md](./01_DASHBOARD.md)
- [02_MONITORING_NILAI_IPK.md](./02_MONITORING_NILAI_IPK.md)
- [03_REVISI_NILAI.md](./03_REVISI_NILAI.md)
- [../DOSEN/03_MATA_KULIAH_DETAIL.md](../DOSEN/03_MATA_KULIAH_DETAIL.md)
- [../MAHASISWA/02_KHS.md](../MAHASISWA/02_KHS.md)
- [BOBOT_NILAI_GUIDE.md](../../../BOBOT_NILAI_GUIDE.md)
