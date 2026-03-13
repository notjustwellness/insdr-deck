# INSDR Waitlist — Google Sheets Setup

## Step 1: Create the Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com) and create a new spreadsheet
2. Name it **"INSDR Waitlist"**
3. In Row 1, add these headers:
   - A1: `Email`
   - B1: `Timestamp`
   - C1: `Source`

## Step 2: Add the Apps Script

1. In your Google Sheet, go to **Extensions → Apps Script**
2. Delete any code in the editor
3. Paste this entire script:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);

  sheet.appendRow([
    data.email,
    data.timestamp || new Date().toISOString(),
    data.source || 'waitlist'
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ status: 'success' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

4. Click **Save** (Ctrl+S / Cmd+S)
5. Name the project **"INSDR Waitlist"**

## Step 3: Deploy as Web App

1. Click **Deploy → New deployment**
2. Click the gear icon next to "Select type" → choose **Web app**
3. Set these options:
   - Description: `INSDR Waitlist`
   - Execute as: **Me**
   - Who has access: **Anyone**
4. Click **Deploy**
5. Click **Authorize access** → choose your Google account → Allow
6. **Copy the Web app URL** — it looks like: `https://script.google.com/macros/s/XXXX.../exec`

## Step 4: Add the URL to the Waitlist Page

1. Open `waitlist.html`
2. Find the line that says `YOUR_GOOGLE_SCRIPT_URL` (around line 290)
3. Replace it with your Web app URL
4. Commit and push

## That's it.

Every email signup will now appear as a new row in your Google Sheet in real-time. You can share the sheet with your partners so everyone can watch signups come in.

## Optional: Email Notifications

To get notified on each signup:
1. In Google Sheets, go to **Tools → Notification settings**
2. Set up email notifications for "Any changes are made"
