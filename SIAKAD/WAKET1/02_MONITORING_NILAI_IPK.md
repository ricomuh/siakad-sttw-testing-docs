# Testing: Monitoring Nilai & IPK

**Status**: 🟡 In Progress  
**Role**: Waket1 (Wakil Ketua 1 - Bidang Akademik)  
**Route**: `/waket1/monitoring`  
**Controller**: `App\Http\Controllers\Waket1\MonitoringController`  
**Model**: Mahasiswa, NilaiMahasiswa, ProgramStudi, KHS

---

## Checklist Testing

### 1. Operations to Test

- [ ] **Index Monitoring**
  - Method: `GET /waket1/monitoring`
  - Action: `MonitoringController@index`
  - Menampilkan monitoring nilai & IPK mahasiswa
  - Response: View dengan data monitoring

### 2. Authorization Testing

- [ ] **Access Control**
  - [x] User dengan role `waket1` dapat mengakses monitoring
  - [x] User tanpa role `waket1` mendapat 403 Forbidden
  - [x] Guest user redirect ke halaman login
  - [x] User dengan role `dosen` tidak dapat mengakses
  - [x] User dengan role `mahasiswa` tidak dapat mengakses

### 3. Filter & Search

- [ ] **Filter Program Studi**

  - [x] Dropdown program studi tersedia
  - [x] Filtering mahasiswa berdasarkan program studi yang dipilih
  - [x] Option "Semua Program Studi" tersedia
  - [x] Query database efficient dengan WHERE clause
  - [x] Count mahasiswa update sesuai filter

- [ ] **Filter Angkatan**

  - [x] Dropdown angkatan tersedia (2020, 2021, 2022, 2023, 2024)
  - [x] Filtering mahasiswa berdasarkan angkatan yang dipilih
  - [x] Option "Semua Angkatan" tersedia
  - [x] Data mahasiswa update sesuai filter
  - [x] Multiple filter dapat dikombinasikan

- [ ] **Filter Status Akademik**

  - [x] Dropdown status akademik tersedia (Aktif/Cuti/Lulus/DO/Mengundurkan Diri)
  - [x] Filtering berdasarkan status akademik yang dipilih
  - [x] Default filter ke "Aktif"
  - [x] Data mahasiswa update sesuai filter

- [ ] **Search Mahasiswa**
  - [x] Input search tersedia (placeholder: "Cari NIM atau Nama")
  - [x] Search by NIM berfungsi
  - [x] Search by nama mahasiswa berfungsi
  - [x] Search case-insensitive
  - [x] Search dapat dikombinasikan dengan filter lainnya

### 4. Table Display

- [ ] **Column Mahasiswa**

  - [x] NIM ditampilkan
  - [x] Nama Mahasiswa ditampilkan
  - [x] Program Studi ditampilkan
  - [x] Angkatan ditampilkan
  - [x] Status Akademik ditampilkan dengan badge warna
  - [x] Semester Saat Ini ditampilkan (misal: Semester 5)

- [ ] **Column Nilai**

  - [x] IPK (Indeks Prestasi Kumulatif) ditampilkan dengan 2 desimal
  - [x] IPS Terakhir (Indeks Prestasi Semester) ditampilkan dengan 2 desimal
  - [x] Total SKS Lulus ditampilkan
  - [x] Total SKS yang seharusnya ditampilkan (berdasarkan semester)
  - [x] Persentase SKS Lulus ditampilkan: `(SKS Lulus / SKS Seharusnya) × 100%`

- [ ] **Predikat IPK**
  - [x] Predikat "Cum Laude" untuk IPK ≥ 3.50
  - [x] Predikat "Sangat Memuaskan" untuk IPK 3.00 - 3.49
  - [x] Predikat "Memuaskan" untuk IPK 2.50 - 2.99
  - [x] Predikat "Cukup" untuk IPK 2.00 - 2.49
  - [x] Predikat "Kurang" untuk IPK < 2.00
  - [x] Badge warna sesuai predikat (hijau = bagus, merah = risiko)

### 5. Sorting

- [ ] **Sort by IPK**

  - [x] Column header "IPK" dapat diklik untuk sorting
  - [x] Sorting ascending (IPK terendah ke tertinggi)
  - [x] Sorting descending (IPK tertinggi ke terendah)
  - [x] Icon arrow up/down ditampilkan sesuai sort direction
  - [x] Default sort descending (IPK tertinggi dulu)

- [ ] **Sort by IPS**

  - [x] Column header "IPS Terakhir" dapat diklik untuk sorting
  - [x] Sorting ascending dan descending berfungsi
  - [x] Icon arrow ditampilkan

- [ ] **Sort by Nama**
  - [x] Column header "Nama" dapat diklik untuk sorting
  - [x] Sorting alphabetically A-Z dan Z-A
  - [x] Case-insensitive sorting

### 6. Pagination

- [ ] **Pagination Control**
  - [x] Data mahasiswa di-pagination (20 per halaman)
  - [x] Total records ditampilkan (misal: "Menampilkan 1-20 dari 150 mahasiswa")
  - [x] Pagination links berfungsi (Previous, 1, 2, 3, Next)
  - [x] Current page highlighted
  - [x] Filter dan search persist saat pindah halaman

### 7. Detail Mahasiswa

- [ ] **View Detail**
  - [x] Button "Detail" pada setiap row mahasiswa
  - [x] Modal atau halaman detail muncul saat diklik
  - [x] Detail IPK per semester ditampilkan (Semester 1-8 dengan IPS masing-masing)
  - [x] Detail SKS per semester ditampilkan
  - [x] Grafik perkembangan IPS per semester ditampilkan
  - [x] List mata kuliah dengan nilai per semester ditampilkan
  - [x] Button "Tutup" untuk close modal

### 8. Mahasiswa Berisiko

- [ ] **Highlight Berisiko**

  - [x] Row mahasiswa dengan IPK < 2.00 di-highlight merah
  - [x] Row mahasiswa dengan IPS terakhir < 2.00 di-highlight orange
  - [x] Row mahasiswa dengan SKS lulus < 50% di-highlight kuning
  - [x] Icon warning ditampilkan untuk mahasiswa berisiko
  - [x] Tooltip muncul saat hover icon warning menjelaskan risiko

- [ ] **Filter Mahasiswa Berisiko**
  - [x] Checkbox "Tampilkan Hanya Mahasiswa Berisiko" tersedia
  - [x] Filter menampilkan mahasiswa dengan IPK < 2.00 OR IPS < 2.00 OR SKS < 50%
  - [x] Count mahasiswa berisiko ditampilkan
  - [x] Filter dapat dikombinasikan dengan filter lainnya

### 9. Export Data

- [ ] **Export Excel**

  - [x] Button "Export Excel" tersedia
  - [x] Excel file ter-download dengan nama: `Monitoring_IPK_{TanggalWaktu}.xlsx`
  - [x] Excel berisi semua column yang ditampilkan di table
  - [x] Excel berisi data sesuai filter yang aktif
  - [x] Excel berisi sheet tambahan dengan grafik distribusi IPK
  - [x] Format excel rapi (header bold, border, freeze panes)

- [ ] **Export PDF**
  - [x] Button "Export PDF" tersedia
  - [x] PDF file ter-download dengan nama: `Monitoring_IPK_{TanggalWaktu}.pdf`
  - [x] PDF berisi table mahasiswa dengan semua column
  - [x] PDF berisi statistik summary (rata-rata IPK, distribusi predikat)
  - [x] PDF berisi header institusi dan logo
  - [x] PDF berisi footer dengan tanggal cetak dan signature Waket1

### 10. Statistik Summary

- [ ] **Summary Cards**

  - [x] Total mahasiswa yang di-monitoring ditampilkan
  - [x] Rata-rata IPK ditampilkan dengan 2 desimal
  - [x] Rata-rata IPS terakhir ditampilkan
  - [x] Jumlah mahasiswa Cum Laude ditampilkan
  - [x] Jumlah mahasiswa berisiko ditampilkan
  - [x] Persentase kelulusan tepat waktu ditampilkan

- [ ] **Grafik Distribusi**
  - [x] Pie chart distribusi predikat IPK ditampilkan
  - [x] Bar chart distribusi IPK per angkatan ditampilkan
  - [x] Line chart trend rata-rata IPK per angkatan ditampilkan
  - [x] Data chart akurat sesuai query database
  - [x] Chart responsive di berbagai ukuran layar

### 11. Notifikasi & Alert

- [ ] **Notifikasi Waket1**
  - [x] Notifikasi muncul jika ada mahasiswa dengan IPK < 1.50 (sangat berisiko)
  - [x] Notifikasi muncul jika ada mahasiswa dengan masa studi > 10 semester
  - [x] Count notifikasi ditampilkan di badge
  - [x] Notifikasi dapat di-dismiss setelah dibaca
  - [x] Log notifikasi disimpan di database

### 12. Performance Testing

- [ ] **Query Optimization**
  - [x] Query dengan pagination untuk hindari load semua data
  - [x] Eager loading untuk relationship (program_studi, nilai_mahasiswa)
  - [x] Index database untuk column IPK, IPS, status_akademik
  - [x] Cache untuk data statistik yang jarang berubah
  - [x] Response time ≤ 3 detik untuk load halaman dengan 1000+ mahasiswa

### 13. Responsive & UI

- [ ] **Display di Berbagai Device**
  - [x] Table responsive di desktop (≥1024px)
  - [x] Table scrollable horizontal di tablet (768px-1023px)
  - [x] Table stack vertical di mobile (≤767px)
  - [x] Filter dropdown accessible di mobile
  - [x] Chart tetap terbaca di layar kecil

### 14. Integration Testing

- [ ] **Integrasi dengan Modul Lain**
  - [x] Data mahasiswa sync dengan modul master data mahasiswa
  - [x] Data nilai sync dengan modul KHS dan nilai mahasiswa
  - [x] Data program studi sync dengan modul master data program studi
  - [x] Filter angkatan menggunakan data aktual dari mahasiswa
  - [x] Perubahan status akademik mahasiswa langsung tercermin di monitoring

### 15. Edge Cases

- [ ] **Kondisi Khusus**
  - [x] Halaman tetap tampil jika tidak ada data mahasiswa
  - [x] Pesan "Tidak ada data" ditampilkan untuk filter yang tidak menghasilkan data
  - [x] Handle mahasiswa dengan nilai belum lengkap (IPK/IPS null)
  - [x] Handle mahasiswa transfer dengan SKS transfer (dihitung dengan benar)
  - [x] Handle mahasiswa yang cuti (tidak dihitung di rata-rata IPK aktif)

### 16. Accessibility

- [ ] **Aksesibilitas**
  - [x] Table memiliki proper headers untuk screen reader
  - [x] Filter dropdown dapat diakses dengan keyboard (Tab navigation)
  - [x] Button focus state terlihat jelas
  - [x] Color contrast memenuhi WCAG AA standard
  - [x] Alt text untuk grafik dan chart

---

## Related Documentation

- [01_DASHBOARD.md](./01_DASHBOARD.md)
- [03_REVISI_NILAI.md](./03_REVISI_NILAI.md)
- [../ADMIN/04_MASTER_DATA_MAHASISWA.md](../ADMIN/04_MASTER_DATA_MAHASISWA.md)
- [../MAHASISWA/02_KHS.md](../MAHASISWA/02_KHS.md)
- [../DOSEN/03_MATA_KULIAH_DETAIL.md](../DOSEN/03_MATA_KULIAH_DETAIL.md)
