# Quick Start Guide

## 🚀 Getting Started with Job Card Templates

### What's New?
The application now supports quick-loading demo templates from the `json-templates/` folder!

## 📁 Project Organization

```
jobcard/
├── json-templates/              ← Demo templates stored here
│   ├── demo-cooling-systems.json
│   ├── demo-brake-systems.json
│   └── README.md
├── index.html                   ← Main application
└── [Documentation files]
```

## 🎯 Three Ways to Load Data

### Method 1: Quick Load Demo (FASTEST ⚡)
**Best for:** Training, quick start, demonstrations

1. Open the application
2. In "Load Template or Import Data" section
3. Click dropdown: **"📁 Quick Load Demo Template"**
4. Select your assessment type
5. Form populates instantly ✓

**Available Demos:**
- ✅ Cooling Systems Assessment
- ✅ Brake Systems Service

### Method 2: Upload Your Own JSON File
**Best for:** Loading previously exported job cards, sharing templates

1. In "Load Template or Import Data" section
2. Click: **"Or Upload Custom JSON"**
3. Browse to your `.json` file
4. Form populates with your data ✓

### Method 3: Use Built-in Templates
**Best for:** Quick customer request loading

1. Scroll to "Load Template" section
2. Select from dropdown (Engine Systems, Brake Systems, etc.)
3. Customer requests and tools load ✓

## 💾 Saving Your Work

### Option A: Save to Browser Storage
```
Click "Save Job Card"
→ Saves to IndexedDB (local storage)
→ Persists across browser sessions
```

### Option B: Save as Named Card
```
Enter name + Click "Save as New Card"
→ Multiple saved cards in list
→ Click "Load" to restore
```

### Option C: Export to JSON (RECOMMENDED)
```
Click "📥 Export to JSON"
→ Downloads: jobcard-StudentName-Date.json
→ Safe backup on your computer
→ Can be shared via email/cloud
→ Can be re-imported anytime
```

## 📊 Demo Contents

### Cooling Systems (demo-cooling-systems.json)
- **Student:** John Smith
- **Vehicle:** Ford Focus 2019 (CV26 ABC)
- **Jobs:** 4 (overheating, thermostat, leak diagnosis, electrical check)
- **Tasks:** 45+ detailed assessment tasks
- **Technical Data:** Specifications and measurements

### Brake Systems (demo-brake-systems.json)
- **Student:** Sarah Johnson
- **Vehicle:** Vauxhall Astra 2022 (BK25 XYZ)
- **Jobs:** 5 (pad replacement, disc inspection, fluid bleed, handbrake)
- **Tasks:** 30+ detailed service tasks
- **Technical Data:** Torque specs, measurements, procedures

## 🔄 Complete Workflow Example

```
1. START
   Open application

2. LOAD DEMO
   Quick Load Demo → Cooling Systems
   (Form auto-populates in 1 second)

3. REVIEW
   Check student name, vehicle, tasks
   (All fields pre-filled with realistic data)

4. CUSTOMIZE
   ├─ Modify student name
   ├─ Update vehicle details
   ├─ Add/edit tasks
   ├─ Mark completed items
   └─ Upload evidence photos

5. SAVE
   Export to JSON
   (Downloads: jobcard-StudentName-2026-01-20.json)

6. EXPORT
   Click "Export PDF" for final assessment document
   (Creates professional PDF report with all data)
```

## 📋 Storage Architecture

The app uses **three storage layers:**

| Layer | Purpose | Persistence | Capacity |
|-------|---------|-------------|----------|
| **File System** (json-templates/) | Static demo templates | Permanent | Unlimited |
| **IndexedDB** (Browser) | Session data, auto-save | Until cleared | ~50MB |
| **JSON Downloads** | Backups, sharing | On computer | Unlimited |

## ✨ Key Features

✅ **One-Click Loading**
- Load complete assessment data instantly from dropdown

✅ **Pre-configured Examples**
- Realistic job card data included
- 45+ example tasks per template

✅ **Easy Sharing**
- Export as JSON file
- Share via email, cloud, USB
- Import on any device

✅ **Organized Structure**
- `json-templates/` folder keeps demos organized
- Easy to add new templates
- Clear project layout

✅ **Data Portability**
- JSON format (no special software needed)
- Import/export anytime
- Multiple backup options

## 🎓 For Training Centers

### Setup Process
1. Place assessment templates in `json-templates/` folder
2. Update `index.html` dropdown to include new templates
3. Students open app and select their assessment
4. All pre-configured data loads
5. Students complete and export

### Example Setup
```
json-templates/
├── demo-cooling-systems.json       # Unit 1
├── demo-brake-systems.json         # Unit 2
├── demo-engine-systems.json        # Unit 3
├── demo-electrical-systems.json    # Unit 4
└── demo-transmission-systems.json  # Unit 5
```

## 🔧 Technical Details

### Supported Browsers
- Chrome/Chromium ✅
- Firefox ✅
- Safari ✅
- Edge ✅

### File Format
- JSON format (human-readable text)
- No special software needed
- Can be edited in any text editor

### Storage Limits
- IndexedDB: ~50MB per domain
- JSON files: Unlimited (on your computer)
- Browser history: Limited by browser

## ❓ Troubleshooting

### Demo dropdown not appearing?
- Refresh the page (Ctrl+R or Cmd+R)
- Clear browser cache
- Try different browser

### "Template not found" error?
- Check `json-templates/` folder exists
- Verify JSON file path is correct
- Look in browser console for details

### Changes not saving?
- Try exporting to JSON as backup
- Check if IndexedDB is enabled
- Clear browser storage and refresh

### Photo uploads not visible?
- Photos store in IndexedDB separately
- Re-upload after importing
- Use PDF export for complete record

## 📞 Need Help?

1. **Read Documentation:**
   - `JSON_IMPORT_GUIDE.md` - Detailed import/export guide
   - `PROJECT_STRUCTURE.md` - Project organization
   - `STORAGE_REVIEW.md` - Technical details

2. **Check Demo Files:**
   - Open `json-templates/demo-cooling-systems.json` in text editor
   - See complete structure and examples

3. **Test Locally:**
   - `python3 -m http.server 8000`
   - Open browser to `localhost:8000`

---

**Ready to get started?** 
👉 Open the application and try the **Quick Load Demo** dropdown! 

It should take less than 5 seconds to populate a complete job card. 📊
