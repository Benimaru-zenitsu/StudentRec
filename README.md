# Class Tracker 📚
### A mobile-friendly student academic tracker — no server, no login, no internet required

---

## What is Class Tracker?

Class tracker is a single-file web app for teachers to track student attendance, assignments, topics covered, and academic performance across multiple subjects. Everything lives in one `classpulse.html` file that you open in any browser — on your phone, tablet, or computer.

All data is saved locally in your browser. Nothing is sent to any server.

---

## Quick Start

1. Download `index.html`
2. Open it in any browser (Chrome, Safari, Firefox)
3. Tap **+ Subject** in the top right to add your first subject
4. Go to **Setup → Students** and add your student roster
5. Start marking attendance from the **Classes** tab

> **Tip for mobile:** In Chrome/Safari, tap the share button and choose **"Add to Home Screen"** to use Class tracker like an app on your phone.

---

## Features

### ✅ Attendance (Classes tab)
- Mark each student **Present / Absent / Late** with a single tap
- Two input modes:
  - **Manual** — tap P / A / L for each student individually
  - **Paste list** — paste roll numbers or names of present students; everyone else is automatically marked absent
- Each class is numbered automatically (Class 1, Class 2 …)
- Log the **topic covered** during class
- **Change the date** to load and edit any previous class — existing statuses are restored automatically
- Recent classes shown below with subject code, date, and P / A / L counts
- Tap any recent class to view the full student list, filterable by Present / Absent / Late (defaults to showing absent students)

### 📝 Assignments (Tasks tab)
- Add assignments per subject with a title, due date, and optional max marks
- Toggle each student's submission status: **Pending → Submitted**
- Overdue assignments are flagged automatically
- Progress bar shows submission rate at a glance

### 📖 Topics (Topics tab)
- Chronological log of all topics covered per subject
- Automatically populated from whatever you enter during class attendance
- Useful for students to follow the syllabus

### 📊 Records tab
- **Expandable student cards** — tap to expand any student's full record
- Each card shows:
  - Personal details (roll number, phone, email)
  - Overall attendance count (Present / Late / Absent) and percentage
  - **Subject-wise breakdown table** with attendance % and assignment submission per subject
  - **GitHub-style attendance calendar** for each subject — green for present, red for absent, amber for late, neutral for weekends and days with no class
  - Assignment submission rate with progress bar
  - Teacher remarks / notes
- **Low attendance alert banner** — automatically flags any student below 75%
- Search by name or roll number

### ⚙️ Setup tab
Three sub-sections:

**Students**
- Add students one at a time (roll number, name, phone, email)
- Or bulk-paste a list in the format `01 Ahmed Ali`
- Roll number is the student's permanent ID
- Edit name, phone, email at any time
- Students are shared across all subjects by default

**Subjects**
- Add subjects with a name, alphanumeric code (used as ID), and optional room number
- Subject code is displayed in the subject strip and throughout the app
- Remove individual students from a specific subject by tapping their chip in the subject card

**Teachers**
- Add a teacher per subject (name, subject code as ID, phone, email)
- **Expandable teacher cards** — tap to see contact details
- Edit or remove teachers at any time

### 💾 Backup & Restore
- Tap the 💾 button in the top bar
- **Export** — downloads a `.json` file with all your data (students, classes, assignments, remarks)
- **Import** — upload a previously exported file to fully restore the app on any device
- Recommended: export a backup at the end of each week

---

## Data Storage

All data is saved in your browser's `localStorage` under the key `cp3`.

| Data | Estimated size for 55 students × 5 subjects × 80 classes |
|---|---|
| Students | ~5 KB |
| Attendance records | ~120 KB |
| Assignments & topics | ~10 KB |
| **Total** | **~135 KB** |

`localStorage` allows up to **5 MB** — well above what this app will ever use.

**Important:** If you clear your browser's cache or site data, all records will be lost. Always keep an exported backup.

---

## ID System

| Entity | ID field | Example |
|---|---|---|
| Student | Roll number | `01`, `23` |
| Subject | Subject code (alphanumeric) | `MTH101`, `ENG202` |
| Teacher | Subject code of the subject they teach | `MTH101` |

IDs are set at creation and cannot be changed (roll number and subject code are permanent).

---

## Attendance Calendar (Records page)

Inside each student's expanded record, every subject has a small calendar similar to GitHub's contribution graph.

| Cell colour | Meaning |
|---|---|
| 🟩 Dark green | Present |
| 🟥 Dark red | Absent |
| 🟨 Dark amber | Late |
| ⬛ Dark neutral | Weekend (Saturday / Sunday — no class expected) |
| ⬜ Light neutral | Weekday with no class recorded |

The calendar spans from the date of the first recorded class to today, aligned to full weeks starting on Monday.

---

## Attendance Thresholds

| Percentage | Colour | Meaning |
|---|---|---|
| 75% and above | 🟢 Green | Good standing |
| 50% – 74% | 🟡 Amber | Warning |
| Below 50% | 🔴 Red | Poor attendance |

Students below 75% are flagged in the alert banner at the top of the Records page.

---

## File Structure

The entire app is contained in a single file:

```
classpulse.html
├── <style>        CSS — all visual styling and theming
├── <body>
│   ├── .app       Main app shell
│   │   ├── .header          Top bar
│   │   ├── .subj-strip      Subject pill tabs
│   │   ├── .pages           5 content pages (one visible at a time)
│   │   └── .bottom-nav      Navigation bar
│   └── Modals               7 slide-up sheets for forms
└── <script>       All JavaScript — data, logic, rendering
```

---

## How to Modify

### Change the attendance threshold (default 75%)

Find these two functions in the `<script>` block and update the numbers:

```javascript
function pctClass(p){
  return p >= 75 ? 'pfg' :   // change 75 to your threshold
         p >= 50 ? 'pfa' :   // change 50 to your warning threshold
                   'pfr';
}
function badgeClass(p){
  return p >= 75 ? 'bg' :
         p >= 50 ? 'ba' :
                   'br';
}
```

Also update the low-attendance alert in `renderRecords()`:
```javascript
return tc > 0 && Math.round(tp / tc * 100) < 75; // change 75
```

### Change the accent colour (default purple)

In the `:root` block at the top of the `<style>` section:

```css
--accent:        #7c6af7;   /* replace with any hex colour */
--accent2:       #a99fff;
--accent-bg:     rgba(124,106,247,0.13);
--accent-border: rgba(124,106,247,0.4);
```

### Add a new student field (e.g. Section)

1. Add an input to the `#m-addStudent` modal HTML
2. Include the field in `addStudent()` and `bulkAddStudents()`
3. Display it in `renderSetupStudents()` and the record card in `renderRecords()`

---

## Browser Compatibility

| Browser | Supported |
|---|---|
| Chrome (Android / Desktop) | ✅ |
| Safari (iOS / macOS) | ✅ |
| Firefox | ✅ |
| Edge | ✅ |
| Internet Explorer | ❌ |

---

## Limitations

- Data is **device and browser specific** — using a different browser on the same device will show an empty app
- No cloud sync — use the Export / Import backup system to move data between devices
- No multi-user access — designed for a single teacher
- No print/PDF export in the current version

---

## Planned / Suggested Future Features

- [ ] Export student records to PDF or CSV
- [ ] Grade/marks entry per assignment
- [ ] Print-friendly attendance sheet
- [ ] Percentage threshold customisation in UI
- [ ] Dark / light mode toggle

---

## License

Free to use, modify, and distribute for personal and educational purposes.

---

*Built as a single-file vanilla HTML/CSS/JavaScript app. No frameworks, no dependencies, no internet required after loading.*
