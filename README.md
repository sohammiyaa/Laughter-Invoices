# Laughter Enterprise — Invoice Generator (v2)

A single-file, GST-compliant tax invoice generator with PDF download, full edit/revision history, and audit trail.

## What's new in this version

- **True PDF download** (not just print-to-PDF). Real `.pdf` files via html2pdf.
- **Edit any saved invoice** with locked number and required revision reason.
- **Version history** preserved automatically — every revision keeps a snapshot of the previous version.
- **GST sequence protection** — duplicate numbers blocked, FY rollover smart, delete requires confirmation with compliance warning.
- **Search & filter** by client, service type, or financial year.
- **Quick PDF from history** — download any past invoice without opening it.

## How invoice numbering works (GST compliance)

Per GST law (Rule 46), invoice numbers must be:
1. Sequential (no gaps)
2. Unique (no duplicates)
3. Within a 16-character limit
4. Reset annually each financial year (April–March)

This app enforces all four:
- **Auto-numbered** in `LE/2026-27/001` format (12 characters, well within limit)
- **Sequence increments only after successful save** — no skipped numbers
- **Editing locks the number** — revisions update content but keep the original number
- **Duplicate detection** prevents accidentally reusing a number
- **FY rollover is automatic** — on April 1st, sequence detects new FY and starts fresh at 001
- **Delete warning** flags that gaps break compliance (intended for test data only)

## How edit / revision works

When you click the edit icon on any past invoice:
1. All fields load — but the **invoice number is locked**
2. A **revision reason is required** before saving (audit trail)
3. The old version is preserved in `revisionHistory` array
4. The version number increments (v1 → v2 → v3…)
5. The invoice shows a "Revised · v2" badge and revision note at the bottom

**Important note for your compliance work:** Under strict GST interpretation, the *legal* way to correct an already-issued invoice is via a credit note or debit note. The "edit" feature here is most appropriate for fixing typos *before* the invoice has been sent to the client or filed in GSTR-1. If the invoice has already been filed with the GST portal, you should issue a credit/debit note instead. The version history exists so you have a clear audit trail either way.

## How PDF download works

Two ways to get a PDF:
- **From the create page** — click "Download PDF" after filling the form (saves a real PDF file)
- **From history** — click the download icon on any saved invoice

Filename format: `Invoice_LE_2026-27_001_ClientName.pdf` (auto-sanitised)

The PDF is generated client-side using html2pdf.js (no server, no upload). Internet is needed only the first time to load the library — after that it works offline if the browser cached it.

## Deploy to a real URL (free)

### Easiest: Netlify Drop
1. Go to **https://app.netlify.com/drop**
2. Drag `index.html` into the drop zone
3. Get a live URL like `https://random-name.netlify.app`
4. Create a free account to make the URL permanent and customize it

### Best for updates: Vercel
1. Sign up at **https://vercel.com**
2. Install CLI: `npm i -g vercel`
3. From the folder containing `index.html`: run `vercel`
4. Re-running `vercel --prod` deploys updates

### Free forever: GitHub Pages
1. Create a public repo at **https://github.com/new** named `laughter-invoices`
2. Upload `index.html`
3. Settings → Pages → Source: `main` branch → Save
4. Your URL: `https://YOUR-USERNAME.github.io/laughter-invoices/`

## Data storage notes

Invoices live in your browser's `localStorage` on the device you used to create them. This means:
- ✅ Fast, private, no server, no account needed
- ✅ Works offline once loaded
- ❌ Not synced between devices (laptop ≠ phone)
- ❌ Clearing browser data wipes everything

**Critical habit:** Click "Export backup" in Settings every week and save the JSON file to Google Drive or email it to yourself. Import it on any other device or browser to restore.

When you're ready for true multi-device sync, the app would need a backend database — happy to upgrade with Firebase or Supabase (still free tier for your volume).

## Editing your business details

If you ever need to change Laughter Enterprise's bank account, GSTIN, etc., open `index.html` in any text editor and find this block near the top of the `<script>` section:

```js
const SELLER = {
  name: 'Laughter Enterprise',
  holder: 'Pranit Satyawan More',
  account: '003601554950',
  ifsc: 'ICIC0000036',
  branch: 'ICICI Bank, Prabhadevi branch',
  pan: 'BIOPM8405N',
  gstin: '27BIOPM8405N1ZJ',
  email: 'rjpranitofficial@gmail.com'
};
```

Edit the values, save the file, redeploy.

## Troubleshooting

**PDF generation fails?** Check internet connection (needed first time to load the PDF library). Try a different browser (Chrome works best).

**Signature not appearing in PDF?** It's embedded as base64 — should always work. If broken, the HTML file was edited or corrupted.

**Invoice number out of sync?** Settings → adjust "Next sequence number" → Save. Useful if you imported invoices from elsewhere.

**Wrong financial year detected?** The app uses your device's clock. Check your system date if numbering looks off in April.

---

Built for Laughter Enterprise · Pranit Satyawan More
