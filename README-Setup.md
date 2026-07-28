# Sofa Shine — Job Manager (Mobile Web App)

A single-file mobile web app for managing sofa-cleaning jobs: New Order → Draft → Completed, with before/after photos, an auto-generated PDF gallery, and printable receipts. Classical, light "royal" theme — ivory, maroon and gold, with the New Order / Drafts / Completed tabs fixed at the **top** so they never sit over your Save button or the keyboard. Works offline as a home-screen app.

## Files in this package
- **index.html** — the entire app (open this in a browser / host it on GitHub Pages)
- **Code.gs** — optional Google Apps Script backend for syncing orders to your Google Sheet
- **README-Setup.md** — this file

---

## 1. Host it on GitHub Pages (free, works on any phone)

1. Create a new GitHub repository, e.g. `sofa-shine-app`.
2. Upload `index.html` to the repo root.
3. Go to **Settings → Pages**, set Source to `main` branch, root folder. Save.
4. GitHub gives you a URL like `https://yourusername.github.io/sofa-shine-app/` — that's your app.
5. On a phone, open that link in Chrome/Safari → menu → **Add to Home Screen**. It now behaves like a native app icon.

No build step, no server. All order data and photos are stored securely on the device itself (IndexedDB), so the app keeps working with no internet connection.

## 2. Connect it to your Google Sheet

Because this is a static app with no server of its own, it talks to Google Sheets through a small **Google Apps Script** that acts as a bridge. Once connected, every order is written as a row in your Sheet and every photo is uploaded to a Drive folder automatically.

1. Open your Google Sheet (or create a new one at sheets.google.com).
2. In the app, tap the ⚙️ **Settings** icon → paste your Sheet's normal browser link into **"Your Google Sheet Link"**. The app instantly pulls out the **Sheet ID** and shows a **Copy** button — you don't need to hunt through the URL yourself.
3. In your Sheet, click **Extensions → Apps Script**. Delete the placeholder code and paste in the entire contents of `Code.gs` from this package.
4. Replace `PASTE_YOUR_GOOGLE_SHEET_ID_HERE` at the top of the script with the ID you copied.
5. Click **Deploy → New deployment**. Choose type **Web app**.
   - Execute as: **Me**
   - Who has access: **Anyone**
6. Click **Deploy**, approve the Google permission prompts (it needs access to your Sheet and Drive), then copy the resulting **Web app URL** (ends in `/exec`).
7. Back in the app's Settings, paste that URL into **"Apps Script Sync URL"**, tap **Test Connection** to confirm it's reachable, then **Save Settings**.

From then on, every order saved in the app is pushed to your Sheet in the background, with clickable Drive links to each before/after photo. If there's no internet at the time, the order still saves fine on the phone — only the Sheet copy is skipped for that entry, and will catch up next time you save something with a connection.

## 3. How the app works

Tabs for **New Order / Drafts / Completed** sit at the top of the screen at all times, so scrolling down to hit Save never gets blocked by a floating bottom bar.

| Dashboard | What it does |
|---|---|
| **New Order** | A quick stats strip (pending drafts, jobs completed today, revenue this month) sits above the form. Enter customer name, mobile, area, sofa type, seats & price (auto-calculates total), and at least 1 "before" photo. Saves as a Draft. |
| **Drafts** | Lists all unfinished jobs by date/name/area. Tap one to see the pre-filled details, add "after" photos, and mark it Completed. |
| **Completed** | Lists finished jobs with date/name/area/amount. Tap one to open a full **Receipt** — customer details, mobile, before & after photos, and grand total — with buttons to download as PDF, share on WhatsApp, or print. |

**Adding a photo:** tap "+ Add Photo" and you're given two clear options — **Take Photo** (opens the camera) or **Choose from Gallery** (opens your photo library) — instead of the browser guessing which one you want.

**Viewing a photo:** tap any photo thumbnail anywhere in the app (new order, drafts, or a receipt) to open it full-screen, with left/right arrows if there's more than one.

## 4. Automatic photo catalog PDF

Every time a job is marked **Completed**, the app silently rebuilds a single PDF and downloads it straight to the device — no popup, no dialog box. The PDF now opens with a proper **cover page**: the Sofa Shine crest, generation date, and an **index listing every customer's name, area, and page number** — so it reads like a printed booklet rather than a blank title screen. Each customer then gets their own page headed by their name, with sofa type/area/date and Before/After photos side by side. You can also trigger it manually anytime from the **Completed** tab's "Download Before/After Catalog" button — this is the file you hand to prospective customers as a portfolio.

## 5. Extra ideas built in

- **Quick stats strip** — pending drafts, jobs completed today, and this month's revenue, visible the moment you open the app.
- **Camera vs Gallery choice** on every photo upload, and a **full-screen photo viewer** with swipe-through navigation.
- **One-tap Sheet ID extraction** from any Google Sheets link, with copy button, plus a **Test Connection** check in Settings.
- **Auto receipt numbers** (e.g. `SS-2026-0001`) generated in sequence.
- **WhatsApp share** button on every receipt — sends a formatted summary straight to the customer's number (or your business number, configurable in Settings).
- **Smart price suggestions** — picking a sofa type auto-fills a typical seater price, which you can still override per job.
- **Search** on both Drafts and Completed lists (by customer name or area).
- **Photo compression** — photos are automatically resized/compressed on capture so the app stays fast and doesn't run out of storage even with many photos per job.
- **Print Receipt** — a clean printable version, in case a customer wants a physical copy instead of PDF/WhatsApp.
- **Settings → Erase All Local Data** — a safe reset option if you ever need to wipe the device's stored jobs.

## 6. Notes & limits

- Photos and orders are stored per-device (IndexedDB). If you use the app from two different phones, connect the Google Sheet sync (step 2) so a copy of every order is centralized — the phones themselves won't automatically see each other's jobs.
- A Google Sheet link alone can't be written to directly from a static web page — Google requires the one-time Apps Script bridge in step 2 for security. Once set up, it's fully automatic from then on.
- Everything runs entirely in the browser — no backend hosting costs beyond the free GitHub Pages and free Google Apps Script quota.
