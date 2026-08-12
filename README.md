# AgriStack Bandipora — Landholder Data Collection Portal

Aadhaar-linked landholder data collection for **Tehsil Sumbal, Bandipora district, Jammu & Kashmir**.

Ground-level workers (patwaris) log in to their assigned village page, look up landholder records from the AgriStack Jamabandi extract, and enter Aadhaar-linked details: Name, Aadhaar No., Parentage, Residence, and Mobile.

Admin/tehsildar accounts get a tehsil-wide dashboard showing per-village progress, worker contribution, and recent activity.

## Architecture

- **Frontend:** static HTML files — one per village + one admin dashboard + a landing page
- **Backend:** Google Apps Script Web App writing to a single Google Sheet (109 tabs, one per village)
- **Auth:** username/password, SHA-256 hashed with salt, 12-hour session tokens
- **Hosting:** GitHub → Render (static site, free tier)

## Files

| File | Purpose |
|---|---|
| `index.html` | Public landing page — links to admin dashboard |
| `admin.html` | Admin/tehsildar dashboard — tehsil-wide progress + per-village drill-down |
| `malikpora.html` | Village portal for Malikpora (Code 2811) |
| `<village>.html` | One per village (109 planned) |
| `robots.txt` | Prevent search engine indexing |

## Deployment

Frontend is hosted on Render as a static site — auto-deploys on every push to `main`. Backend runs on Google Apps Script and does not need redeployment on frontend changes.

## Adding a new village

1. Import the village's AgriStack extract as a new tab in the master Google Sheet (tab name: `VillageName_Code`, e.g. `Trigam_2812`)
2. Run `createPatwariAccount()` in Apps Script with that village's details
3. Duplicate `malikpora.html` → change `VILLAGE_NAME`, `VILLAGE_CODE`, and the embedded `RECORDS` JSON → save as `<villagename>.html`
4. Commit and push — Render auto-deploys within a minute

## Access

- Admin: `https://<your-render-url>/admin.html`
- Patwari for village X: `https://<your-render-url>/<villagename>.html`

Passwords are set from the Apps Script backend. Contact the tehsildar's office for access.

---

Office of the Tehsildar, Sumbal · Bandipora
