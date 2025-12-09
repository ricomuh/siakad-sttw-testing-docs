# Testing: Neo Feeder Sync (PDDIKTI)

**Status**: ⚪ [TODO]  
**Role**: Admin  
**Route**: `/siakad/neo-feeder-sync`  
**Controller**: `NeoFeederSyncController`  
**API**: https://pddikti.kemdikbud.go.id/api/

---

## 📋 Checklist Testing

### 1. Operations

-   [ ] **Dashboard**: Status sync, last sync time, failed records
-   [ ] **Sync Mahasiswa**: Push data mahasiswa to PDDIKTI
-   [ ] **Sync Dosen**: Push data dosen
-   [ ] **Sync Nilai**: Push nilai mahasiswa per semester
-   [ ] **Sync Kurikulum**: Push struktur kurikulum

### 2. Authentication

-   [ ] Get token from PDDIKTI API
-   [ ] Store token in cache (expire 1 hour)
-   [ ] Refresh token automatically
-   [ ] Handle invalid credentials

### 3. Data Mapping

-   [ ] Map local fields to PDDIKTI fields
-   [ ] Handle required vs optional fields
-   [ ] Data transformation (date format, enum values)
-   [ ] Validate before sending

### 4. Sync Process

-   [ ] **Incremental Sync**: Only changed data since last sync
-   [ ] **Full Sync**: All data (use with caution)
-   [ ] **Chunk Processing**: Send 100 records per batch
-   [ ] **Queue Job**: Long sync runs in background

### 5. Error Handling

-   [ ] API timeout (retry 3x)
-   [ ] Validation errors from PDDIKTI
-   [ ] Network errors
-   [ ] Log all failed records with reason

### 6. Retry Mechanism

-   [ ] Auto-retry failed records
-   [ ] Manual retry from dashboard
-   [ ] Skip permanently failed (after 5 attempts)
-   [ ] Show detailed error message

### 7. Reports

-   [ ] Sync history log
-   [ ] Success/fail statistics
-   [ ] List of failed records
-   [ ] Export error log to Excel

---

## 🔗 Related

-   [Mahasiswa](06_MASTER_DATA_MAHASISWA.md)
-   [Dosen](04_MASTER_DATA_DOSEN.md)
-   [Kurikulum](07_STRUKTUR_KURIKULUM.md)

## 📚 Resources

-   PDDIKTI API Docs: https://pddikti.kemdikbud.go.id/api/docs
-   Neo Feeder Manual: https://pddikti.kemdikbud.go.id/feeder/manual
