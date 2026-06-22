# Complete Setup Guide: Sales Order & Inventory Automation

Follow these steps in order. Each section takes 5–10 minutes.

---

## Part 1: Google Drive Setup (5 minutes)

### Step 1.1: Create the Folder Structure

1. Go to [Google Drive](https://drive.google.com)
2. In "My Drive," right-click and select **New > Folder**
3. Name it `01-Incoming Receipts` (exact name matters for Make.com)
4. Open this folder

### Step 1.2: Create the Output Spreadsheet

1. Inside the folder, right-click → **New > Google Sheets**
2. Name it `Copy of Sales_Orders_Automation` (or your choice, you'll configure it in Make.com)
3. In the new sheet, create a header row with these columns:

| A | B | C | D |
|---|---|---|---|
| Customer Name | Date | Item Name | Quantity |

4. Make sure row 1 contains exactly these headers (column names matter for data mapping)
5. Save the sheet (Ctrl+S or Cmd+S)

### Step 1.3: Get Your Folder ID

1. Open the `01-Incoming Receipts` folder in Drive
2. Look at the URL in the address bar: `https://drive.google.com/drive/folders/XXXXX...`
3. Copy the long ID after `/folders/` – you'll need this in Make.com

---

## Part 2: Google Gemini API Setup (5 minutes)

### Step 2.1: Enable Gemini API

1. Go to [Google AI Studio](https://ai.google.dev/aistudio)
2. Click **"Create API key"** (if first time)
3. Copy the API key – you'll need this in Make.com
4. Keep this key safe; don't share it publicly

### Step 2.2: Verify Free Tier Access

1. Go to [Google Gemini Pricing](https://ai.google.dev/pricing)
2. Confirm you see the free tier: **20 requests per minute, 1,500 requests/day**
3. No credit card needed for free tier

---

## Part 3: Make.com Setup (15 minutes)

### Step 3.1: Create a Make.com Account

1. Go to [Make.com](https://www.make.com)
2. Sign up with email or Google account
3. Verify your email

### Step 3.2: Create a New Scenario

1. Click **Create** → **New Scenario**
2. Name it: `Receipt OCR to Google Sheets`
3. You'll see a blank canvas with a trigger box in the middle

### Step 3.3: Add the Google Drive Watcher (Module 1)

1. Click the box in the middle, search for `Google Drive`
2. Select **Google Drive > Watch Files in a Folder**
3. Click **Save**
4. A settings panel opens on the right:
   - **Connection**: Click **Add**, then authorize your Google Drive account
   - **Watch Files**: Select `create` (triggers on new files)
   - **Choose a Drive**: Select `My Drive`
   - **File Types to Watch**: Select `All`
   - **Limit**: Set to `2`
   - **Select the Folder to be Watched**: Browse and select `01-Incoming Receipts`
5. Click **Save**

### Step 3.4: Add the File Download Module (Module 5)

1. Click the arrow/line extending from your Google Drive module
2. Search for `Google Drive > Get a File`
3. Settings:
   - **Connection**: Use the same Google Drive connection
   - Under **Expect**, set format conversions:
     - Documents → MS Word
     - Spreadsheets → MS Excel
     - Presentations → MS PowerPoint
     - Drawings → JPEG
   - Under **Enter manually**, set:
     - **File ID**: Click the mapping icon, open Module 1, select `id`
4. Click **Save**

### Step 3.5: Add Gemini AI Upload (Module 12)

1. Click the arrow, search for `Gemini AI > Upload a File`
2. Settings:
   - **Connection**: Click **Add**, paste your Google Gemini API key
   - **Name**: Map to Module 5 → `name`
   - **Data**: Map to Module 5 → `data`
   - **Use File Search Store**: Leave as `No`
3. Click **Save**

### Step 3.6: Add Gemini AI Extract (Module 10)

1. Click the arrow, search for `Gemini AI > Create a Completion`
2. Settings:
   - **Connection**: Use the Gemini connection from Step 3.5
   - **Model**: Select `Gemini 3.5 Flash`
   - **Messages**: Expand and configure:
     - **Role**: `User`
     - **Parts**: Add two parts:
       - Part 1 (Text): Copy-paste this prompt exactly:

```
Act as a data extraction agent. Read the receipt image and extract the information into this exact JSON object:
{
"customer_name": "String",
"date": "YYYY-MM-DD",
"items": [
{"item_name": "String", "quantity": Number}
],
"total_items_count": Number
}

Output ALL text values in English only. Do not output any Hindi or Devanagari characters. Transliterate names of people and places into Roman script. Translate product names into English.

If a value is missing or unreadable, return null. Return only the JSON, with no markdown, code fences, or commentary.
```

       - Part 2 (File): 
         - **Message Type**: `File`
         - **Mime Type**: `image/jpeg`
         - **File URI**: Map to Module 12 → `Uri`

   - **Advanced Settings** (scroll down and toggle ON):
     - **Response Format**: Select `application/json`

3. Click **Save**

### Step 3.7: Add JSON Parser (Module 16)

1. Click the arrow, search for `JSON > Parse JSON`
2. Settings:
   - **JSON string**: Map to Module 10 → in the Candidates array, select the text output
     - (This may show as `{{10.candidates[].content.parts[].text}}`)
   - Leave **Data structure** blank for now
3. Click **Save**

### Step 3.8: Add Iterator (Module 15)

1. Click the arrow, search for `Flow Control > Iterator`
2. Settings:
   - Click the **Map** toggle (top right of Array field) to turn it ON
   - **Array**: Map to Module 16 → `items`
3. Click **Save**

### Step 3.9: Add Google Sheets Insert (Module 14)

1. Click the arrow, search for `Google Sheets > Add a Row`
2. Settings:
   - **Connection**: Click **Add**, authorize your Google Sheets account
   - **Search Method**: Select `Search by path`
   - **Drive**: Select `My Drive`
   - **Spreadsheet**: Browse and select `Copy of Sales_Orders_Automation`
   - **Sheet**: Select `Sheet1`
   - **Table contains headers**: Select `Yes`
   - **Use column headers as IDs**: Select `Yes`
   - **Values in columns**: Map each column:
     - **Customer Name (A)**: Map to Module 16 → `customer_name`
     - **Date (B)**: Map to Module 16 → `date`
     - **Item Name (C)**: Map to Module 15 → `item_name`
     - **Quantity (D)**: Map to Module 15 → `quantity`
3. Click **Save**

### Step 3.10: Connect All Modules

Your scenario should now look like this:

```
[Google Drive Watcher] → [File Download] → [Gemini Upload] → [Gemini Extract] → [JSON Parse] → [Iterator] → [Sheets Insert]
```

If lines are missing, click and drag from one module to the next.

---

## Part 4: Test the Scenario (5 minutes)

### Step 4.1: Prepare a Test Receipt

1. Take a photo of a handwritten receipt, or use a sample image
2. Make sure it shows:
   - Customer name
   - Date
   - At least 2 line items with quantities

### Step 4.2: Upload the Test Image

1. Go to Google Drive → `01-Incoming Receipts` folder
2. Upload your test receipt image (JPG or PNG)

### Step 4.3: Run the Scenario

1. In Make.com, click the purple **"Run once"** button (bottom left)
2. Watch the circles light up:
   - Green = successful
   - Red = error (click the circle to see the error message)
3. Wait 10–15 seconds for the full run

### Step 4.4: Check Google Sheets

1. Open your `Copy of Sales_Orders_Automation` sheet
2. You should see a new row with:
   - Customer Name (Column A)
   - Date (Column B)
   - Item Name (Column C)
   - Quantity (Column D)
3. If data appears → **Success!** ✅
4. If blank cells → Check the error message on the red circle in Make.com

---

## Part 5: Troubleshooting

### Issue: Red circle on Module 1 (Google Drive Watcher)

**Error**: `Connection refused` or `403 Forbidden`

**Fix**:
1. Double-click Module 1
2. Click **"Add"** next to Connection (top)
3. Re-authorize your Google Drive account
4. Try again

---

### Issue: Red circle on Module 10 (Gemini AI Extract)

**Error**: `[400] Unsupported MIME type: application/vnd.openxmlformats...`

**Fix**: Your output sheet ended up in the watched folder. Move it to the parent folder.
1. Go to Google Drive
2. Cut `Copy of Sales_Orders_Automation` from the watched folder
3. Paste it into `My Drive` (parent level)
4. Re-run

---

### Issue: Red circle on Module 10

**Error**: `[429] You exceeded your current quota`

**Fix**: You hit the rate limit (20 requests/minute free tier).
1. Wait 30 seconds
2. Click **"Run once"** again
3. If this happens repeatedly, consider upgrading to paid Gemini tier

---

### Issue: Google Sheets row is blank

**Error**: No error circles, but columns C & D (Item Name, Quantity) are empty

**Fix**: The Iterator isn't breaking down items correctly. Likely cause: data structure issue or wrong Array mapping.
1. Double-click Module 15 (Iterator)
2. Confirm **Array** field shows `{{16.items}}`
3. Confirm **Map** toggle is ON
4. Click **Save**
5. Re-run with a fresh image

---

### Issue: JSON Parse fails

**Error**: `Source is not valid JSON`

**Fix**: Gemini wrapped the JSON in markdown backticks.
1. Double-click Module 10 (Gemini)
2. Scroll to **Advanced Settings** (toggle ON)
3. Set **Response Format** to `application/json`
4. Click **Save**
5. Re-run

---

## Part 6: Going to Production

### Enable Scheduled Runs

1. In Make.com, scroll to the bottom of the scenario
2. Find **"Every 15 minutes"** toggle (bottom left, next to "Run once")
3. Toggle it **ON**
4. The scenario now auto-runs every 15 minutes
5. New receipts in the folder trigger processing automatically

### Monitor Execution

1. Click the **"Execution History"** tab at the bottom
2. View all past runs, errors, and data processed
3. Retry failed runs or manually trigger as needed

### Backup Your Data

1. Weekly, download your Google Sheet as CSV:
   - Open sheet → **File > Download > CSV**
   - Store locally or in cloud backup

---

## Cost Tracking

After 1 week, check your actual usage:

**Google Gemini**:
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Navigate to **Gemini API > Usage**
3. See tokens used and cost

**Make.com**:
1. In Make.com, go to **Settings > Plans & Usage**
2. See operations used (target: stay under 1,000/month on free tier)

---

## Next Steps

Once running smoothly:

1. **Customize the Prompt** → Edit the Gemini extraction prompt to capture additional fields (discount, total price, tax, etc.)
2. **Add Validation** → Use Make's conditional logic to flag suspicious entries (negative quantity, future dates, etc.)
3. **Integrate Accounting** → Connect Sheets data to QuickBooks or Salesforce via Make.com connectors
4. **Scale Up** → Move to paid tiers if volume exceeds free tier limits

---

## Support Resources

- **Make.com Help**: https://support.make.com
- **Google Gemini Docs**: https://ai.google.dev/gemini-api/docs
- **Google Sheets API**: https://developers.google.com/sheets/api
- **Community Forums**: https://www.make.com/en/community

---

**Congratulations!** Your automation is now live. 🎉
