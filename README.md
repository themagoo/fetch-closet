# Fetch Closet

A single-file web app that turns a folder of unsorted clothing photos into a ready-to-import **Poshmark bulk upload** spreadsheet. Built as a prototype — open the HTML file directly in a browser, no build step.

## What it does

Walks the user through four steps:

1. **Photos.** Drop a folder of raw clothing photos. The app clusters them into individual garments by visual similarity, color, pattern, and EXIF timestamp, then classifies each shot as `front`, `back`, `label`, `closeup`, or `callout`.
2. **Groups.** Review each garment as a card with renamed filenames in the `clothing_upload_<YYYYMMDD>_<###>_<suffix>.jpg` format. Reassign shot types or regroup mis-clustered photos.
3. **Research.** For each garment, the app runs reverse-image search, identifies brand/product, pulls the MSRP, benchmarks sold listings on Poshmark / Mercari / eBay, and drafts a Poshmark-style description with measurement placeholders and hashtags. Edit anything inline; low-confidence fields are highlighted.
4. **Export.** Live preview of the populated `Listing Details` tab from the Poshmark bulk upload template — rows start at row 3, all required fields populated, and the template's hidden taxonomy/color/size dropdown tabs are preserved untouched. Save as `<YYYYMMDD>/bulk_upload_<YYYYMMDD>.xlsx`.

## Running it

Open `Fetch Closet.html` in any modern browser. The app is self-contained — React 18, ReactDOM, and Babel-standalone load from CDN; everything else is inline.

## Files

| File | Purpose |
|---|---|
| `Fetch Closet.html` | The app — single self-contained file with inline CSS and JSX |
| `PROJECT_PROMPT.md` | Product spec the app was built against |
| `Starting Prompt-Fetch.md` | Original brief / scratchpad |
| `sample bulk upload template.xlsx` | Reference Poshmark bulk upload template (column layout, taxonomy tabs, dropdown validations) |

## Design

The visual design uses three lockable settings from the source design package:

- **Mood: Vault** — dark archive palette, charcoal backgrounds, warm amber accents.
- **Density: Workhorse** — tight rhythm, sans-serif headers, compact tables — built to plow through 200+ garments per session.
- **Photo style: Silhouette** — line-drawn garment SVGs on flat color blocks for stand-in photos.

## Status

Prototype — all garment data, research, and clustering are mocked in-memory. The data layer (`INITIAL_GARMENTS`) drives every screen and can be swapped out for real ingestion + research backends without changing the UI.
