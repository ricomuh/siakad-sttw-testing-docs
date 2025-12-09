# Testing: Approval Yudisium & KHS

**Status**: 🟡 In Progress  
**Role**: Ketua (Ketua Sekolah Tinggi)  
**Route**: `/ketua/approval`  
**Controller**: `App\Http\Controllers\Ketua\ApprovalController`  
**Model**: ApprovalYudisium, Mahasiswa, NilaiMahasiswa, ProgramStudi

---

## Checklist Testing

### 1. Operations to Test

- [ ] **Index Approval**

  - Method: `GET /ketua/approval`
  - Action: `ApprovalController@index`
  - Menampilkan daftar approval yudisium
  - Response: View dengan list approval

- [ ] **Show Detail Approval**

  - Method: `GET /ketua/approval/{approval}`
  - Action: `ApprovalController@show`
  - Menampilkan detail approval yudisium
  - Response: View dengan detail mahasiswa dan transkrip

- [ ] **Approve Yudisium**

  - Method: `POST /ketua/approval/{approval}/approve`
  - Action: `ApprovalController@approve`
  - Menyetujui yudisium mahasiswa
  - Response: Redirect dengan success message

- [ ] **Reject Yudisium**
  - Method: `POST /ketua/approval/{approval}/reject`
  - Action: `ApprovalController@reject`
  - Menolak yudisium mahasiswa
  - Response: Redirect dengan success message

### 2. Authorization Testing

- [ ] **Access Control**
  - [x] User dengan role `ketua` dapat mengakses semua operasi
  - [x] User tanpa role `ketua` mendapat 403 Forbidden
  - [x] Guest user redirect ke halaman login
  - [x] User dengan role `waket1` tidak dapat approve yudisium
  - [x] User dengan role `dosen` tidak dapat approve yudisium
  - [x] User dengan role `mahasiswa` tidak dapat akses

### 3. Index View - Filter & Search

- [ ] **Filter Status**

  - [x] Tabs filter status: "Pending", "Disetujui", "Ditolak", "Semua"
  - [x] Default tab "Pending" aktif saat pertama load
  - [x] Count badge jumlah per status ditampilkan
  - [x] Data ter-filter sesuai status yang dipilih
  - [x] Tab aktif di-highlight

- [ ] **Filter Program Studi**

  - [x] Dropdown program studi tersedia
  - [x] Filtering approval berdasarkan program studi mahasiswa
  - [x] Option "Semua Program Studi" tersedia
  - [x] Multiple filter dapat dikombinasikan

- [ ] **Filter Predikat**

  - [x] Dropdown predikat: Cum Laude, Sangat Memuaskan, Memuaskan
  - [x] Filtering approval berdasarkan predikat kelulusan
  - [x] Option "Semua Predikat" tersedia

- [ ] **Search**
  - [x] Input search tersedia (placeholder: "Cari NIM atau Nama Mahasiswa")
  - [x] Search by NIM berfungsi
  - [x] Search by nama mahasiswa berfungsi
  - [x] Search case-insensitive

### 4. Table Display

- [ ] **Column Informasi**

  - [x] Tanggal Pengajuan ditampilkan (format: DD MMM YYYY)
  - [x] NIM Mahasiswa ditampilkan
  - [x] Nama Mahasiswa ditampilkan
  - [x] Program Studi ditampilkan
  - [x] Angkatan ditampilkan
  - [x] Masa Studi ditampilkan (dalam tahun, misal: 4.0 tahun)

- [ ] **Column Akademik**

  - [x] IPK (Indeks Prestasi Kumulatif) ditampilkan dengan 2 desimal
  - [x] Total SKS Lulus ditampilkan
  - [x] Predikat Kelulusan ditampilkan dengan badge:
    - Cum Laude (hijau) - IPK ≥ 3.50
    - Sangat Memuaskan (biru) - IPK 3.00 - 3.49
    - Memuaskan (kuning) - IPK 2.50 - 2.99
  - [x] Status Akademik terakhir ditampilkan (Aktif/Lulus)

- [ ] **Column Status**

  - [x] Badge status approval: Pending (kuning), Disetujui (hijau), Ditolak (merah)
  - [x] Tanggal approval/reject ditampilkan (jika sudah diproses)
  - [x] Nama approver ditampilkan (jika sudah diproses)

- [ ] **Column Aksi**
  - [x] Button "Detail" tersedia untuk semua approval
  - [x] Button "Approve" tersedia hanya untuk status Pending
  - [x] Button "Reject" tersedia hanya untuk status Pending
  - [x] Button disabled untuk approval yang sudah Disetujui/Ditolak

### 5. Detail Approval

- [ ] **Informasi Mahasiswa**

  - [x] Foto mahasiswa ditampilkan (jika ada)
  - [x] NIM ditampilkan
  - [x] Nama lengkap mahasiswa ditampilkan
  - [x] Program Studi ditampilkan
  - [x] Angkatan ditampilkan
  - [x] Tempat, Tanggal Lahir ditampilkan
  - [x] Tanggal masuk dan tanggal lulus ditampilkan
  - [x] Masa studi ditampilkan (dalam tahun dan bulan)

- [ ] **Ringkasan Akademik**

  - [x] IPK Kumulatif ditampilkan (font besar, bold)
  - [x] Total SKS Lulus ditampilkan
  - [x] Total SKS Wajib ditampilkan
  - [x] Predikat Kelulusan ditampilkan dengan badge besar
  - [x] Status: "Memenuhi Syarat Yudisium" atau "Tidak Memenuhi Syarat"

- [ ] **Transkrip Nilai**

  - [x] Tabel transkrip nilai per semester ditampilkan
  - [x] Grouping per semester (Semester 1-8)
  - [x] Column: Kode MK, Nama Mata Kuliah, SKS, Grade (A-E), Bobot (0-4)
  - [x] Nilai setiap mata kuliah ditampilkan
  - [x] IPS (Indeks Prestasi Semester) ditampilkan per semester
  - [x] SKS Semester ditampilkan per semester
  - [x] Highlight mata kuliah dengan grade D atau E (warning color)

- [ ] **Verifikasi Kelengkapan**
  - [x] Checklist "Semua Mata Kuliah Wajib Lulus" (✓ atau ✗)
  - [x] Checklist "Tidak Ada Nilai D/E di Mata Kuliah Wajib" (✓ atau ✗)
  - [x] Checklist "IPK ≥ 2.00" (✓ atau ✗)
  - [x] Checklist "Tidak Ada Tunggakan Administrasi" (✓ atau ✗)
  - [x] Checklist "Tidak Ada Tunggakan Perpustakaan" (✓ atau ✗)
  - [x] Semua checklist harus ✓ untuk bisa di-approve

### 6. Approve Yudisium

- [ ] **Approval Form**

  - [x] Modal konfirmasi muncul sebelum approve
  - [x] Summary mahasiswa dan IPK ditampilkan di modal
  - [x] Input catatan approval (opsional) tersedia
  - [x] Checkbox konfirmasi: "Saya menyatakan bahwa mahasiswa ini memenuhi syarat yudisium"
  - [x] Button "Ya, Setujui" dan "Batal"

- [ ] **Approval Process**

  - [x] Status approval berubah dari "Pending" menjadi "Disetujui"
  - [x] Field `approved_by` diisi dengan user ID Ketua
  - [x] Field `approved_at` diisi dengan timestamp saat approve
  - [x] Field `catatan_approval` diisi jika ada catatan
  - [x] Database transaction untuk ensure atomicity

- [ ] **Update Status Mahasiswa**

  - [x] Status akademik mahasiswa berubah dari "Aktif" menjadi "Lulus"
  - [x] Field `tanggal_lulus` diisi dengan tanggal approval
  - [x] Field `predikat_kelulusan` diisi (Cum Laude/Sangat Memuaskan/Memuaskan)
  - [x] Field `nomor_ijazah` di-generate (format: NOMOR/TAHUN/PRODI)
  - [x] Field `masa_studi` dihitung dari tanggal masuk ke tanggal lulus

- [ ] **Generate Dokumen**

  - [x] Transkrip Nilai PDF ter-generate otomatis
  - [x] Surat Keterangan Lulus PDF ter-generate otomatis
  - [x] Nomor ijazah dan nomor transkrip ter-assign
  - [x] Dokumen tersimpan di storage dengan path terstruktur

- [ ] **Notifikasi**
  - [x] Notifikasi email dikirim ke mahasiswa
  - [x] Notifikasi in-app muncul di dashboard mahasiswa
  - [x] Email berisi ucapan selamat, predikat, dan link download transkrip
  - [x] Notifikasi juga dikirim ke Kaprodi untuk informasi

### 7. Reject Yudisium

- [ ] **Rejection Form**

  - [x] Modal konfirmasi muncul sebelum reject
  - [x] Summary mahasiswa ditampilkan di modal
  - [x] Textarea alasan penolakan (wajib, min 20 karakter)
  - [x] Info checklist yang tidak terpenuhi ditampilkan otomatis
  - [x] Button "Ya, Tolak" dan "Batal"

- [ ] **Rejection Process**

  - [x] Status approval berubah dari "Pending" menjadi "Ditolak"
  - [x] Field `rejected_by` diisi dengan user ID Ketua
  - [x] Field `rejected_at` diisi dengan timestamp saat reject
  - [x] Field `alasan_penolakan` diisi (required)
  - [x] Database transaction untuk ensure atomicity

- [ ] **Status Mahasiswa Tidak Berubah**

  - [x] Status akademik mahasiswa tetap "Aktif" (tidak berubah menjadi Lulus)
  - [x] Field `tanggal_lulus` tetap NULL
  - [x] Field `predikat_kelulusan` tetap NULL
  - [x] Mahasiswa dapat mengajukan ulang setelah memenuhi syarat

- [ ] **Notifikasi Penolakan**
  - [x] Notifikasi email dikirim ke mahasiswa
  - [x] Email berisi alasan penolakan dari Ketua
  - [x] Notifikasi in-app muncul di dashboard mahasiswa
  - [x] Mahasiswa dapat melihat detail alasan penolakan
  - [x] Notifikasi juga dikirim ke Kaprodi dan Dosen PA

### 8. Validation Rules

- [ ] **Approval Validation**

  - [x] Hanya approval dengan status "Pending" yang dapat di-approve
  - [x] Approval yang sudah "Disetujui" tidak dapat di-approve lagi
  - [x] Approval yang sudah "Ditolak" tidak dapat di-approve
  - [x] Semua checklist kelengkapan harus terpenuhi (✓)
  - [x] IPK mahasiswa harus ≥ 2.00
  - [x] Error message jelas jika validasi gagal

- [ ] **Rejection Validation**

  - [x] Hanya approval dengan status "Pending" yang dapat di-reject
  - [x] Alasan penolakan wajib diisi (min 20 karakter)
  - [x] Approval yang sudah "Disetujui" tidak dapat di-reject
  - [x] Approval yang sudah "Ditolak" tidak dapat di-reject lagi

- [ ] **Data Integrity**
  - [x] IPK mahasiswa valid (0.00 - 4.00)
  - [x] Total SKS lulus valid (≥ SKS minimum kelulusan, misal 144 SKS untuk S1)
  - [x] Mahasiswa tidak memiliki tunggakan administrasi
  - [x] Mahasiswa sudah mengembalikan semua buku perpustakaan

### 9. Sorting & Pagination

- [ ] **Sorting**

  - [x] Column "Tanggal Pengajuan" dapat di-sort (terbaru/terlama)
  - [x] Column "IPK" dapat di-sort (tertinggi/terendah)
  - [x] Column "Nama" dapat di-sort alphabetically
  - [x] Default sort: Tanggal Pengajuan ascending (terlama dulu = prioritas)
  - [x] Icon arrow up/down sesuai sort direction

- [ ] **Pagination**
  - [x] Data approval di-pagination (20 per halaman)
  - [x] Total records ditampilkan
  - [x] Pagination links berfungsi
  - [x] Filter dan search persist saat pindah halaman

### 10. Bulk Actions

- [ ] **Select Multiple**

  - [x] Checkbox untuk select individual approval
  - [x] Checkbox "Select All" di header table
  - [x] Counter jumlah approval yang dipilih
  - [x] Button "Approve Selected" tersedia jika ada yang dipilih
  - [x] Button "Export Selected" untuk export transkrip massal

- [ ] **Bulk Approve**
  - [x] Modal konfirmasi bulk approve muncul
  - [x] List approval yang akan di-approve ditampilkan
  - [x] Warning jika ada approval yang tidak memenuhi syarat (skip otomatis)
  - [x] Proses approve untuk semua yang memenuhi syarat
  - [x] Success message menampilkan jumlah berhasil dan gagal
  - [x] Generate dokumen untuk semua yang di-approve (async queue job)

### 11. Export & Print

- [ ] **Export Excel**

  - [x] Button "Export Excel" di index page
  - [x] Excel berisi semua approval sesuai filter aktif
  - [x] Column: Tanggal, NIM, Nama, Prodi, IPK, SKS, Predikat, Status
  - [x] Format excel rapi dengan header bold dan border
  - [x] File naming: `Approval_Yudisium_{TanggalWaktu}.xlsx`

- [ ] **Print Transkrip**

  - [x] Button "Print Transkrip" di halaman detail approval
  - [x] PDF transkrip ter-generate sesuai format resmi institusi
  - [x] PDF berisi: Identitas mahasiswa, transkrip nilai per semester, IPK, predikat
  - [x] Header PDF: Logo institusi, nama institusi, alamat
  - [x] Footer PDF: Signature Ketua dengan QR code verifikasi
  - [x] Watermark "SALINAN" jika belum di-approve

- [ ] **Print Surat Keterangan Lulus**
  - [x] Button "Print SKL" di halaman detail approval (hanya jika sudah di-approve)
  - [x] PDF SKL ter-generate dengan format resmi
  - [x] Nomor surat ter-generate otomatis
  - [x] Tanggal surat sesuai tanggal approval

### 12. Statistics & Reports

- [ ] **Summary Statistics**
  - [x] Card "Total Pending" dengan count approval pending
  - [x] Card "Rata-rata IPK Lulusan" bulan ini
  - [x] Card "Total Disetujui" bulan ini dengan breakdown predikat
  - [x] Card "Rata-rata Masa Studi" dalam tahun
  - [x] Chart trend approval per bulan (12 bulan terakhir)

### 13. Performance Testing

- [ ] **Query Optimization**
  - [x] Eager loading relationship: mahasiswa, programStudi, nilaiMahasiswa
  - [x] Index database untuk column: status, created_at, mahasiswa_id
  - [x] Pagination untuk hindari load semua data
  - [x] Cache count badge status untuk performa
  - [x] Response time ≤ 3 detik untuk load halaman dengan 100+ approval
  - [x] Generate dokumen dengan queue job (async) untuk avoid blocking

### 14. Notifikasi Real-time

- [ ] **Notifikasi System**
  - [x] Notifikasi muncul di sidebar Ketua jika ada approval baru
  - [x] Badge count notifikasi pending di menu sidebar
  - [x] Notifikasi dapat diklik untuk langsung ke halaman approval
  - [x] Sound notification (opsional) saat ada approval baru
  - [x] Browser push notification jika user mengizinkan
  - [x] Email reminder setiap hari jika ada approval > 7 hari belum diproses

### 15. Integration Testing

- [ ] **Integrasi dengan Modul Lain**
  - [x] Data mahasiswa sync dengan modul master data mahasiswa
  - [x] Data nilai sync dengan modul KHS mahasiswa
  - [x] Status mahasiswa ter-update di seluruh sistem setelah approve
  - [x] Transkrip nilai dapat diakses mahasiswa di dashboard mereka
  - [x] Notifikasi terintegrasi dengan mail dan notification system

### 16. Edge Cases

- [ ] **Kondisi Khusus**
  - [x] Handle approval untuk mahasiswa dengan IPK < 2.00 (reject otomatis dengan pesan)
  - [x] Handle approval untuk mahasiswa dengan SKS < minimum (reject otomatis)
  - [x] Handle approval untuk mahasiswa dengan nilai D/E di mata kuliah wajib (warning, require confirmation)
  - [x] Handle concurrent approval (dua Ketua approve bersamaan) dengan lock mechanism
  - [x] Handle mahasiswa yang sudah lulus (approval tidak bisa dibuat lagi)
  - [x] Handle generate nomor ijazah duplikat (unique constraint)

### 17. Audit Trail & History

- [ ] **Change Log**
  - [x] Semua perubahan status approval dicatat di log table
  - [x] Log berisi: timestamp, user, action (approve/reject), status lama, status baru
  - [x] Log dapat diakses dari halaman detail approval
  - [x] Button "Lihat Riwayat" menampilkan modal history

### 18. Accessibility & UI

- [ ] **Responsive Design**

  - [x] Table responsive di desktop
  - [x] Table scrollable horizontal di tablet
  - [x] Card view untuk mobile (stack vertical)
  - [x] Modal accessible di semua device
  - [x] Button size adequate untuk touch di mobile

- [ ] **Visual Feedback**
  - [x] Badge predikat dengan warna berbeda (Cum Laude=hijau, dll)
  - [x] Loading indicator saat approve/reject
  - [x] Success/error message dengan toast notification
  - [x] Hover effect di row table
  - [x] Disabled state jelas untuk button yang tidak bisa diklik

---

## Related Documentation

- [01_DASHBOARD.md](./01_DASHBOARD.md)
- [../WAKET1/02_MONITORING_NILAI_IPK.md](../WAKET1/02_MONITORING_NILAI_IPK.md)
- [../MAHASISWA/02_KHS.md](../MAHASISWA/02_KHS.md)
- [../ADMIN/04_MASTER_DATA_MAHASISWA.md](../ADMIN/04_MASTER_DATA_MAHASISWA.md)
- [../ADMIN/16_MANAJEMEN_YUDISIUM.md](../ADMIN/16_MANAJEMEN_YUDISIUM.md)
