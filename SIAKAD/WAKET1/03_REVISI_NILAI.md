# Testing: Revisi Nilai (Approval)

**Status**: 🟡 In Progress  
**Role**: Waket1 (Wakil Ketua 1 - Bidang Akademik)  
**Route**: `/waket1/revisi-nilai`  
**Controller**: `App\Http\Controllers\Waket1\RevisiNilaiController`  
**Model**: RevisiNilai, NilaiMahasiswa, Mahasiswa, MataKuliah, Dosen

---

## Checklist Testing

### 1. Operations to Test

- [ ] **Index Revisi Nilai**

  - Method: `GET /waket1/revisi-nilai`
  - Action: `RevisiNilaiController@index`
  - Menampilkan daftar revisi nilai yang menunggu approval
  - Response: View dengan list revisi nilai

- [ ] **Show Detail Revisi**

  - Method: `GET /waket1/revisi-nilai/{revisiNilai}`
  - Action: `RevisiNilaiController@show`
  - Menampilkan detail revisi nilai
  - Response: View dengan detail revisi

- [ ] **Approve Revisi**

  - Method: `POST /waket1/revisi-nilai/{revisiNilai}/approve`
  - Action: `RevisiNilaiController@approve`
  - Menyetujui revisi nilai
  - Response: Redirect dengan success message

- [ ] **Reject Revisi**
  - Method: `POST /waket1/revisi-nilai/{revisiNilai}/reject`
  - Action: `RevisiNilaiController@reject`
  - Menolak revisi nilai
  - Response: Redirect dengan success message

### 2. Authorization Testing

- [ ] **Access Control**
  - [x] User dengan role `waket1` dapat mengakses semua operasi
  - [x] User tanpa role `waket1` mendapat 403 Forbidden
  - [x] Guest user redirect ke halaman login
  - [x] User dengan role `dosen` tidak dapat approve/reject
  - [x] User dengan role `admin` tidak dapat approve tanpa role waket1

### 3. Index View - Filter & Search

- [ ] **Filter Status**

  - [x] Tabs filter status tersedia: "Pending", "Disetujui", "Ditolak", "Semua"
  - [x] Default tab "Pending" aktif saat pertama load
  - [x] Count badge jumlah per status ditampilkan di tab
  - [x] Data ter-filter sesuai status yang dipilih
  - [x] Tab aktif di-highlight dengan warna berbeda

- [ ] **Filter Program Studi**

  - [x] Dropdown program studi tersedia
  - [x] Filtering revisi berdasarkan program studi mahasiswa
  - [x] Option "Semua Program Studi" tersedia
  - [x] Multiple filter dapat dikombinasikan

- [ ] **Filter Dosen Pengaju**

  - [x] Dropdown dosen tersedia
  - [x] Filtering revisi berdasarkan dosen yang mengajukan
  - [x] List dosen sorted alphabetically
  - [x] Option "Semua Dosen" tersedia

- [ ] **Search**
  - [x] Input search tersedia (placeholder: "Cari NIM, Nama Mahasiswa, atau Mata Kuliah")
  - [x] Search by NIM mahasiswa berfungsi
  - [x] Search by nama mahasiswa berfungsi
  - [x] Search by nama mata kuliah berfungsi
  - [x] Search case-insensitive

### 4. Table Display

- [ ] **Column Informasi**

  - [x] Tanggal Pengajuan ditampilkan (format: DD MMM YYYY HH:mm)
  - [x] NIM Mahasiswa ditampilkan
  - [x] Nama Mahasiswa ditampilkan
  - [x] Mata Kuliah ditampilkan (kode + nama)
  - [x] Dosen Pengaju ditampilkan
  - [x] Program Studi ditampilkan

- [ ] **Column Nilai**

  - [x] Nilai Lama ditampilkan (misal: B = 3.00)
  - [x] Nilai Baru yang diajukan ditampilkan (misal: A = 4.00)
  - [x] Selisih nilai ditampilkan dengan warna (hijau jika naik, merah jika turun)
  - [x] Arrow icon (↑ atau ↓) untuk perubahan nilai
  - [x] Format nilai: Huruf (Angka) misal: A (4.00)

- [ ] **Column Status**

  - [x] Badge status ditampilkan: Pending (kuning), Disetujui (hijau), Ditolak (merah)
  - [x] Status sesuai dengan field `status` di database
  - [x] Tooltip muncul saat hover badge menjelaskan arti status

- [ ] **Column Aksi**
  - [x] Button "Detail" tersedia untuk semua revisi
  - [x] Button "Approve" tersedia hanya untuk status Pending
  - [x] Button "Reject" tersedia hanya untuk status Pending
  - [x] Button disabled untuk revisi yang sudah Disetujui/Ditolak

### 5. Detail Revisi

- [ ] **Informasi Mahasiswa**

  - [x] Foto mahasiswa ditampilkan (jika ada)
  - [x] NIM ditampilkan
  - [x] Nama lengkap mahasiswa ditampilkan
  - [x] Program Studi ditampilkan
  - [x] Angkatan ditampilkan
  - [x] Status akademik ditampilkan

- [ ] **Informasi Mata Kuliah**

  - [x] Kode mata kuliah ditampilkan
  - [x] Nama mata kuliah ditampilkan
  - [x] SKS ditampilkan
  - [x] Semester ditampilkan
  - [x] Tahun akademik ditampilkan (misal: 2024/2025 Ganjil)

- [ ] **Informasi Dosen**

  - [x] NIP dosen pengaju ditampilkan
  - [x] Nama lengkap dosen ditampilkan
  - [x] Status sebagai dosen pengampu mata kuliah tersebut ditampilkan
  - [x] Tanggal pengajuan ditampilkan

- [ ] **Perubahan Nilai**
  - [x] Tabel perbandingan Nilai Lama vs Nilai Baru ditampilkan
  - [x] Komponen nilai yang berubah ditampilkan (UTS/UAS/Tugas/Total)
  - [x] Alasan revisi yang diinput dosen ditampilkan
  - [x] File pendukung (jika ada) dapat di-download
  - [x] Highlight perubahan dengan warna berbeda

### 6. Approve Revisi

- [ ] **Approval Form**

  - [x] Modal konfirmasi muncul sebelum approve
  - [x] Summary perubahan ditampilkan di modal
  - [x] Input catatan approval (opsional) tersedia
  - [x] Button "Ya, Setujui" dan "Batal" tersedia

- [ ] **Approval Process**

  - [x] Status revisi berubah dari "Pending" menjadi "Disetujui"
  - [x] Field `approved_by` diisi dengan user ID Waket1
  - [x] Field `approved_at` diisi dengan timestamp saat approve
  - [x] Field `catatan_approval` diisi jika ada catatan
  - [x] Database transaction untuk ensure atomicity

- [ ] **Update Nilai Mahasiswa**

  - [x] Record `NilaiMahasiswa` ter-update dengan nilai baru
  - [x] Komponen nilai (UTS/UAS/Tugas) ter-update sesuai revisi
  - [x] Nilai akhir ter-recalculate dengan benar
  - [x] IPK mahasiswa ter-recalculate dengan benar
  - [x] IPS semester terkait ter-recalculate

- [ ] **Log & Audit Trail**
  - [x] Log perubahan nilai disimpan di `NilaiLog` table
  - [x] Log berisi nilai lama, nilai baru, user yang approve, timestamp
  - [x] Activity log dicatat untuk audit trail
  - [x] Notifikasi email/sistem dikirim ke dosen pengaju
  - [x] Notifikasi dikirim ke mahasiswa terkait

### 7. Reject Revisi

- [ ] **Rejection Form**

  - [x] Modal konfirmasi muncul sebelum reject
  - [x] Summary revisi ditampilkan di modal
  - [x] Input alasan penolakan (wajib) tersedia
  - [x] Textarea untuk alasan dengan min 10 karakter
  - [x] Button "Ya, Tolak" dan "Batal" tersedia

- [ ] **Rejection Process**

  - [x] Status revisi berubah dari "Pending" menjadi "Ditolak"
  - [x] Field `rejected_by` diisi dengan user ID Waket1
  - [x] Field `rejected_at` diisi dengan timestamp saat reject
  - [x] Field `alasan_penolakan` diisi dengan alasan (required)
  - [x] Database transaction untuk ensure atomicity

- [ ] **Nilai Tidak Berubah**

  - [x] Record `NilaiMahasiswa` TIDAK ter-update (tetap nilai lama)
  - [x] IPK mahasiswa TIDAK berubah
  - [x] IPS semester terkait TIDAK berubah
  - [x] Log penolakan disimpan di `RevisiNilaiLog` table

- [ ] **Notifikasi Penolakan**
  - [x] Notifikasi email/sistem dikirim ke dosen pengaju
  - [x] Notifikasi berisi alasan penolakan dari Waket1
  - [x] Dosen dapat melihat alasan penolakan di halaman revisi nilai mereka
  - [x] Dosen dapat mengajukan ulang jika perlu

### 8. Validation Rules

- [ ] **Approval Validation**

  - [x] Hanya revisi dengan status "Pending" yang dapat di-approve
  - [x] Revisi yang sudah "Disetujui" tidak dapat di-approve lagi
  - [x] Revisi yang sudah "Ditolak" tidak dapat di-approve
  - [x] Error message jelas jika validasi gagal

- [ ] **Rejection Validation**

  - [x] Hanya revisi dengan status "Pending" yang dapat di-reject
  - [x] Alasan penolakan wajib diisi (min 10 karakter)
  - [x] Revisi yang sudah "Disetujui" tidak dapat di-reject
  - [x] Revisi yang sudah "Ditolak" tidak dapat di-reject lagi

- [ ] **Data Integrity**
  - [x] Pastikan nilai baru valid sesuai range (0.00 - 4.00)
  - [x] Pastikan grade (A, B, C, D, E) sesuai dengan nilai angka
  - [x] Pastikan mahasiswa masih aktif (tidak DO/Lulus)
  - [x] Pastikan mata kuliah masih aktif di semester terkait

### 9. Sorting & Pagination

- [ ] **Sorting**

  - [x] Column "Tanggal Pengajuan" dapat di-sort (terbaru/terlama)
  - [x] Column "NIM" dapat di-sort
  - [x] Column "Mata Kuliah" dapat di-sort alphabetically
  - [x] Default sort: Tanggal Pengajuan descending (terbaru dulu)
  - [x] Icon arrow up/down ditampilkan sesuai sort direction

- [ ] **Pagination**
  - [x] Data revisi di-pagination (20 per halaman)
  - [x] Total records ditampilkan
  - [x] Pagination links berfungsi (Previous, 1, 2, 3, Next)
  - [x] Filter dan search persist saat pindah halaman

### 10. Bulk Actions

- [ ] **Select Multiple**

  - [x] Checkbox untuk select individual revisi
  - [x] Checkbox "Select All" di header table
  - [x] Counter jumlah revisi yang dipilih ditampilkan
  - [x] Button "Approve Selected" tersedia jika ada yang dipilih
  - [x] Button "Reject Selected" tersedia jika ada yang dipilih

- [ ] **Bulk Approve**

  - [x] Modal konfirmasi bulk approve muncul
  - [x] List revisi yang akan di-approve ditampilkan
  - [x] Proses approve untuk semua revisi yang dipilih
  - [x] Success message menampilkan jumlah revisi yang berhasil di-approve
  - [x] Gagal approve partial tidak mempengaruhi yang lain (independent transaction)

- [ ] **Bulk Reject**
  - [x] Modal konfirmasi bulk reject muncul
  - [x] Input satu alasan penolakan untuk semua revisi yang dipilih
  - [x] Proses reject untuk semua revisi yang dipilih
  - [x] Success message menampilkan jumlah revisi yang berhasil di-reject

### 11. Export & Print

- [ ] **Export Excel**

  - [x] Button "Export Excel" tersedia
  - [x] Excel berisi semua revisi sesuai filter aktif
  - [x] Column: Tanggal, NIM, Nama, Mata Kuliah, Dosen, Nilai Lama, Nilai Baru, Status
  - [x] Format excel rapi dengan header bold dan border
  - [x] File naming: `Revisi_Nilai_{TanggalWaktu}.xlsx`

- [ ] **Print Detail**
  - [x] Button "Print" di halaman detail revisi
  - [x] PDF detail revisi ter-generate
  - [x] PDF berisi semua informasi mahasiswa, mata kuliah, nilai lama/baru
  - [x] PDF berisi header institusi dan logo
  - [x] PDF berisi signature Waket1 jika sudah approved

### 12. Performance Testing

- [ ] **Query Optimization**
  - [x] Eager loading relationship: mahasiswa, mataKuliah, dosen, nilaiMahasiswa
  - [x] Index database untuk column: status, created_at, mahasiswa_id
  - [x] Pagination untuk hindari load semua data
  - [x] Cache count badge status untuk performa
  - [x] Response time ≤ 2 detik untuk load halaman dengan 500+ revisi

### 13. Notifikasi Real-time

- [ ] **Notifikasi System**
  - [x] Notifikasi muncul di sidebar Waket1 jika ada revisi baru
  - [x] Badge count notifikasi pending di menu sidebar
  - [x] Notifikasi dapat diklik untuk langsung ke halaman revisi
  - [x] Sound notification (opsional) saat ada revisi baru
  - [x] Browser push notification jika user mengizinkan

### 14. Integration Testing

- [ ] **Integrasi dengan Modul Lain**
  - [x] Revisi nilai diajukan dari modul Dosen → Mata Kuliah Detail
  - [x] Nilai ter-update di modul KHS mahasiswa setelah approved
  - [x] IPK ter-recalculate di modul Monitoring Nilai
  - [x] Notifikasi terkirim ke dashboard dosen dan mahasiswa
  - [x] Log audit trail terintegrasi dengan sistem log

### 15. Edge Cases

- [ ] **Kondisi Khusus**
  - [x] Handle revisi untuk mahasiswa yang sudah lulus (reject otomatis)
  - [x] Handle revisi untuk mata kuliah yang sudah lebih dari 1 tahun (warning)
  - [x] Handle revisi dengan nilai baru sama dengan nilai lama (reject otomatis)
  - [x] Handle concurrent approval (dua Waket1 approve bersamaan) dengan lock mechanism
  - [x] Handle revisi untuk semester yang sudah di-lock (reject otomatis)

### 16. Accessibility & UI

- [ ] **Responsive Design**

  - [x] Table responsive di desktop
  - [x] Table scrollable horizontal di tablet
  - [x] Card view untuk mobile (stack vertical)
  - [x] Modal accessible di semua device
  - [x] Button size adequate untuk touch di mobile

- [ ] **Keyboard Navigation**
  - [x] Tab navigation berfungsi untuk semua form input
  - [x] Enter untuk submit form
  - [x] Esc untuk close modal
  - [x] Arrow keys untuk navigasi table (opsional)

---

## Related Documentation

- [01_DASHBOARD.md](./01_DASHBOARD.md)
- [02_MONITORING_NILAI_IPK.md](./02_MONITORING_NILAI_IPK.md)
- [../DOSEN/03_MATA_KULIAH_DETAIL.md](../DOSEN/03_MATA_KULIAH_DETAIL.md)
- [../MAHASISWA/02_KHS.md](../MAHASISWA/02_KHS.md)
- [../ADMIN/13_PRESENSI_MAHASISWA.md](../ADMIN/13_PRESENSI_MAHASISWA.md)
