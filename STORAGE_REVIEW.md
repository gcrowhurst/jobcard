# Code Review: Database & Storage Implementation

## Current Implementation Analysis

### 1. **IndexedDB Setup (`initDB`)**
- ✅ **Strengths:**
  - Properly uses versioned schema (version 2)
  - Creates two object stores: `jobCards` (main data) and `photos` (binary data)
  - Uses `keyPath: 'id'` for jobCards and `autoIncrement: true` for photos
  - Handles schema upgrades gracefully

- ⚠️ **Observations:**
  - No error handling for blocked upgrades
  - No validation of existing data during migration

### 2. **Data Structures & Indexing**

#### Current ID Management
```javascript
let currentJobId = 0;           // Tracks next job ID
let currentTaskId = 0;          // Tracks next task ID
let currentToolId = 0;          // Tracks next tool ID
```

**Issues Found:**
- `currentTaskId` is **never incremented** - all tasks in a job will have ID 0 on new job creation
- `currentToolId` properly increments in `addTool()` but resets on template load
- IDs use both `Date.now()` and numeric increment - inconsistent approach

**Recommendations:**
- Use `Date.now() + index` for all entities to ensure uniqueness
- Or implement a dedicated ID generator function

### 3. **Photo Storage**
- ✅ **Good:** Separate object store for binary data
- ✅ **Good:** Image compression before storage (1200px max width, 0.8 quality)
- ⚠️ **Issue:** No cleanup - deleting photos doesn't remove from IndexedDB
- ⚠️ **Issue:** `photoId` reference in task may become orphaned

### 4. **Save/Load Operations**

#### `saveJobCard()`
```javascript
const transaction = db.transaction([STORE_NAME], 'readwrite');
const store = transaction.objectStore(STORE_NAME);
await store.put(data);  // ⚠️ This line won't work - store.put() doesn't return Promise
```

**Issue:** Uses `await` on transaction, but should properly handle `request.onsuccess/onerror`

#### `loadJobCard()`
- Uses key `'current'` for auto-save
- Loads only from IndexedDB, no fallback mechanism

#### `loadSavedCards()` & `loadCard()`
- ✅ Properly filters cards with `filter(card => card.id !== 'current' && card.name)`
- ✅ Uses `getAll()` and correctly handles multiple cards

### 5. **Data Validation**
- ❌ **Missing:** No validation when loading data
- ❌ **Missing:** No schema version checks when loading
- ❌ **Missing:** No sanitization against XSS (uses `innerHTML` in many places)

### 6. **Storage Issues Found**

1. **Mixed ID Strategies:**
   - Tools: numeric IDs (1, 2, 3...)
   - Customer Requests: `Date.now() + index`
   - Jobs: numeric IDs (1, 2, 3...)
   - Tasks: numeric IDs but never incremented!

2. **Async/Promise Issues:**
   - `store.put()` doesn't return a Promise - shouldn't use `await`
   - Should use callbacks instead

3. **Missing Error Handling:**
   - IndexedDB quota exceeded not handled
   - Browser privacy mode (IndexedDB not available) not handled

4. **Data Export/Import:**
   - Only exports to PDF, no JSON export
   - No import from JSON files
   - Templates are hardcoded, not loaded from files

## Recommendations

### Priority 1 (Critical)
1. Fix `currentTaskId` increment bug in `addTaskToJob()`
2. Remove `await` from IndexedDB operations - use callbacks
3. Add JSON import/export functionality
4. Implement fallback if IndexedDB unavailable (use localStorage)

### Priority 2 (Important)
1. Standardize ID generation across all entities
2. Add input validation before loading data
3. Implement proper error handling for quota exceeded
4. Add data migration helper for schema changes

### Priority 3 (Nice to Have)
1. Add export templates as JSON files
2. Implement search/filter in saved cards
3. Add data backup feature
4. Add offline mode indicator

