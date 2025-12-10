# Testing Documentation: Kurikulum (Curriculum) Module

**Status**: ✅ Complete  
**Route Prefix**: `/siakad/kurikulum`  
**Models**: Kurikulum, ProgramStudi, MataKuliah  
**Results**: 33/33 tests passing (93 assertions, 1.74s)

## Test Coverage Summary

### ✅ Authorization (2 tests)

- Authenticated admin access
- Guest redirection to login

### ✅ Index/List (6 tests)

- Display with programStudi relationship
- Empty state handling
- Pagination (10 per page)
- Filter by program_studi_id
- Filter by is_active (boolean)

### ✅ Create (8 tests)

- Form access
- Valid creation
- Required field validation
- **Year validation** (min: 2000, max: current_year + 5)
- **SKS non-negative** (total_sks, sks_wajib, sks_pilihan)
- program_studi_id exists validation

### ✅ Show/Detail (3 tests)

- View detail page
- Display with programStudi and mataKuliah relationships
- 404 for non-existent

### ✅ Edit (2 tests)

- Form access
- Display existing data

### ✅ Update (2 tests)

- Valid update
- Required field validation

### ✅ Delete (3 tests)

- Successful deletion
- **Foreign key constraint** (cannot delete if has mata kuliah)
- 404 for non-existent

### ✅ Model (5 tests)

- Fillable fields (7 total)
- is_active boolean cast
- **tahun_kurikulum integer cast**
- programStudi BelongsTo relationship
- mataKuliah HasMany relationship

### ✅ Factory (2 tests)

- Valid data creation
- Multiple instances

### ✅ Performance (2 tests)

- 100 records < 2s
- N+1 prevention (eager loading)

## Key Features

**Dynamic Year Validation**:

- Minimum: 2000
- Maximum: Current year + 5 (allows future planning)

**SKS Management**:

- Three fields: total_sks, sks_wajib, sks_pilihan
- All must be non-negative integers

**Foreign Key Protection**:

- Cannot delete kurikulum that has mata kuliah
- Changed migration from `cascade` to `restrict`
- Controller has try-catch for constraint violations

**Integer Casting**:

- tahun_kurikulum automatically cast to integer
- Ensures type consistency in business logic

**No Soft Deletes**:

- Hard delete only (like Mata Kuliah)
- Permanent deletion after confirmation

## Issues Found & Fixed

### 1. Migration Foreign Key Cascade (FIXED)

**Problem**: `mata_kuliah.kurikulum_id` had `onDelete('cascade')` - automatically deleted mata kuliah when kurikulum deleted  
**Impact**: Controller's try-catch for foreign key constraint never triggered  
**Fix**: Changed to `onDelete('restrict')` in migration `2025_11_16_115952_create_mata_kuliah_table.php`  
**Result**: Now correctly prevents deletion when mata kuliah exists

### 2. Role Seeding Issue (FIXED)

**Problem**: Tests failed with `RoleDoesNotExist: There is no role named 'admin'`  
**Impact**: 0/33 tests could execute (all failed in beforeEach)  
**Fix**: Added `Role::create(['name' => 'admin'])` before `assignRole('admin')` in beforeEach  
**Result**: All tests now execute properly

## Statistics

| Metric                 | Value                                              |
| ---------------------- | -------------------------------------------------- |
| Tests                  | 33                                                 |
| Assertions             | 93                                                 |
| Duration               | 1.74s                                              |
| Pass Rate              | 100%                                               |
| Controller Methods     | 7 (CRUD)                                           |
| Eager Loaded Relations | 1 (programStudi)                                   |
| Filters                | 2 (program_studi_id, is_active)                    |
| Validation Rules       | 7 fields                                           |
| Casts                  | 2 (is_active → boolean, tahun_kurikulum → integer) |
| Foreign Keys           | 1 (program_studi_id)                               |
| Has Children           | Yes (mataKuliah)                                   |

## Relationships

```
ProgramStudi (Parent)
    ↓ HasMany
Kurikulum
    ↓ HasMany
MataKuliah (Children)
```

**Delete Behavior**:

- ProgramStudi deletion: Kurikulum.program_studi_id set to NULL (if nullable)
- Kurikulum deletion: **RESTRICTED** if has MataKuliah records

## Next Steps

Module completed successfully. Ready for:

- ✅ Integration with Program Studi (parent relationship verified)
- ✅ Integration with Mata Kuliah (child relationship verified, constraint working)
- ⚪ Additional validation (e.g., total_sks = sks_wajib + sks_pilihan logic)
- ⚪ Academic year management features
