# Should You Record a Demo Video?

## TL;DR

**Short answer**: Screenshots + GIFs are better than video for this project. Videos are nice-to-have, not essential.

---

## Why Screenshots > Video for This Project

### Video Cons for GitHub

- **Maintenance burden**: If you update the UI in 6 months, video becomes outdated; screenshots take 2 minutes to refresh
- **Storage**: Video files are 50–500 MB; GitHub has soft limits on large files
- **Viewing**: Most GitHub visitors skim docs; they don't watch videos
- **Mobile**: Many people browse GitHub on mobile; videos are awkward
- **Load time**: Large video files slow down your README

### Screenshots/GIFs Pros

- **Lightweight**: 50–200 KB per GIF/screenshot
- **Scannable**: Readers can see the output in 2 seconds
- **Easy to update**: Replace a PNG file, re-commit
- **Mobile-friendly**: Renders instantly on all devices
- **Archival**: Doesn't break links like YouTube can

---

## What to Include Instead of Video

### 1. **Annotated Screenshots** (5 minutes to create)

Take 3–4 key screenshots and add annotations:

- **Screenshot 1**: "Make.com scenario overview – full automation flow"
- **Screenshot 2**: "Google Drive folder structure with test receipt"
- **Screenshot 3**: "Google Sheets output with extracted data"
- **Screenshot 4**: "Make.com execution logs showing successful run"

**Tools**:
- Mac: Preview (built-in) or Skitch (free, adds arrows/text)
- Windows: Snagit ($50) or Screenshot Path (free)
- Online: Annotate.com (free, no signup)

### 2. **Animated GIF of the Workflow** (10 minutes)

Show: Upload receipt → Automation runs → Data appears in sheet

**Tools**:
- Mac: Licecap (free, super easy)
- Windows: ScreenToGif (free)
- Online: ezgif.com (free, upload MP4 convert to GIF)

**Steps**:
1. Record 15–20 second video of the flow (use OBS, QuickTime, or Snagit)
2. Convert to GIF
3. Compress to < 5 MB
4. Upload to repo in `screenshots/` folder

### 3. **Data Samples** (Already done – just organize)

Include:
- `sample_input_receipt.jpg` – Example receipt image
- `sample_output.json` – Example Gemini extraction
- `sample_sheet_export.csv` – Example Google Sheet rows

---

## When Video IS Worth It

Record a video (5–10 min) **only if**:

- [ ] You're submitting to a portfolio/job application and they ask for video proof
- [ ] You're pitching this to a client and want to show live execution
- [ ] You're creating a tutorial course (YouTube channel, Udemy, etc.)

Otherwise: **Skip it.** Your time is better spent on documentation.

---

## If You Still Want a Video

### Option 1: YouTube (Best for Resumes/Portfolios)

**Steps**:
1. Record 5–10 min walkthrough using OBS (free) or Quicktime:
   - Show Make.com scenario
   - Upload a test receipt
   - Run the automation
   - Check Google Sheets result
2. Upload to YouTube (unlisted or public)
3. Embed link in GitHub README under **"Demo Video"** section

**YouTube Link Format**:
```markdown
## Demo Video

[Watch Full Demo](https://www.youtube.com/watch?v=YOUR_VIDEO_ID) (5 min)

Or see screenshots below for quick reference.
```

### Option 2: Loom (Quick, Shareable)

**Steps**:
1. Go to [Loom.com](https://www.loom.com) (free tier)
2. Record your screen (browser + Make.com + Google Sheets)
3. Share the link
4. Add to README:

```markdown
## Quick Demo

[View on Loom](https://www.loom.com/share/YOUR_LOOM_ID) (3 min)
```

**Pros**: No uploading, instant share, nice UI  
**Cons**: Loom account required, link can expire

### Option 3: GitHub Releases (Upload MP4)

**Steps**:
1. Record and export as MP4 (< 100 MB)
2. Go to GitHub repo → **Releases**
3. Create a new release
4. Attach the MP4 file
5. Add link to README

```markdown
## Demo Video

[Download Demo Video](https://github.com/your-repo/releases/download/v1.0/demo.mp4) (5 min, 50 MB)
```

---

## Recommended Approach for Your GitHub

**Create this file structure** (no video required):

```
README.md                        # Main documentation
├── SETUP_GUIDE.md               # Step-by-step setup
├── PROMPT_ENGINEERING.md        # Customization guide
├── screenshots/
│   ├── 01_scenario_overview.png     # Annotated Make.com flow
│   ├── 02_drive_setup.png           # Folder structure
│   ├── 03_sheet_output.png          # Sample data in Sheets
│   ├── 04_execution_log.png         # Successful run
│   └── demo.gif                     # 20-second workflow GIF
└── sample_data/
    ├── receipt_sample.jpg           # Example input
    ├── extracted_output.json        # Example JSON
    └── sheet_export.csv             # Example rows
```

**In your README**, add a "Screenshots" section:

```markdown
## Screenshots

### Automation Flow
![Make.com scenario](screenshots/01_scenario_overview.png)

### Quick Demo (Animated)
![Demo GIF](screenshots/demo.gif)

### Sample Output
![Google Sheets output](screenshots/03_sheet_output.png)

See [SETUP_GUIDE.md](SETUP_GUIDE.md) for detailed walkthrough.
```

---

## Portfolio Tip

If this is for a **job application or portfolio**:

1. **Do add one short video** (5 min) to YouTube or Loom
   - Shows it actually works
   - Proves you built it (not copy-pasted)
   - Interviewers can verify your skills
2. **Link it in your README** under a "Demo" section
3. **Keep it short**: Nobody watches a 20-min tutorial on GitHub

**Link format**:
```markdown
## 📹 Demo

[Watch on YouTube](https://youtube.com/...) - 5 minute walkthrough showing receipt upload, automation run, and data output.
```

---

## My Recommendation for You

Given that you're a **Sales Ops professional building a portfolio project**:

1. **Create 3–4 annotated screenshots** (30 min total) ✅
2. **Make a 15-second GIF** of the workflow (15 min) ✅
3. **Skip the long-form video for now** – Your docs are solid

**Later, if interviewing**:
- Record a quick 5-min YouTube showing it live
- Link in your Cover Letter (not GitHub)
- This proves active maintenance + live-working code

---

## Screenshots You Should Take

### Screenshot 1: Make.com Scenario Overview
**File**: `screenshots/01_scenario_overview.png`

Content: Show the full scenario with all 9 modules connected.

Annotation arrow pointing to each:
1. "Watch folder for new receipts"
2. "Download file from Drive"
3. "Upload to Gemini"
4. "Extract JSON from image"
5. "Parse JSON"
6. "Loop through items"
7. "Insert to Google Sheets"

### Screenshot 2: Google Drive Folder Setup
**File**: `screenshots/02_drive_folder.png`

Content: Show the folder structure in Drive:
- 01-Incoming Receipts (folder)
  - sample_receipt.jpg (file)
  - Copy of Sales_Orders_Automation (sheet)

Annotation: "Place receipts here → automation processes them"

### Screenshot 3: Google Sheets Output
**File**: `screenshots/03_sheet_output.png`

Content: Show completed rows in Google Sheets with:
- Customer Name
- Date
- Item Name
- Quantity

Annotation: "Output: 2 line items from one receipt, auto-populated in Sheets"

### Screenshot 4: Make.com Execution Log
**File**: `screenshots/04_execution_log.png`

Content: Show the execution history in Make.com with green checkmarks.

Annotation: "All modules ran successfully in 8 seconds"

---

## Quick GIF Tutorial

**15-Second GIF to Create**:

1. Open OBS Studio (free)
2. Set up recording area: Make.com browser window + Google Sheets on desktop
3. Record:
   - Drag receipt into Drive folder (2 sec)
   - Click "Run once" in Make.com (1 sec)
   - Watch modules light up green (5 sec)
   - Switch to Google Sheets window (1 sec)
   - Show new row populated (3 sec)
4. Export as MP4
5. Convert to GIF: [ezgif.com](https://ezgif.com)
6. Upload as `screenshots/demo.gif`

---

## Final Decision Matrix

| Scenario | Video? | Why |
|----------|--------|-----|
| Portfolio/resume project | Screenshot + GIF + optional YouTube | Prove it works + looks professional |
| Open-source contribution | GIF only | Community values lightweight docs |
| Client deliverable | YouTube link | Client appreciates proof of execution |
| Job interview | YouTube + link in cover letter | Not in GitHub, directly to hiring manager |
| Just sharing on GitHub | GIF + screenshots | Fast loading, easy to update |

---

**Your move**: Pick the "Just sharing on GitHub" path. Screenshots + GIF = professional, lightweight, maintainable. Video can wait if you need it later.

---

## Commands to Generate Your Screenshots

```bash
# Mac: Take screenshot of active window
cmd+shift+4, space, click window

# Windows: Use Snagit or builtin screenshot tool
win+shift+s

# Linux
gnome-screenshot -a   # Area select

# Convert MP4 to GIF (after recording)
ffmpeg -i demo.mp4 -vf "fps=5,scale=1280:-1:flags=lanczos,split[s0][s1];[s0]palgen[p];[s1][p]paletteuse" -loop 0 demo.gif
```

Done. Now create your GitHub repo with the docs structure I outlined. You've got a solid project. 🎉
