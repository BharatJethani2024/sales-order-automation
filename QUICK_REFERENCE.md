# Quick Reference Card

Bookmark this for fast lookups.

---

## Project at a Glance

**Name**: Sales Order & Inventory Automation  
**Tech**: Make.com + Google Gemini AI + Google Drive/Sheets  
**Purpose**: Convert handwritten receipts → Structured data in spreadsheets  
**Result**: Eliminates 2 hours of manual data entry per day  
**Skill Tags**: #BPA #DataExtraction #AI #CloudAutomation #NoCode  

---

## 60-Second Explanation

Handwritten receipt → Photo → Google Drive → Make.com watches folder → Gemini AI reads image → Extracts JSON (customer, date, items) → Iterator loops items → Each item becomes a row in Google Sheets. Done.

---

## File Map

| File | Purpose | Read When |
|------|---------|-----------|
| README.md | Main overview, features, tech stack | Starting out |
| SETUP_GUIDE.md | Step-by-step setup (15 min) | First time installing |
| PROMPT_ENGINEERING.md | Customize what fields to extract | Tweaking for custom needs |
| VIDEO_DEMO_GUIDE.md | Should I record a demo? | Deciding on video |
| GITHUB_UPLOAD_CHECKLIST.md | Complete upload process | Publishing to GitHub |
| QUICK_REFERENCE.md | This file – fast lookup | When in a hurry |

---

## Architecture in 30 Seconds

```
Input: Receipt Photo
   ↓
Google Drive (stores receipt)
   ↓
Make.com watches folder
   ↓
Google Gemini AI reads & extracts JSON
   ↓
JSON Parser validates
   ↓
Iterator loops each item
   ↓
Google Sheets inserts one row per item
   ↓
Output: Structured data in spreadsheet
```

---

## Setup: 5-Step Fast Path

1. **Google Drive**: Create folder `01-Incoming Receipts` + sheet with columns (Customer, Date, Item, Qty)
2. **Make.com**: Create account, import scenario, connect Drive + Gemini + Sheets
3. **Gemini API**: Get free API key from Google AI Studio
4. **Configure Modules**: Set folder ID, API key, sheet mapping
5. **Test**: Upload receipt, click "Run once", check sheet for data

---

## Module Cheat Sheet

| # | Module | Job | Succeeds When |
|---|--------|-----|---|
| 1 | Google Drive Watcher | Detects new files | Finds receipt in folder |
| 5 | File Download | Gets file content | File loads without error |
| 12 | Gemini Upload | Sends to AI | File accepted by Google |
| 10 | Gemini Extract | Reads & extracts | Returns valid JSON |
| 16 | JSON Parser | Validates structure | Parses without error |
| 15 | Iterator | Loops items | Creates one bundle per item |
| 14 | Sheets Insert | Writes to sheet | Row appears in sheet |

---

## Common Errors & Fixes

| Error | Root Cause | Fix |
|-------|-----------|-----|
| `[400] Unsupported MIME type: spreadsheet` | Sheet in watched folder | Move sheet to parent folder |
| `[429] Quota exceeded` | Rate limit hit | Wait 30 sec, retry |
| `[400] Request contains invalid argument` | JSON nested wrong | Rebuild data structure |
| Blank Item/Qty columns | Iterator not looping | Map Array to `{{16.items}}`, turn Map ON |
| `Source is not valid JSON` | Gemini wrapped in backticks | Set Response Format = JSON |

---

## Make.com Navigation

- **Scenario Editor**: Where you build the flow
- **Execution History**: Bottom of screen, shows all runs + errors
- **"Run once"**: Purple button, test immediately
- **"Every 15 minutes"**: Toggle to auto-run schedule
- **Module Settings**: Double-click any circle to edit

---

## Google Sheets Columns (Required)

| Column | Data Type | Example |
|--------|-----------|---------|
| A | Customer Name | Bharat ji Jagatpura 92578-39554 |
| B | Date | 2025-02-23 |
| C | Item Name | Seeli Dhoti |
| D | Quantity | 1 |

**Required in row 1 as headers** (exact spelling matters for Make.com)

---

## Gemini Prompt (Minimal)

```
Extract this JSON from the receipt:
{
"customer_name": "String",
"date": "YYYY-MM-DD",
"items": [{"item_name": "String", "quantity": Number}],
"total_items_count": Number
}
Output all text in English. If missing, return null. Return JSON only, no markdown.
```

For customization, see `PROMPT_ENGINEERING.md`.

---

## Data Flow Mapping Reference

| Field Source | Make.com Reference |
|--------------|-------------------|
| Customer from Gemini | `{{16.customer_name}}` |
| Date from Gemini | `{{16.date}}` |
| Item name (looping) | `{{15.item_name}}` |
| Item quantity (looping) | `{{15.quantity}}` |

**16** = JSON module, **15** = Iterator module

---

## Cost Breakdown (Monthly)

| Service | Free Tier | Cost if Scaled |
|---------|-----------|---|
| Google Gemini | 20 req/min, 1,500/day | ~$0.075 per 1K tokens |
| Google Drive | 15 GB | $2/100GB |
| Google Sheets | Unlimited | Included with Drive |
| Make.com | 1,000 ops/month | $9-15/month |
| **Total** | **$0** | **$12-20/month** |

---

## Testing Checklist

- [ ] Folder `01-Incoming Receipts` created in Drive
- [ ] Google Sheet has headers in row 1
- [ ] All 7 modules show in Make.com (connected with lines)
- [ ] Test receipt uploaded to folder
- [ ] Clicked "Run once" → all circles turned green
- [ ] New row appeared in Google Sheet with correct data

If anything fails, **check which circle turned red** and look at its error message.

---

## GitHub Upload Quickstart

```bash
# Create folder
mkdir sales-order-automation && cd sales-order-automation

# Copy all .md files + screenshots/ + sample_data/
# (Instructions in GITHUB_UPLOAD_CHECKLIST.md)

# Initialize git
git init
git add .
git commit -m "Initial commit: Receipt OCR automation"

# Push to GitHub
git remote add origin https://github.com/YOUR_USERNAME/sales-order-automation.git
git branch -M main
git push -u origin main
```

**Then share the GitHub link on LinkedIn, Upwork, resumes.**

---

## Customization Ideas (Next Steps)

- [ ] Add price extraction (for revenue tracking)
- [ ] Extract tax & discounts
- [ ] Add SKU/batch tracking (cannabis-specific)
- [ ] Validate against inventory database
- [ ] Send to Salesforce/QuickBooks
- [ ] Duplicate detection (flag re-entered receipts)
- [ ] Mobile app to snap photos directly

See `PROMPT_ENGINEERING.md` for ready-to-use prompts for each.

---

## Key Contacts / Resources

- **Make.com Help**: https://support.make.com
- **Gemini Docs**: https://ai.google.dev/gemini-api/docs
- **Google Drive API**: https://developers.google.com/drive
- **Google Sheets API**: https://developers.google.com/sheets

---

## For Job Interviews

**Q: How did you build this?**  
A: "I used Make.com's no-code automation platform to orchestrate the flow, Google's Gemini AI for OCR and extraction, and Google Sheets as the database. It demonstrates my ability to integrate cloud services, automate business processes, and think about scalability."

**Q: How would you scale this?**  
A: "For higher volume, I'd upgrade to paid tiers of Google Gemini (higher rate limits) and Make.com (more operations). I'd add error handling to route failures to a manual review queue, implement duplicate detection, and potentially build a custom web interface instead of Google Sheets."

**Q: What was the hardest part?**  
A: "Getting the data structure right in Make.com's Iterator. The first few runs, Gemini's output was nested one level too deep. Once I debugged that and mapped the fields correctly, everything clicked. It taught me that with automation, testing early and reading error messages carefully is crucial."

---

## One-Liner Pitch

"I built a receipt OCR automation that processes handwritten sales orders from images into a Google Sheet in seconds using Make.com and Google Gemini AI—cutting manual data entry time by 95%."

---

## Links to Share

- **GitHub**: `https://github.com/YOUR_USERNAME/sales-order-automation`
- **LinkedIn Post**: Reference the GitHub link + demo video (if you make one)
- **Upwork**: Add GitHub link to portfolio section
- **Personal Site**: Embed GitHub card or link

---

## Pro Tips

✅ **Do**: Keep receipts clean + well-lit for better OCR  
✅ **Do**: Test with 5+ different receipt samples  
✅ **Do**: Document any custom prompts you create  
✅ **Do**: Monitor Make.com execution history weekly  

❌ **Don't**: Commit API keys to GitHub (use `.gitignore`)  
❌ **Don't**: Assume Gemini will always extract perfectly (handle nulls)  
❌ **Don't**: Leave the scenario running 24/7 without monitoring  

---

## Glossary

| Term | Means |
|------|-------|
| BPA | Business Process Automation |
| OCR | Optical Character Recognition (reading images) |
| JSON | Data format (structured text) |
| Iterator | Loop that processes multiple items one at a time |
| Webhook | Make.com listener for events |
| Rate Limit | Max requests per time window (20/min free Gemini) |
| Payload | The data sent between modules |
| Module | A step/task in the Make.com scenario |

---

**Last Updated**: June 2026  
**Version**: 1.0  
**Status**: Production-Ready ✅

---

Keep this file bookmarked. Reference it when you're coding, interviewing, or teaching others.

Good luck! 🚀
