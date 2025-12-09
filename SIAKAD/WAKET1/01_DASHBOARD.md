# Testing: Dashboard Waket1

**Status**: 🟡 In Progress  
**Role**: Waket1 (Wakil Ketua 1 - Bidang Akademik)  
**Route**: `/waket1/dashboard`  
**Controller**: `App\Http\Controllers\Waket1\DashboardController`  
**Model**: Multiple (ProgramStudi, Mahasiswa, Dosen, NilaiMahasiswa, KalenderPendidikan)

---

## Checklist Testing

### 1. Operations to Test

- [ ] **Index Dashboard**
  - Method: `GET /waket1/dashboard`
  - Action: `DashboardController@index`
  - Menampilkan dashboard Waket1 dengan statistik akademik
  - Response: View dengan data statistik

### 2. Authorization Testing

- [ ] **Access Control**
  - [x] User dengan role `waket1` dapat mengakses dashboard
  - [x] User tanpa role `waket1` mendapat 403 Forbidden
  - [x] Guest user redirect ke halaman login
  - [x] User dengan role `dosen` tidak dapat mengakses
  - [x] User dengan role `mahasiswa` tidak dapat mengakses
  - [x] User dengan role `admin` tidak dapat mengakses (kecuali juga punya role waket1)

### 3. Dashboard Display

- [ ] **Statistik Akademik**
  - [x] Total Program Studi ditampilkan dengan benar
  - [x] Total Mahasiswa Aktif ditampilkan (status_akademik = 'Aktif')
  - [x] Total Dosen Aktif ditampilkan (status_keaktifan = 'Aktif')
  - [x] Total Mata Kuliah aktif ditampilkan
  - [x] Jumlah mahasiswa per program studi ditampilkan
  - [x] Data statistik diambil dari database dengan query efisien

### 4. Monitoring Nilai

- [ ] **Quick Stats Nilai**
  - [x] Rata-rata IPK seluruh mahasiswa aktif ditampilkan
  - [x] Distribusi IPK (4.00, 3.50-3.99, 3.00-3.49, 2.50-2.99, < 2.50) ditampilkan
  - [x] Jumlah mahasiswa dengan IPK Sangat Memuaskan (≥ 3.50) ditampilkan
  - [x] Jumlah mahasiswa dengan IPK Memuaskan (3.00-3.49) ditampilkan
  - [x] Jumlah mahasiswa dengan IPK di bawah 2.50 (berisiko) ditampilkan
  - [x] Persentase distribusi dihitung dengan benar

### 5. Mahasiswa Berisiko

- [ ] **Alert Mahasiswa Berisiko**
  - [x] List mahasiswa dengan IPK < 2.00 ditampilkan
  - [x] List mahasiswa dengan IPS semester aktif < 2.00 ditampilkan
  - [x] Mahasiswa dengan SKS lulus < 50% dari seharusnya ditampilkan
  - [x] Mahasiswa dengan masa studi > 8 semester (untuk S1) ditampilkan
  - [x] Warning count ditampilkan per kategori risiko
  - [x] Link ke detail mahasiswa tersedia

### 6. Revisi Nilai Pending

- [ ] **Pending Approvals**
  - [x] Jumlah revisi nilai yang menunggu approval ditampilkan
  - [x] Status `Pending` atau `Diajukan` counted correctly
  - [x] List 5 revisi nilai terbaru ditampilkan
  - [x] Info dosen pengaju, mata kuliah, dan mahasiswa ditampilkan
  - [x] Link ke halaman revisi nilai untuk approve/reject
  - [x] Badge jumlah pending ditampilkan jika ada

### 7. Kalender Akademik

- [ ] **Periode Akademik Aktif**
  - [x] Tahun akademik aktif ditampilkan (misal: 2024/2025 Ganjil)
  - [x] Periode KRS (tanggal mulai - tanggal selesai) ditampilkan
  - [x] Periode UTS (tanggal mulai - tanggal selesai) ditampilkan
  - [x] Periode UAS (tanggal mulai - tanggal selesai) ditampilkan
  - [x] Status periode (Belum Dimulai/Berlangsung/Selesai) ditampilkan
  - [x] Countdown hari ke periode berikutnya ditampilkan

### 8. Chart & Visualisasi

- [ ] **Grafik Distribusi IPK**
  - [x] Chart distribusi IPK per range (4.00, 3.50-3.99, 3.00-3.49, 2.50-2.99, < 2.50)
  - [x] Data chart akurat sesuai query database
  - [x] Warna bar chart sesuai standar (hijau = bagus, merah = risiko)
  - [x] Total mahasiswa di setiap kategori benar

- [ ] **Grafik Mahasiswa per Program Studi**
  - [x] Bar chart jumlah mahasiswa per program studi
  - [x] Data sesuai dengan query per program
  - [x] Nama program studi ditampilkan dengan benar
  - [x] Sorting dari terbesar ke terkecil

### 9. Recent Activities

- [ ] **Log Aktivitas Akademik**
  - [x] 10 aktivitas terbaru ditampilkan (revisi nilai, approval KRS, perubahan status)
  - [x] Timestamp aktivitas ditampilkan dengan format user-friendly
  - [x] Aktor (dosen/admin) yang melakukan aktivitas ditampilkan
  - [x] Jenis aktivitas (Created/Updated/Approved/Rejected) ditampilkan
  - [x] Link ke detail aktivitas jika ada

### 10. Quick Actions

- [ ] **Tombol Aksi Cepat**
  - [x] Link "Lihat Semua Revisi Nilai" ke halaman revisi nilai
  - [x] Link "Monitoring Nilai & IPK" ke halaman monitoring
  - [x] Link "Kelola Bobot Nilai" ke halaman bobot nilai
  - [x] Link "Buat Pengumuman" ke halaman pengumuman
  - [x] Button disabled jika tidak ada data pending

### 11. Performance Testing

- [ ] **Query Optimization**
  - [x] Total query ≤ 15 untuk load dashboard
  - [x] Semua eager loading relationship sudah benar
  - [x] Index database untuk kolom yang sering diquery (IPK, status_akademik)
  - [x] Cache untuk data statistik yang jarang berubah (total mahasiswa, total dosen)
  - [x] Response time ≤ 2 detik untuk load dashboard

### 12. Responsive & UI

- [ ] **Display di Berbagai Device**
  - [x] Dashboard responsive di desktop (≥1024px)
  - [x] Dashboard responsive di tablet (768px-1023px)
  - [x] Dashboard responsive di mobile (≤767px)
  - [x] Chart tetap terbaca di layar kecil
  - [x] Card statistik stacked dengan baik di mobile

### 13. Data Filtering

- [ ] **Filter Tahun Akademik**
  - [x] Dropdown tahun akademik tersedia
  - [x] Filtering data berdasarkan tahun akademik yang dipilih
  - [x] Default ke tahun akademik aktif
  - [x] Data statistik update sesuai filter
  - [x] Chart update sesuai filter

- [ ] **Filter Program Studi**
  - [x] Dropdown program studi tersedia
  - [x] Filtering data berdasarkan program studi yang dipilih
  - [x] Option "Semua Program Studi" tersedia
  - [x] Data statistik update sesuai filter
  - [x] Chart update sesuai filter

### 14. Export & Print

- [ ] **Export Data**
  - [x] Button "Export PDF" untuk laporan dashboard
  - [x] PDF berisi semua statistik, chart, dan list mahasiswa berisiko
  - [x] Format PDF rapi dan profesional
  - [x] Header PDF berisi logo, nama institusi, tanggal generate
  - [x] Footer PDF berisi halaman dan signature Waket1

### 15. Edge Cases

- [ ] **Kondisi Khusus**
  - [x] Dashboard tetap tampil jika tidak ada data mahasiswa
  - [x] Dashboard tetap tampil jika tidak ada revisi nilai pending
  - [x] Pesan "Tidak ada data" ditampilkan untuk section kosong
  - [x] Chart tidak error jika data kosong (tampilkan placeholder)
  - [x] Handle error jika kalender akademik belum diset

### 16. Integration Testing

- [ ] **Integrasi dengan Modul Lain**
  - [x] Data statistik mahasiswa sync dengan modul master data mahasiswa
  - [x] Data revisi nilai sync dengan modul revisi nilai
  - [x] Data kalender akademik sync dengan modul kalender pendidikan
  - [x] Quick action link berfungsi dengan benar ke modul terkait
  - [x] Role-based menu sidebar menampilkan item yang sesuai untuk waket1

---

## Related Documentation

- [02_MONITORING_NILAI_IPK.md](./02_MONITORING_NILAI_IPK.md)
- [03_REVISI_NILAI.md](./03_REVISI_NILAI.md)
- [04_BOBOT_NILAI.md](./04_BOBOT_NILAI.md)
- [05_PENGUMUMAN_AKADEMIK.md](./05_PENGUMUMAN_AKADEMIK.md)
- [../ADMIN/01_MASTER_DATA_PROGRAM_STUDI.md](../ADMIN/01_MASTER_DATA_PROGRAM_STUDI.md)
- [../ADMIN/04_MASTER_DATA_MAHASISWA.md](../ADMIN/04_MASTER_DATA_MAHASISWA.md)
- [../ADMIN/09_KALENDER_PENDIDIKAN.md](../ADMIN/09_KALENDER_PENDIDIKAN.md)
