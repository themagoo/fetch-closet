# Fetch Closet

A single-file web app that turns a folder of unsorted clothing photos into a ready-to-import **Poshmark bulk upload** spreadsheet. Built as a prototype — open the HTML file directly in a browser, no build step.

## What it does

Walks the user through three steps:

1. **Photos.** Pick a folder of clothing photos from your computer. **MVP model: one photo per garment, no clustering.** Each photo is renamed into the `clothing_upload_<YYYYMMDD>_<###>_front.<ext>` format and dropped into a dated subfolder.
2. **Research.** For each garment, the app would run reverse-image search, identify brand/product, pull the MSRP, benchmark sold listings on Poshmark / Mercari / eBay, and draft a Poshmark-style description with measurement placeholders and hashtags. In this prototype the research is mocked — fields start empty and flagged, ready for manual entry. Editing a field clears its flag.
3. **Export.** Live preview of the populated `Listing Details` tab from the Poshmark bulk upload template — rows start at row 3, all required fields populated, and the template's hidden taxonomy/color/size dropdown tabs are preserved untouched. Save as `<YYYYMMDD>/bulk_upload_<YYYYMMDD>.xlsx`.

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
