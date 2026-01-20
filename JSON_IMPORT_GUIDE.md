# JSON Import/Export & Storage Guide

## Overview

The Motor Vehicle Job Card application now supports JSON-based data import and export, allowing you to:
- **Import** complete job cards from JSON files (templates or previous exports)
- **Export** your current job card data as JSON for backup or sharing
- **Use demo files** to quickly populate the form with realistic assessment data

## Files Included

### Demo Job Card: `demo-cooling-systems.json`
A complete example based on the 7290-702 Assessor Observation Recording Form for **Engine Cooling Systems** assessment.

**Contents:**
- Student: John Smith
- Vehicle: Ford Focus 2019 (CV26 ABC)
- 4 complete jobs with multiple tasks
- All customer requests and tools for cooling systems work
- Technical data and specifications

**To Use:**
1. Click "Import JSON Job Card" file selector in the form
2. Select `demo-cooling-systems.json`
3. The entire form will populate with the example data
4. Modify as needed for your actual assessment

## Storage Architecture

### IndexedDB Implementation

**Database Name:** `MotorVehicleJobCardDB` (Version 2)

**Object Stores:**

1. **jobCards** (keyPath: 'id')
   - Stores all job card data
   - Special entry with id='current' holds auto-saved draft
   - Other entries are named job cards saved by user

2. **photos** (keyPath: 'id', autoIncrement: true)
   - Stores compressed image data
   - Images are compressed to 1200px wide at 0.8 quality
   - Referenced from task.photos array by photoId

### Data Model

```javascript
// Job Card Structure
{
  studentName: "string",
  jobDate: "YYYY-MM-DD",
  jobLocation: "string",
  vehicleReg: "string",
  vehicleMake: "string",
  vehicleYear: "string",
  vehicleOdometer: "string",
  methodology: "string",          // Template key
  methodologyText: "string",
  methodologyNotes: "string",
  technicalData: "string",
  learnerSignature: "string",
  learnerDate: "YYYY-MM-DD",
  learnerProfile: "string",
  
  customerRequests: [
    { id: number, text: "string", completed: boolean }
  ],
  
  tools: [
    { id: number, name: "string" }
  ],
  
  jobs: [
    {
      id: number,
      requestId: number,
      requestText: "string",
      completed: boolean,
      tasks: [
        {
          id: number,
          description: "string",
          completed: boolean,
          photos: [photoId1, photoId2, ...]
        }
      ]
    }
  ]
}
```

## Using JSON Files

### Importing a Job Card

1. **From Demo File:**
   - Click "Import JSON Job Card" file selector
   - Choose `demo-cooling-systems.json`
   - Form populates automatically

2. **From Custom JSON:**
   - Export your own job card as JSON (button in form)
   - Share or store the JSON file
   - Import it anytime using the file selector
   - All data including tasks, requests, and tools are restored

### Exporting a Job Card

1. Enter all data into the form
2. Click "📥 Export to JSON" button
3. Browser downloads file: `jobcard-[StudentName]-[Date].json`
4. Store for backup or sharing

## ID Generation Strategy

The application uses multiple ID generation approaches:

| Entity | ID Type | Generation | Notes |
|--------|---------|-----------|-------|
| Jobs | Numeric | Incremented | Tracks currentJobId |
| Tasks | Numeric | Incremented | Tracks currentTaskId per job |
| Tools | Numeric | Incremented | Tracks currentToolId |
| Requests | Timestamp | Date.now() + index | Unique across sessions |
| Saved Cards | String | Date.now().toString() | Timestamp based ID |

## Available Templates

Six built-in templates are available for quick loading:

1. **Engine Systems Diagnosis & Repair**
   - Key: `engine-systems`
   - Requests: Diagnosis, oil change, gasket leak, etc.
   - Tools: Diagnostic equipment, wrenches, compression tester, etc.

2. **Brake Systems Service & Repair**
   - Key: `brake-systems`
   - Requests: Brake pad replacement, fluid bleed, etc.
   - Tools: Brake bleeding kit, caliper tools, etc.

3. **Transmission & Drivetrain Service**
   - Key: `transmission`
   - Requests: Oil service, clutch issues, CV boot, etc.
   - Tools: Fluid pump, seal puller, torque wrench, etc.

4. **Electrical Systems & Diagnostics**
   - Key: `electrical-systems`
   - Requests: Battery charging, headlights, faults, etc.
   - Tools: Multimeter, battery tester, scanner, etc.

5. **Cooling & Climate Systems** ⭐ (Matches demo file)
   - Key: `cooling-systems`
   - Requests: Overheating, leaks, thermostat, etc.
   - Tools: Pressure tester, thermometer, AC gauge set, etc.

6. **Suspension & Steering Systems**
   - Key: `suspension-steering`
   - Requests: Alignment, tire wear, shock replacement, etc.
   - Tools: Alignment equipment, spring compressor, etc.

## Storage Workflow

### Saving Data

**Three Save Methods:**

1. **Auto-Save to IndexedDB**
   ```javascript
   await saveJobCard();  // Saves to 'current' entry
   ```

2. **Save as Named Card**
   ```javascript
   await saveAsNewCard();  // Saves with custom name
   ```

3. **Export as JSON**
   ```javascript
   exportToJSON();  // Downloads .json file to computer
   ```

### Loading Data

**Three Load Methods:**

1. **Load Auto-Saved**
   ```javascript
   await loadJobCard();  // Loads from 'current' entry
   ```

2. **Load Named Card**
   ```javascript
   await loadCard(cardId);  // Loads specific saved card
   ```

3. **Import from JSON**
   ```javascript
   importFromJSON(event);  // Imports from file upload
   ```

## Workflow Example: Using Demo File

### Step 1: Import Demo
1. Open the application
2. Scroll to "Load Template or Import Data" section
3. Click "Import JSON Job Card"
4. Select `demo-cooling-systems.json`
5. All fields populate with example assessment data

### Step 2: Review & Modify
1. Verify student name and vehicle details
2. Review customer requests (tasks to be completed)
3. Check all 4 jobs with their tasks
4. Review technical specifications entered

### Step 3: Add More Work
1. Add new customer requests using the input field
2. Create additional jobs for other requests
3. Add tasks to jobs
4. Upload photos for evidence

### Step 4: Save Your Work
- **Option A:** Click "Save Job Card" to auto-save in IndexedDB
- **Option B:** Enter a name and click "Save as New Card" to save with a label
- **Option C:** Click "Export to JSON" to download a backup

## Technical Improvements Made

### 1. ✅ Added JSON Import/Export
- `importFromJSON(event)` - Loads JSON file into form
- `exportToJSON()` - Exports form data as JSON file
- Validation of basic structure

### 2. ✅ Enhanced UI
- Added file input for JSON import
- Added export button next to import
- Added demo file reference
- Better section labeling

### 3. ✅ Improved Error Handling
- JSON parsing error handling
- User-friendly error messages
- File validation

## Browser Compatibility

| Feature | Chrome | Firefox | Safari | Edge |
|---------|--------|---------|--------|------|
| IndexedDB | ✅ | ✅ | ✅ | ✅ |
| File API | ✅ | ✅ | ✅ | ✅ |
| Blob/URL | ✅ | ✅ | ✅ | ✅ |
| JSON.stringify | ✅ | ✅ | ✅ | ✅ |

## Known Limitations & Future Improvements

### Current Limitations
1. Photos stored in IndexedDB - not included in JSON export (use PDF for full export)
2. Templates are hardcoded - not loaded from external files
3. No cloud sync - stored only locally
4. No data validation on import - relies on user providing valid JSON

### Recommended Future Improvements
1. **Prioritize:**
   - Add template loading from JSON files
   - Implement photo attachment handling in JSON
   - Add data schema versioning for migrations

2. **Medium Priority:**
   - Cloud backup/sync support
   - Data validation schema
   - Template sharing/marketplace

3. **Nice to Have:**
   - Bulk import multiple job cards
   - Template builder UI
   - Data compression for storage optimization

## Troubleshooting

### "Invalid job card format" Error
- Ensure JSON file has required fields: `studentName`, `customerRequests`, or `jobs`
- Check JSON syntax is valid
- Use `demo-cooling-systems.json` as a reference

### Photos Not Showing After Import
- JSON export does not include photos (stored separately in IndexedDB)
- Re-upload photos after importing
- Use PDF export for complete documentation with photos

### IndexedDB Quota Exceeded
- Clear browser storage
- Export JSON backup before clearing
- Compress images more aggressively

### Changes Not Saving
- Enable IndexedDB in browser settings
- Disable private/incognito mode
- Try exporting to JSON as backup

