# Prompt Engineering Guide: Customizing Data Extraction

This guide shows how to modify the Gemini AI prompt to extract different fields or handle special cases.

---

## Base Prompt (Current)

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

---

## Customization Examples

### Example 1: Add Price Per Item & Total Price

**Use Case**: Track revenue, not just inventory

**Modified JSON Schema**:
```json
{
  "customer_name": "String",
  "date": "YYYY-MM-DD",
  "items": [
    {
      "item_name": "String",
      "quantity": Number,
      "price_per_unit": Number,
      "line_total": Number
    }
  ],
  "total_items_count": Number,
  "grand_total": Number
}
```

**Updated Prompt**:
```
Act as a data extraction agent. Read the receipt image and extract the information into this exact JSON object:
{
"customer_name": "String",
"date": "YYYY-MM-DD",
"items": [
{
"item_name": "String",
"quantity": Number,
"price_per_unit": Number,
"line_total": Number
}
],
"total_items_count": Number,
"grand_total": Number
}

Output ALL text values in English only. Transliterate names into Roman script. Translate product names into English.

For price fields:
- Extract the unit price (price per item, not total line price)
- Extract the line total (quantity × unit price)
- Extract the grand total (sum of all line totals)
- If a price is missing or unclear, return null

Return only the JSON, with no markdown, code fences, or commentary.
```

**How to Apply**:
1. In Make.com, double-click Module 10 (Gemini Extract)
2. Replace the **Text** prompt with the updated version above
3. Add new columns to your Google Sheet: `Price Per Unit` and `Line Total`
4. In Module 14 (Sheets), map the new fields:
   - Price Per Unit → `{{15.price_per_unit}}`
   - Line Total → `{{15.line_total}}`
5. Save and test

---

### Example 2: Extract Payment Method & Discounts

**Use Case**: Track how customers pay and discounts applied

**Modified JSON Schema**:
```json
{
  "customer_name": "String",
  "date": "YYYY-MM-DD",
  "payment_method": "String",
  "discount_amount": Number,
  "discount_percent": Number,
  "items": [
    {
      "item_name": "String",
      "quantity": Number
    }
  ],
  "total_items_count": Number
}
```

**Updated Prompt**:
```
Act as a data extraction agent. Read the receipt image and extract the information into this exact JSON object:
{
"customer_name": "String",
"date": "YYYY-MM-DD",
"payment_method": "String",
"discount_amount": Number,
"discount_percent": Number,
"items": [
{
"item_name": "String",
"quantity": Number
}
],
"total_items_count": Number
}

Extract payment method as one of: "cash", "card", "check", "mobile_pay", or "other".
If no discount is shown, set both discount fields to 0 (not null).
Return only JSON, no commentary.
```

---

### Example 3: Cannabis-Specific Fields (Potency, SKU)

**Use Case**: Track product potency (THC %), batch numbers, SKUs

**Modified JSON Schema**:
```json
{
  "customer_name": "String",
  "date": "YYYY-MM-DD",
  "items": [
    {
      "item_name": "String",
      "sku": "String",
      "quantity": Number,
      "thc_percent": Number,
      "batch_number": "String"
    }
  ],
  "total_items_count": Number
}
```

**Updated Prompt**:
```
Act as a data extraction agent for a cannabis distribution company. Read the receipt image and extract:
{
"customer_name": "String",
"date": "YYYY-MM-DD",
"items": [
{
"item_name": "String",
"sku": "String",
"quantity": Number,
"thc_percent": Number,
"batch_number": "String"
}
],
"total_items_count": Number
}

Rules:
- Extract SKU (product code) if visible on receipt
- Extract THC potency percentage if listed
- Extract batch/lot number if visible
- If any field is unreadable, return null (not a placeholder)
- Translate product names to English
- Return only JSON, no markdown.
```

---

### Example 4: Strict Validation (Fail If Missing Critical Fields)

**Use Case**: Only accept receipts with all required info; flag incomplete ones

**Modified Prompt**:
```
Act as a strict data extraction agent. Read the receipt image and extract:
{
"customer_name": "String",
"date": "YYYY-MM-DD",
"items": [
{
"item_name": "String",
"quantity": Number
}
],
"total_items_count": Number,
"is_valid": Boolean
}

Rules:
- Set "is_valid" to true ONLY if:
  - Customer name is present
  - Date is present and in YYYY-MM-DD format
  - At least 1 item is listed
  - All item names and quantities are readable
- If ANY required field is missing or unclear, set "is_valid" to false
- Return null for missing fields
- Return only JSON, no commentary
```

**In Make.com**, add a validation step after parsing:
1. Add a **Filter** after Module 16 (JSON Parser)
2. Set condition: `{{16.is_valid}} === true`
3. Only insert to Sheets if is_valid = true
4. Optionally, send invalid receipts to a separate "Review" folder

---

### Example 5: Handwriting Confidence Score

**Use Case**: Flag receipts with unclear handwriting for manual review

**Modified Prompt**:
```
Act as a data extraction agent. Read the receipt image and extract:
{
"customer_name": "String",
"date": "YYYY-MM-DD",
"items": [
{
"item_name": "String",
"quantity": Number
}
],
"total_items_count": Number,
"handwriting_confidence": "high|medium|low"
}

Rules:
- Assess handwriting clarity:
  - "high": All fields are legible and unambiguous
  - "medium": Most fields clear, some guessing needed
  - "low": Difficult to read, low confidence in extracted data
- Always extract the data as best you can
- Use handwriting_confidence as a quality flag
- Return only JSON, no commentary
```

**In Make.com**, you can then:
1. Flag low-confidence receipts for manual review
2. Route them to a "Manual Review" sheet instead of auto-processing
3. Alert a team member to double-check

---

## How to Apply Custom Prompts

### Steps in Make.com

1. **Open the Scenario**
2. **Double-click Module 10** (Google Gemini AI – Generate a response)
3. Find the large **Text** box under Messages > Item 1 > Parts > Item 1
4. **Clear the existing prompt** and paste your new one
5. If you added new fields to the JSON, scroll to **Advanced Settings** and confirm:
   - **Response Format** is set to `application/json`
6. Click **Save**

### Update Your Google Sheet

1. Add new columns to your sheet for each new field
2. In Make.com Module 14 (Google Sheets), map each new field:
   - Open the module, scroll to **Values in columns**
   - Add new column mappings using the format `{{15.field_name}}` (if from Iterator) or `{{16.field_name}}` (if from JSON)
3. Save and test

### Test With a Sample Receipt

1. Upload a fresh receipt image to the watched folder
2. Click **"Run once"** in Make.com
3. Check for errors on any red circles
4. Verify the new data appears in Google Sheets

---

## Advanced: Handling Edge Cases

### Edge Case 1: Multi-Page Receipts

**Problem**: Receipt is split across 2 images

**Solution**: Modify the prompt to handle multiple images

```
Act as a data extraction agent. You may receive multiple receipt images (part 1, part 2).
Combine data from all images into a single JSON:
{
"customer_name": "String",
"date": "YYYY-MM-DD",
"items": [
{...}
],
"total_items_count": Number,
"note": "Multi-page receipt"
}

If data spans multiple images, consolidate into one unified receipt.
```

**Limitation**: Make.com's current flow processes one image at a time. To handle multi-page:
- Upload both images as a single PDF
- Use a PDF-to-image converter before sending to Gemini
- Or manually merge images before upload

---

### Edge Case 2: Foreign Language Receipts

**Problem**: Receipt written in Spanish, Arabic, or other language

**Current Solution**: Our prompt already transliterates and translates. To force a specific language:

```
...
Output all text in English only. If the receipt is in Spanish, translate product and customer names to English.
If the receipt is in Arabic, transliterate names to Roman script and translate descriptions to English.
...
```

---

### Edge Case 3: No Line Items (e.g., Lump Sum Payment)

**Problem**: Receipt shows total but no itemized breakdown

**Modified Prompt**:
```
...
If the receipt does NOT list individual items but only a total:
{
"customer_name": "String",
"date": "YYYY-MM-DD",
"items": [
{
"item_name": "Bulk payment (no itemization)",
"quantity": 1
}
],
"total_items_count": 1,
"note": "Lump sum payment, no itemization"
}

...
```

---

## Testing & Iteration

### Rapid Testing Workflow

1. Take a tricky receipt photo
2. Upload to Drive
3. Run scenario: `"Run once"`
4. Check Make.com for errors
5. If output is wrong:
   - Tweak the prompt
   - Delete the bad row from Google Sheets
   - Re-run
6. Repeat until satisfied

### Prompting Best Practices

- **Be explicit**: Instead of "extract items," say "extract item_name and quantity for each line item"
- **Give examples**: "For example, 'तुलसी की माला' translates to 'Tulsi garland'"
- **Set defaults**: "If a field is missing, return null (not empty string)"
- **Specify format**: "Date must be in YYYY-MM-DD format"
- **Limit scope**: "Only extract fields I asked for; ignore logos, watermarks, receipt numbers"

---

## Common Mistakes to Avoid

❌ **Too vague**: "Extract the receipt data"  
✅ **Clear**: "Extract customer_name, date, and items (item_name and quantity for each line)"

❌ **Too wordy**: Long rambling instructions  
✅ **Concise**: Bullet-pointed, specific rules

❌ **Flexible on format**: "If date is available, include it"  
✅ **Strict on format**: "Date must be YYYY-MM-DD; if missing, return null"

❌ **Contradictory**: "Return JSON. Also return a CSV."  
✅ **Single format**: "Return only JSON, nothing else"

---

## Rollback to Base Prompt

If a custom prompt breaks the scenario:

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

Paste this, save, and test with a known-good receipt image.

---

## Request a Custom Prompt

If you need help customizing the prompt for your specific use case, provide:

1. **Sample receipt image** (anonymized if needed)
2. **Fields you want to extract** (e.g., "invoice number, payment terms, shipping address")
3. **Output format** (JSON keys, data types)
4. **Any special rules** (e.g., "flag if total exceeds $1,000")

---

**Pro Tip**: Start with the base prompt, test it thoroughly, then add ONE custom field at a time. This makes debugging easy if something breaks.
