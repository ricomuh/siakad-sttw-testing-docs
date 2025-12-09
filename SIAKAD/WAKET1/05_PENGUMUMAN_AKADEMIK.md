# Testing: Pengumuman Akademik

**Status**: 🟡 In Progress  
**Role**: Waket1 (Wakil Ketua 1 - Bidang Akademik)  
**Route**: `/waket1/pengumuman`  
**Controller**: `App\Http\Controllers\Waket1\PengumumanAkademikController`  
**Model**: PengumumanAkademik, ProgramStudi

---

## Checklist Testing

### 1. Operations to Test

- [ ] **Index Pengumuman**
  - Method: `GET /waket1/pengumuman`
  - Action: `PengumumanAkademikController@index`
  - Menampilkan daftar pengumuman akademik
  - Response: View dengan list pengumuman

- [ ] **Create Pengumuman**
  - Method: `GET /waket1/pengumuman/create`
  - Action: `PengumumanAkademikController@create`
  - Menampilkan form tambah pengumuman
  - Response: View form create

- [ ] **Store Pengumuman**
  - Method: `POST /waket1/pengumuman`
  - Action: `PengumumanAkademikController@store`
  - Menyimpan pengumuman baru
  - Response: Redirect dengan success message

- [ ] **Show Detail Pengumuman**
  - Method: `GET /waket1/pengumuman/{pengumuman}`
  - Action: `PengumumanAkademikController@show`
  - Menampilkan detail pengumuman
  - Response: View detail

- [ ] **Edit Pengumuman**
  - Method: `GET /waket1/pengumuman/{pengumuman}/edit`
  - Action: `PengumumanAkademikController@edit`
  - Menampilkan form edit pengumuman
  - Response: View form edit

- [ ] **Update Pengumuman**
  - Method: `PATCH /waket1/pengumuman/{pengumuman}`
  - Action: `PengumumanAkademikController@update`
  - Mengupdate pengumuman
  - Response: Redirect dengan success message

- [ ] **Delete Pengumuman**
  - Method: `DELETE /waket1/pengumuman/{pengumuman}`
  - Action: `PengumumanAkademikController@destroy`
  - Menghapus pengumuman
  - Response: Redirect dengan success message

### 2. Authorization Testing

- [ ] **Access Control**
  - [x] User dengan role `waket1` dapat mengakses semua operasi
  - [x] User tanpa role `waket1` mendapat 403 Forbidden untuk create/edit/delete
  - [x] Guest user redirect ke halaman login
  - [x] User dengan role `dosen` dapat melihat (read-only)
  - [x] User dengan role `mahasiswa` dapat melihat (read-only)
  - [x] User dengan role `admin` dapat melihat (read-only)

### 3. Index View - Display

- [ ] **Table Display**
  - [x] Column "Judul" ditampilkan
  - [x] Column "Kategori" ditampilkan (Akademik/Beasiswa/Event/Pengumuman Umum/Penting)
  - [x] Column "Target" ditampilkan (Semua/Program Studi Tertentu/Angkatan Tertentu)
  - [x] Column "Tanggal Mulai" ditampilkan
  - [x] Column "Tanggal Selesai" ditampilkan
  - [x] Column "Status" ditampilkan (Draft/Aktif/Kadaluarsa)
  - [x] Column "Prioritas" ditampilkan (Biasa/Penting/Mendesak) dengan icon
  - [x] Column "Aksi" berisi button View, Edit, Delete

- [ ] **Status Badge**
  - [x] Badge "Draft" dengan warna abu-abu
  - [x] Badge "Aktif" dengan warna hijau
  - [x] Badge "Kadaluarsa" dengan warna merah
  - [x] Auto-update status berdasarkan tanggal (Aktif jika sekarang antara tanggal mulai dan selesai)

- [ ] **Prioritas Icon**
  - [x] Icon 🔵 (biru) untuk prioritas "Biasa"
  - [x] Icon 🟡 (kuning) untuk prioritas "Penting"
  - [x] Icon 🔴 (merah) untuk prioritas "Mendesak"
  - [x] Tooltip muncul saat hover icon

### 4. Filter & Search

- [ ] **Filter Status**
  - [x] Tabs filter status: "Aktif", "Draft", "Kadaluarsa", "Semua"
  - [x] Default tab "Aktif" saat pertama load
  - [x] Count badge jumlah per status ditampilkan
  - [x] Data ter-filter sesuai status yang dipilih

- [ ] **Filter Kategori**
  - [x] Dropdown kategori: Akademik, Beasiswa, Event, Pengumuman Umum, Penting
  - [x] Option "Semua Kategori" tersedia
  - [x] Data ter-filter sesuai kategori yang dipilih

- [ ] **Filter Prioritas**
  - [x] Dropdown prioritas: Biasa, Penting, Mendesak
  - [x] Option "Semua Prioritas" tersedia
  - [x] Data ter-filter sesuai prioritas yang dipilih

- [ ] **Search**
  - [x] Input search tersedia (placeholder: "Cari judul atau konten pengumuman")
  - [x] Search by judul berfungsi
  - [x] Search by konten berfungsi
  - [x] Search case-insensitive

### 5. Create Pengumuman

- [ ] **Form Fields**
  - [x] Input "Judul" (required, max 255 karakter)
  - [x] Textarea "Konten" (required, min 10 karakter) dengan rich text editor
  - [x] Dropdown "Kategori" (required): Akademik, Beasiswa, Event, Pengumuman Umum, Penting
  - [x] Dropdown "Prioritas" (required): Biasa, Penting, Mendesak
  - [x] Date picker "Tanggal Mulai" (required)
  - [x] Date picker "Tanggal Selesai" (required)
  - [x] Radio "Target": Semua, Program Studi Tertentu, Angkatan Tertentu
  - [x] Dropdown "Program Studi" (conditional, muncul jika target = Program Studi Tertentu)
  - [x] Input "Angkatan" (conditional, muncul jika target = Angkatan Tertentu)
  - [x] Upload "File Lampiran" (optional, max 5MB, types: pdf/doc/docx/jpg/png)
  - [x] Input "Link Eksternal" (optional, URL format)
  - [x] Checkbox "Kirim Notifikasi Email" (default unchecked)
  - [x] Checkbox "Tampilkan di Banner Homepage" (default unchecked)
  - [x] Button "Simpan sebagai Draft" dan "Publish"

- [ ] **Rich Text Editor**
  - [x] Bold, Italic, Underline formatting
  - [x] Heading (H1, H2, H3)
  - [x] Bullet list dan Numbered list
  - [x] Link insertion
  - [x] Image upload (inline)
  - [x] Code block
  - [x] Text alignment (left, center, right)

### 6. Validation Rules

- [ ] **Create/Update Validation**
  - [x] Judul wajib diisi, max 255 karakter
  - [x] Konten wajib diisi, min 10 karakter
  - [x] Kategori wajib dipilih
  - [x] Prioritas wajib dipilih
  - [x] Tanggal Mulai wajib diisi, format date
  - [x] Tanggal Selesai wajib diisi, format date
  - [x] Tanggal Selesai harus lebih besar atau sama dengan Tanggal Mulai
  - [x] File lampiran max 5MB, types: pdf, doc, docx, jpg, png
  - [x] Link eksternal harus format URL valid (jika diisi)
  - [x] Program Studi wajib dipilih jika Target = Program Studi Tertentu
  - [x] Angkatan wajib diisi jika Target = Angkatan Tertentu

### 7. Edit Pengumuman

- [ ] **Form Pre-fill**
  - [x] Form ter-pre-fill dengan data pengumuman yang akan diedit
  - [x] Rich text editor ter-load dengan konten HTML
  - [x] File lampiran existing ditampilkan dengan link download
  - [x] Target dan conditional fields (Program Studi/Angkatan) ter-set dengan benar
  - [x] Button "Simpan Perubahan" dan "Batal"

- [ ] **Update File Lampiran**
  - [x] File lampiran lama ditampilkan (jika ada)
  - [x] Button "Hapus File" untuk hapus file lama
  - [x] Upload file baru untuk replace file lama
  - [x] Jika upload baru, file lama otomatis terhapus dari storage

### 8. Delete Pengumuman

- [ ] **Delete Confirmation**
  - [x] Modal konfirmasi muncul sebelum delete
  - [x] Info pengumuman yang akan dihapus ditampilkan (judul, tanggal)
  - [x] Warning: "Pengumuman ini akan dihapus permanen"
  - [x] Button "Ya, Hapus" dan "Batal"

- [ ] **Delete Process**
  - [x] Soft delete: Set `deleted_at` timestamp
  - [x] File lampiran dihapus dari storage
  - [x] Success message ditampilkan
  - [x] Redirect ke index page

### 9. Show Detail Pengumuman

- [ ] **Detail View**
  - [x] Judul pengumuman ditampilkan (font besar, bold)
  - [x] Badge kategori dan prioritas ditampilkan
  - [x] Tanggal mulai dan selesai ditampilkan
  - [x] Konten pengumuman ditampilkan dengan HTML rendering
  - [x] File lampiran dapat di-download (jika ada)
  - [x] Link eksternal dapat diklik (jika ada)
  - [x] Penulis (Waket1 name) ditampilkan
  - [x] Tanggal dibuat dan terakhir diupdate ditampilkan
  - [x] Jumlah views ditampilkan (track berapa kali dilihat)

- [ ] **Comments Section**
  - [x] Section komentar tersedia (opsional)
  - [x] User dapat menambahkan komentar
  - [x] List komentar ditampilkan chronologically
  - [x] Nama dan timestamp komentar ditampilkan
  - [x] Waket1 dapat menghapus komentar yang tidak pantas

### 10. Publish vs Draft

- [ ] **Save as Draft**
  - [x] Button "Simpan sebagai Draft" di form create/edit
  - [x] Status pengumuman = "Draft"
  - [x] Pengumuman tidak tampil di dashboard mahasiswa/dosen
  - [x] Hanya Waket1 yang dapat melihat draft
  - [x] Badge "Draft" ditampilkan di list

- [ ] **Publish**
  - [x] Button "Publish" di form create/edit atau di list (untuk draft)
  - [x] Modal konfirmasi publish muncul
  - [x] Status pengumuman = "Aktif" (jika tanggal mulai ≤ hari ini ≤ tanggal selesai)
  - [x] Pengumuman tampil di dashboard target users
  - [x] Notifikasi dikirim jika checkbox "Kirim Notifikasi Email" dicentang

### 11. Notifikasi System

- [ ] **Send Notification**
  - [x] Checkbox "Kirim Notifikasi Email" di form
  - [x] Email notifikasi dikirim ke target users saat publish
  - [x] Target: Semua mahasiswa/dosen (jika Target = Semua)
  - [x] Target: Mahasiswa di program studi tertentu (jika Target = Program Studi)
  - [x] Target: Mahasiswa angkatan tertentu (jika Target = Angkatan)
  - [x] Email berisi judul, excerpt konten, link ke detail pengumuman
  - [x] In-app notification juga muncul di dashboard target users

- [ ] **Banner Homepage**
  - [x] Checkbox "Tampilkan di Banner Homepage" di form
  - [x] Pengumuman tampil di banner rotating carousel homepage
  - [x] Banner menampilkan judul, excerpt, dan badge prioritas
  - [x] Klik banner redirect ke detail pengumuman
  - [x] Max 5 pengumuman di banner (prioritas tertinggi dulu)

### 12. Sorting & Pagination

- [ ] **Sorting**
  - [x] Column "Tanggal Mulai" dapat di-sort (terbaru/terlama)
  - [x] Column "Prioritas" dapat di-sort (mendesak → penting → biasa)
  - [x] Column "Judul" dapat di-sort alphabetically
  - [x] Default sort: Prioritas descending, lalu Tanggal Mulai descending
  - [x] Icon arrow up/down sesuai sort direction

- [ ] **Pagination**
  - [x] Data pengumuman di-pagination (15 per halaman)
  - [x] Total records ditampilkan
  - [x] Pagination links berfungsi
  - [x] Filter dan search persist saat pindah halaman

### 13. Scheduled Publishing

- [ ] **Auto Publish**
  - [x] Pengumuman dengan status "Draft" auto menjadi "Aktif" saat tanggal mulai tercapai
  - [x] Background job (cron) check dan update status setiap hari jam 00:01
  - [x] Notifikasi otomatis terkirim saat auto publish (jika dicentang)

- [ ] **Auto Expire**
  - [x] Pengumuman dengan status "Aktif" auto menjadi "Kadaluarsa" saat tanggal selesai terlewat
  - [x] Background job check dan update status setiap hari jam 00:01
  - [x] Pengumuman kadaluarsa tidak tampil di dashboard (tapi bisa dilihat di archive)

### 14. Statistics & Analytics

- [ ] **View Tracking**
  - [x] Setiap view detail pengumuman di-track (increment view count)
  - [x] IP address dan user ID di-log untuk analytics
  - [x] Statistik views per hari ditampilkan di admin dashboard

- [ ] **Engagement Metrics**
  - [x] Jumlah views per pengumuman ditampilkan
  - [x] Jumlah komentar per pengumuman ditampilkan
  - [x] Jumlah download file lampiran ditampilkan
  - [x] Chart trend views per minggu/bulan

### 15. Export & Print

- [ ] **Export Excel**
  - [x] Button "Export Excel" di index page
  - [x] Excel berisi list pengumuman dengan semua column
  - [x] File naming: `Pengumuman_Akademik_{TanggalWaktu}.xlsx`

- [ ] **Print Detail**
  - [x] Button "Print" di halaman detail pengumuman
  - [x] PDF detail pengumuman ter-generate
  - [x] PDF berisi judul, konten (formatted), file lampiran list, header institusi

### 16. Performance Testing

- [ ] **Query Optimization**
  - [x] Eager loading relationship: programStudi, createdBy (Waket1 user)
  - [x] Index database untuk column: status, kategori, tanggal_mulai, tanggal_selesai
  - [x] Pagination untuk hindari load semua data
  - [x] Cache pengumuman aktif untuk performa homepage (cache 1 jam)
  - [x] Response time ≤ 2 detik untuk load halaman

### 17. Integration Testing

- [ ] **Integrasi dengan Modul Lain**
  - [x] Pengumuman tampil di dashboard Dosen dengan filter kategori "Akademik"
  - [x] Pengumuman tampil di dashboard Mahasiswa sesuai target
  - [x] Pengumuman "Penting" tampil di banner homepage semua role
  - [x] Notifikasi email terintegrasi dengan mail system
  - [x] File upload terintegrasi dengan storage system

### 18. Edge Cases

- [ ] **Kondisi Khusus**
  - [x] Handle pengumuman dengan tanggal mulai di masa depan (status = Draft sampai tanggal mulai)
  - [x] Handle pengumuman dengan tanggal selesai sudah lewat (status = Kadaluarsa)
  - [x] Handle upload file corrupt atau virus-infected (scan file)
  - [x] Handle konten HTML dengan script injection (sanitize HTML)
  - [x] Handle concurrent edit (dua user edit bersamaan) dengan lock mechanism

### 19. Accessibility & UI

- [ ] **Responsive Design**
  - [x] Table responsive di desktop
  - [x] Card view untuk mobile
  - [x] Form accessible di semua device
  - [x] Rich text editor mobile-friendly

- [ ] **Visual Feedback**
  - [x] Loading indicator saat save/update
  - [x] Success/error message dengan toast notification
  - [x] Badge dan icon sesuai prioritas dan kategori
  - [x] Hover effect di row table

---

## Related Documentation

- [01_DASHBOARD.md](./01_DASHBOARD.md)
- [../DOSEN/01_JADWAL_MENGAJAR.md](../DOSEN/01_JADWAL_MENGAJAR.md)
- [../MAHASISWA/01_KRS.md](../MAHASISWA/01_KRS.md)
- [../ADMIN/01_MASTER_DATA_PROGRAM_STUDI.md](../ADMIN/01_MASTER_DATA_PROGRAM_STUDI.md)
