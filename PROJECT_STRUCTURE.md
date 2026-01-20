# Project Structure Guide

## Directory Layout

```
jobcard/
├── index.html                          # Main application
├── json-templates/                     # Demo job card templates
│   ├── README.md                       # Template directory documentation
│   ├── demo-cooling-systems.json       # Cooling Systems Assessment template
│   └── demo-brake-systems.json         # Brake Systems Service template
├── STORAGE_REVIEW.md                   # Database & storage analysis
├── JSON_IMPORT_GUIDE.md                # JSON import/export documentation
└── PROJECT_STRUCTURE.md                # This file
```

## How It Works

### Application Flow

```
User Opens Application
    ↓
Choose Action:
    ├─→ Load from Template (hardcoded in JS)
    ├─→ Quick Load Demo (from json-templates/ folder) ✨ NEW
    ├─→ Upload Custom JSON (from computer)
    └─→ Start Fresh
    ↓
Fill Out Form
    ├─ Student info
    ├─ Vehicle details
    ├─ Customer requests
    ├─ Jobs & tasks
    ├─ Add photos
    └─ Technical data
    ↓
Save/Export:
    ├─→ Save to IndexedDB (local browser storage)
    ├─→ Save as Named Card (indexed in IndexedDB)
    └─→ Export to JSON (download .json file)
```

## JSON Import/Export System

### Quick Load Demo (NEW FEATURE)
Located in "Load Template or Import Data" section:

1. **Dropdown: "📁 Quick Load Demo Template"**
   - Automatically discovers JSON files in `json-templates/` folder
   - Pre-configured with built-in demo templates
   - Loads complete job card with one click
   - No file selection dialog needed
   - Fast loading for training scenarios

### Custom File Upload
Also in same section:

2. **File Input: "Or Upload Custom JSON"**
   - Browse computer for any `.json` file
   - Load previously exported job cards
   - Share templates between users
   - Flexible for user-created templates

### Export to JSON
Button in same section:

3. **Export: "📥 Export to JSON"**
   - Saves current form data as `.json` file
   - Downloads to computer
   - Filename format: `jobcard-[StudentName]-[Date].json`
   - Portable between devices and browsers

## JSON Templates Folder Structure

### Location: `/json-templates/`

**Purpose:** Store reusable demo job cards and templates

**Files:**
- `README.md` - Folder documentation
- `demo-cooling-systems.json` - Example: Engine Cooling Systems assessment (7290-702)
- `demo-brake-systems.json` - Example: Brake Systems Service assessment (7290-701)

**Adding New Templates:**
1. Create new JSON file following the structure in demo files
2. Place in `json-templates/` folder
3. Update `index.html` dropdown to include new template:
   ```html
   <option value="json-templates/demo-new-system.json">New System Template</option>
   ```

## Storage Architecture

### Three-Layer Storage System

```
┌─────────────────────────────────────────────────────┐
│ Browser Storage Options                              │
├─────────────────────────────────────────────────────┤
│                                                       │
│ 1. LOCAL FILE SYSTEM (json-templates/)              │
│    ├─ Static demo templates                         │
│    ├─ Loaded via fetch() API                        │
│    ├─ User computer (upload)                        │
│    └─ Exported .json files                          │
│                                                       │
│ 2. INDEXEDDB (Local Browser Storage)                │
│    ├─ jobCards store - user data                    │
│    ├─ photos store - images                         │
│    ├─ Persistent across sessions                    │
│    ├─ Quota: ~50MB per domain                       │
│    └─ Survives browser restarts                     │
│                                                       │
│ 3. JSON STRING (In Memory)                          │
│    ├─ Current form state                            │
│    ├─ Collected via collectFormData()               │
│    ├─ Exported/Imported as JSON                     │
│    └─ Transferred between systems                   │
│                                                       │
└─────────────────────────────────────────────────────┘
```

## Data Flow Diagram

### Import Path
```
json-templates/demo-*.json
        ↓
fetch(filePath)
        ↓
JSON.parse()
        ↓
populateForm(data)
        ↓
renderRequests/Jobs/Tools
        ↓
✓ Form populated
```

### Export Path
```
Form Fields
        ↓
collectFormData()
        ↓
JSON.stringify()
        ↓
Blob creation
        ↓
Download as .json
        ↓
✓ File saved to computer
```

### Save Path
```
collectFormData()
        ↓
IndexedDB transaction
        ↓
store.put(data)
        ↓
✓ Data persisted
```

## File Organization Strategy

### Why Separate Folders?

**`json-templates/` folder:**
- ✅ Organizes demo data separately from source code
- ✅ Easy to find and manage templates
- ✅ Clear separation of concerns
- ✅ Scalable for many templates
- ✅ Simple to share/backup templates

**Root level:**
- ✅ Core application files only
- ✅ Documentation accessible
- ✅ Clean project root

**Best Practice:**
```
Production use:
  json-templates/
    ├─ demo-cooling-systems.json
    ├─ demo-brake-systems.json
    ├─ demo-engine-systems.json
    ├─ demo-electrical-systems.json
    ├─ demo-transmission-systems.json
    └─ demo-suspension-systems.json

Custom templates:
  uploads/  (user-created)
    ├─ training-module-1.json
    └─ customer-demo.json
```

## Feature Comparison

| Feature | Method | Speed | Use Case |
|---------|--------|-------|----------|
| **Quick Load Demo** | `json-templates/` | ⚡ Very Fast | Training, quick start |
| **Custom Upload** | User file | ⚡ Very Fast | Load own exports, shared files |
| **Template Select** | Hardcoded JS | ⚡ Very Fast | Basic customer requests |
| **Save to IndexedDB** | Local DB | ⚡ Very Fast | Auto-save during work |
| **Export to JSON** | Download | ⚡ Very Fast | Backup, share, archive |

## Development Workflow

### For Adding New Demo Templates

1. **Create JSON file** in `json-templates/`
   - Name: `demo-[system-name].json`
   - Follow structure in existing demos

2. **Test locally**
   - `python3 -m http.server 8000`
   - Load via Quick Load Demo dropdown

3. **Update HTML**
   - Add option to demo select dropdown
   - Format: `<option value="json-templates/demo-name.json">Description</option>`

4. **Document**
   - Update `json-templates/README.md`
   - Add description to dropdown

### For Customizing Import/Export

**JavaScript Functions:**
```javascript
loadDemoTemplate()    // Load from json-templates/
importFromJSON()      // Load from file upload
exportToJSON()        // Save to computer
loadTemplates()       // Load hardcoded templates (JS)
```

**Form Data:**
```javascript
collectFormData()     // Get all form fields as object
populateForm(data)    // Populate form from object
```

## Technical Benefits

### Organized Structure Provides:
- **Scalability** - Easy to add more templates
- **Maintainability** - Templates clearly separated from code
- **User Experience** - Quick access to demos
- **Flexibility** - Users can upload own templates
- **Portability** - Templates easily shared via JSON files
- **Version Control** - Track template changes in git

## Future Enhancement Opportunities

### Phase 1 (Current)
✅ Quick load from json-templates/
✅ Upload custom JSON files
✅ Export to JSON downloads

### Phase 2 (Recommended)
- [ ] Create template builder UI in app
- [ ] Save custom templates to json-templates/
- [ ] Template versioning system
- [ ] Template sharing/marketplace

### Phase 3 (Advanced)
- [ ] Cloud storage integration
- [ ] Template validation schema
- [ ] Bulk template operations
- [ ] Template search/categorization

