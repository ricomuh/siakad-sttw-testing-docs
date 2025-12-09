# Testing: Dashboard Ketua

**Status**: 🟡 In Progress  
**Role**: Ketua (Ketua Sekolah Tinggi)  
**Route**: `/ketua/dashboard`  
**Controller**: `App\Http\Controllers\Ketua\DashboardController`  
**Model**: Multiple (ProgramStudi, Mahasiswa, Dosen, ApprovalYudisium, KalenderPendidikan)

---

## Checklist Testing

### 1. Operations to Test

- [ ] **Index Dashboard**
  - Method: `GET /ketua/dashboard`
  - Action: `DashboardController@index`
  - Menampilkan dashboard Ketua dengan statistik institusi
  - Response: View dengan data statistik

### 2. Authorization Testing

- [ ] **Access Control**
  - [x] User dengan role `ketua` dapat mengakses dashboard
  - [x] User tanpa role `ketua` mendapat 403 Forbidden
  - [x] Guest user redirect ke halaman login
  - [x] User dengan role `waket1` tidak dapat mengakses (kecuali juga punya role ketua)
  - [x] User dengan role `dosen` tidak dapat mengakses
  - [x] User dengan role `mahasiswa` tidak dapat mengakses

### 3. Dashboard Display

- [ ] **Statistik Institusi**
  - [x] Total Program Studi ditampilkan dengan benar
  - [x] Total Mahasiswa Aktif ditampilkan (semua program studi)
  - [x] Total Dosen Aktif ditampilkan (status_keaktifan = 'Aktif')
  - [x] Total Mahasiswa Lulus tahun ini ditampilkan
  - [x] Jumlah Yudisium pending approval ditampilkan
  - [x] Data statistik diambil dari database dengan query efisien

### 4. Approval Yudisium Pending

- [ ] **Quick Stats Approval**
  - [x] Jumlah approval yudisium pending ditampilkan dengan badge merah
  - [x] List 10 approval yudisium terbaru ditampilkan
  - [x] Info mahasiswa (NIM, Nama, Program Studi) ditampilkan
  - [x] IPK mahasiswa ditampilkan
  - [x] Total SKS lulus ditampilkan
  - [x] Predikat kelulusan ditampilkan (Cum Laude/Sangat Memuaskan/Memuaskan)
  - [x] Link ke halaman approval untuk approve/reject
  - [x] Status "Pending" dengan badge kuning

### 5. Mahasiswa Lulus

- [ ] **Statistik Kelulusan**
  - [x] Total mahasiswa lulus tahun ini ditampilkan
  - [x] Rata-rata IPK mahasiswa lulus ditampilkan
  - [x] Distribusi predikat kelulusan ditampilkan:
    - Cum Laude (IPK ≥ 3.50)
    - Sangat Memuaskan (IPK 3.00 - 3.49)
    - Memuaskan (IPK 2.50 - 2.99)
  - [x] Persentase kelulusan tepat waktu ditampilkan (≤ 4 tahun untuk S1)
  - [x] Chart distribusi predikat kelulusan

### 6. Mahasiswa per Program Studi

- [ ] **Breakdown per Program**
  - [x] Tabel mahasiswa per program studi ditampilkan
  - [x] Column: Nama Program, Total Mahasiswa, Mahasiswa Aktif, Mahasiswa Cuti, Mahasiswa Lulus
  - [x] Bar chart jumlah mahasiswa per program studi
  - [x] Data akurat sesuai query database
  - [x] Sorting dari program dengan mahasiswa terbanyak

### 7. Dosen per Program Studi

- [ ] **Breakdown Dosen**
  - [x] Tabel dosen per program studi ditampilkan
  - [x] Column: Nama Program, Total Dosen, Dosen Aktif, Dosen PNS, Dosen Non-PNS
  - [x] Rasio Dosen:Mahasiswa ditampilkan per program
  - [x] Warning jika rasio > 1:30 (overload)
  - [x] Chart distribusi dosen per program studi

### 8. Kalender Akademik

- [ ] **Periode Akademik Aktif**
  - [x] Tahun akademik aktif ditampilkan (misal: 2024/2025 Ganjil)
  - [x] Periode KRS ditampilkan dengan status (Berlangsung/Selesai)
  - [x] Periode UTS ditampilkan dengan status
  - [x] Periode UAS ditampilkan dengan status
  - [x] Periode Yudisium ditampilkan dengan status
  - [x] Countdown hari ke periode penting berikutnya
  - [x] Link ke halaman kalender akademik lengkap

### 9. Aktivitas Terkini

- [ ] **Recent Activities**
  - [x] 10 aktivitas institusi terbaru ditampilkan
  - [x] Jenis aktivitas: Approval Yudisium, Wisuda, Perubahan Status Mahasiswa
  - [x] Timestamp aktivitas dengan format user-friendly
  - [x] Aktor yang melakukan aktivitas ditampilkan
  - [x] Link ke detail aktivitas jika ada

### 10. Quick Actions

- [ ] **Tombol Aksi Cepat**
  - [x] Button "Lihat Semua Approval Yudisium" dengan badge count pending
  - [x] Button "Lihat Laporan Institusi" ke halaman report
  - [x] Button "Kelola Kalender Akademik" (jika Ketua punya akses)
  - [x] Button disabled jika tidak ada data pending

### 11. Chart & Visualisasi

- [ ] **Grafik Mahasiswa Aktif per Semester**

  - [x] Line chart trend mahasiswa aktif 3 tahun terakhir
  - [x] Breakdown per semester (Ganjil/Genap)
  - [x] Data akurat dari database
  - [x] Tooltip menampilkan detail saat hover

- [ ] **Grafik Kelulusan per Tahun**

  - [x] Bar chart jumlah mahasiswa lulus per tahun (5 tahun terakhir)
  - [x] Stacked bar dengan breakdown predikat kelulusan
  - [x] Data akurat dan up-to-date

- [ ] **Grafik Distribusi IPK**
  - [x] Pie chart distribusi IPK seluruh mahasiswa aktif
  - [x] Kategori: ≥3.50, 3.00-3.49, 2.50-2.99, 2.00-2.49, <2.00
  - [x] Warna sesuai standar (hijau = bagus, merah = risiko)

### 12. Performance Indicators

- [ ] **KPI Institusi**
  - [x] Card "Akreditasi Program Studi" dengan status per program (A/B/C)
  - [x] Card "Tingkat Kelulusan Tepat Waktu" dengan persentase
  - [x] Card "Rata-rata IPK Lulusan" dengan trend (naik/turun)
  - [x] Card "Rasio Dosen:Mahasiswa" dengan status (ideal/overload)
  - [x] Icon status (✓ hijau = bagus, ⚠ kuning = warning, ✗ merah = buruk)

### 13. Notifikasi & Alert

- [ ] **Alert System**
  - [x] Badge notifikasi approval pending di menu sidebar
  - [x] Alert muncul jika ada approval yudisium > 3 hari belum diproses
  - [x] Alert muncul jika periode yudisium akan berakhir dalam 7 hari
  - [x] Sound notification (optional) untuk approval baru
  - [x] Browser push notification jika user mengizinkan

### 14. Export & Print

- [ ] **Export Dashboard**
  - [x] Button "Export PDF" untuk laporan dashboard
  - [x] PDF berisi semua statistik, chart, dan KPI
  - [x] Format PDF profesional dengan header institusi
  - [x] Footer PDF berisi tanggal generate dan signature Ketua
  - [x] File naming: `Dashboard_Ketua_{TanggalWaktu}.pdf`

### 15. Data Filtering

- [ ] **Filter Tahun Akademik**

  - [x] Dropdown tahun akademik tersedia
  - [x] Filtering data berdasarkan tahun akademik yang dipilih
  - [x] Default ke tahun akademik aktif
  - [x] Semua statistik dan chart update sesuai filter

- [ ] **Filter Program Studi**
  - [x] Dropdown program studi tersedia
  - [x] Filtering data untuk melihat breakdown per program studi
  - [x] Option "Semua Program Studi" tersedia
  - [x] Statistik mahasiswa dan dosen update sesuai filter

### 16. Performance Testing

- [ ] **Query Optimization**
  - [x] Total query ≤ 20 untuk load dashboard lengkap
  - [x] Eager loading semua relationship
  - [x] Index database untuk column sering diquery (status_akademik, tahun_lulus)
  - [x] Cache untuk data statistik institusi (update setiap 1 jam)
  - [x] Response time ≤ 3 detik untuk load dashboard dengan data besar

### 17. Responsive & UI

- [ ] **Display di Berbagai Device**
  - [x] Dashboard responsive di desktop (≥1024px)
  - [x] Dashboard responsive di tablet (768px-1023px)
  - [x] Dashboard responsive di mobile (≤767px)
  - [x] Chart tetap terbaca di layar kecil
  - [x] Card statistik stacked dengan baik di mobile
  - [x] Table scrollable horizontal di mobile

### 18. Integration Testing

- [ ] **Integrasi dengan Modul Lain**
  - [x] Data mahasiswa sync dengan modul master data mahasiswa
  - [x] Data approval yudisium sync dengan modul approval
  - [x] Data kalender akademik sync dengan modul kalender pendidikan
  - [x] Quick action link berfungsi dengan benar
  - [x] Role-based menu sidebar menampilkan item yang sesuai untuk ketua

### 19. Edge Cases

- [ ] **Kondisi Khusus**
  - [x] Dashboard tetap tampil jika tidak ada approval pending
  - [x] Dashboard tetap tampil jika tidak ada mahasiswa lulus tahun ini
  - [x] Pesan "Tidak ada data" untuk section kosong
  - [x] Chart tidak error jika data kosong (tampilkan placeholder)
  - [x] Handle error jika kalender akademik belum diset untuk tahun ini

### 20. Accessibility

- [ ] **Aksesibilitas**
  - [x] Dashboard dapat diakses dengan keyboard (Tab navigation)
  - [x] Screen reader friendly (proper ARIA labels)
  - [x] Color contrast memenuhi WCAG AA standard
  - [x] Focus state visible untuk semua interactive elements
  - [x] Alt text untuk chart dan grafik

---

## Related Documentation

- [02_APPROVAL_YUDISIUM.md](./02_APPROVAL_YUDISIUM.md)
- [../WAKET1/01_DASHBOARD.md](../WAKET1/01_DASHBOARD.md)
- [../WAKET1/02_MONITORING_NILAI_IPK.md](../WAKET1/02_MONITORING_NILAI_IPK.md)
- [../ADMIN/04_MASTER_DATA_MAHASISWA.md](../ADMIN/04_MASTER_DATA_MAHASISWA.md)
- [../ADMIN/09_KALENDER_PENDIDIKAN.md](../ADMIN/09_KALENDER_PENDIDIKAN.md)
