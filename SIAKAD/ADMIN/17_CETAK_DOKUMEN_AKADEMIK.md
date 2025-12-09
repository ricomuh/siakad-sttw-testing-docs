# Testing: Cetak Dokumen Akademik

**Status**: ⚪ [TODO]  
**Role**: Admin  
**Route**: `/siakad/cetak-dokumen`  
**Controller**: `CetakDokumenController`  
**Library**: DomPDF, Laravel Snappy (optional)

---

## 📋 Checklist Testing

### 1. Dokumen Types

-   [ ] **Transkrip Nilai**: PDF transkrip mahasiswa
-   [ ] **KHS (Kartu Hasil Studi)**: Per semester
-   [ ] **Kartu Ujian**: Foto, QR code, identitas
-   [ ] **Surat Keterangan**: Aktif kuliah, lulus, cuti
-   [ ] **Berita Acara**: Yudisium, wisuda

### 2. Template Management

-   [ ] Load template from `resources/views/pdf/`
-   [ ] Dynamic placeholders: {{nama}}, {{nim}}, {{prodi}}
-   [ ] Header/footer: Logo, nomor dokumen, tanggal cetak
-   [ ] Watermark untuk draft dokumen

### 3. PDF Generation

-   [ ] Generate single PDF (1 mahasiswa)
-   [ ] Generate batch PDF (multiple mahasiswa in 1 file)
-   [ ] Generate zip of individual PDFs
-   [ ] Page size: A4, Letter, F4
-   [ ] Orientation: Portrait, Landscape

### 4. Data Validation

-   [ ] Check mahasiswa exists
-   [ ] Check nilai completed (for transkrip)
-   [ ] Check foto available (for kartu ujian)
-   [ ] Check eligibility (for surat lulus)

### 5. Numbering System

-   [ ] Auto-generate nomor dokumen: 001/SK/STTW/I/2025
-   [ ] Increment per jenis dokumen
-   [ ] Store nomor to database (prevent duplicate)

### 6. Security

-   [ ] QR Code validation (signed URL)
-   [ ] Digital signature (optional)
-   [ ] Prevent PDF editing (protected mode)
-   [ ] Watermark for unofficial copies

### 7. Performance

-   [ ] Queue large batch printing jobs
-   [ ] Progress indicator for batch processing
-   [ ] Timeout handling (max 5 minutes)
-   [ ] Cache frequently used templates

---

## 🔗 Related

-   [Mahasiswa](06_MASTER_DATA_MAHASISWA.md)
-   [Manajemen Yudisium](16_MANAJEMEN_YUDISIUM.md)
