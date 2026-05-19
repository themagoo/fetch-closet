# Poshmark Bulk Listing Generator — Project Prompt

## Overview

Build an app that automates the creation of high-quality Poshmark listings from a folder of clothing photos. The app ingests raw photos, groups them by garment, researches each item online, generates listing copy, and outputs a completed Poshmark **bulk upload `.xlsx`** file ready for import.

The output must conform to the structure of the supplied `sample bulk upload template.xlsx`, specifically the **`Listing Details`** tab (data begins at row 3; rows 1–2 are headers/instructions).

---

## End-to-End Workflow

### Step 1 — Photo Ingestion & Grouping

**Input:** A folder of unsorted photos of various clothing pieces.

**Tasks:**
- Read every image in the input folder.
- Cluster photos that belong to the **same unique garment** (by visual similarity, garment color, pattern, background, EXIF timestamp proximity, etc.).
- For each cluster, classify each photo into one of the following **suffix types**:
  - `front` — full front view of the garment
  - `back` — full back view
  - `label` — the inside size/care label (used later to read size)
  - `closeup` — fabric or stitching detail
  - `callout` — a flaw, tag, sticker, or other notable detail (e.g. paper hang tag indicating NWT)
- Rename and save each image using the pattern:

  ```
  clothing_upload_<YYYYMMDD>_<###>_<suffix>.<ext>
  ```

  Example: `clothing_upload_20260518_001_front.jpg`

- The `###` index is the unique garment number (zero-padded to 3 digits), assigned in sequence.
- Place each garment's renamed photos into its own subfolder named `clothing_upload_<YYYYMMDD>_<###>/`.

### Step 2 — Web Research & Listing Generation

For each garment group, use the `front` image as the primary search input.

**Research targets:**
1. **Stock photo URL** — a direct image URL to the manufacturer's or retailer's product photo (use reverse-image search; prefer high-resolution, watermark-free URLs).
2. **Brand and product name** — e.g. `Free People Carpe Diem Shorts`.
3. **MSRP** — original retail price in USD.
4. **Recommended sale price** — based on comparable sold listings on Poshmark, Mercari, eBay, etc.

**Listing copy:** generate a Poshmark-style description that includes:
- Title line (Brand + Product Name + key attribute, e.g. color and size).
- A 1–2 paragraph product description written in a warm, sales-friendly tone.
- Measurement placeholders (chest, waist, inseam, length, etc.) for the user to fill in.
- The original retail price called out in the closing line.
- A trailing block of hashtags and keywords relevant to the brand, style, and category.

**Output of this step:** a user-friendly, in-app table where each row represents one garment and each column represents a researched / generated field. The user should be able to review and edit any field before publishing to the spreadsheet.

### Step 3 — Populate the Bulk Upload Template

Open `sample bulk upload template.xlsx` → **`Listing Details`** tab. Write each garment as a new row **starting at row 3**.

| Column | Source / Logic |
|---|---|
| **SKU** | Auto-generate: first letter of each word in the brand name (uppercase) + zero-padded 4-digit sequence number per brand. E.g. `Free People` → `FP0001`, `FP0002`, … `Nike` → `N0001`. |
| **Title** | `Brand + Product Name`. If any photo in the group has a visible paper hang tag indicating new-with-tags, prepend `NWT ` to the title. |
| **Description** | The generated Poshmark description from Step 2. |
| **Department** | Choose from the dropdown on the `Dept Cat Subcat Taxonomy` tab. Valid values: `Women`, `Men`, `Kids`, `Home`, `Electronics`, `Pets`. |
| **Category** | Choose the row on the `Dept Cat Subcat Taxonomy` tab whose Department matches the chosen Department and whose Category best fits the garment. |
| **Sub-category** | Optional — pick a Sub-category from the same taxonomy tab if a clear match exists. |
| **Quantity** | Default to `1`. |
| **Size** | Read the `label` photo for the garment's size; select the matching value from the size dropdown (`Size Taxonomy` tab). If no label photo exists, leave blank for user review. |
| **Condition** | If the garment is NWT, set `NWT`. Otherwise visually evaluate the photos and pick one of: `Like New`, `Good`, `Fair`. |
| **Brand** | The brand string from research. |
| **Color1** | The dominant color, picked from the `Color Mapping` tab: `Red, Pink, Orange, Yellow, Green, Blue, Purple, Gold, Silver, Black, Gray, White, Cream, Brown, Tan`. |
| **Color2** | If the garment has a clear secondary color, pick from the same list. Otherwise leave blank. |
| **Original Price** | MSRP from research (numeric, no `$`). |
| **Listing Price** | Recommended sale price from research (numeric, no `$`). |
| **Availability** | Default to `Not For Sale`. |
| **Primary image** | The stock photo URL from Step 2. |
| **Alt image 1–15** | Optional — leave blank in v1; future enhancement can upload the user's own renamed photos to a cloud host and populate these URLs. |

All other columns (`ProductID`, `VariantGroupID`, `Style Tag1–3`, `Shipping Discount`, `Price Floor`, `Minimum Price`, `Drop time`, `Other info`, `Copy Listing?`, `Update Existing SKU?`, `NEW SKU`) should be left blank by default unless researched data clearly maps to them.

### Step 4 — Save & Preview

- Save the populated workbook as `bulk_upload_<YYYYMMDD>.xlsx` inside a new folder named `<YYYYMMDD>/` (today's date).
- Preserve the original tabs, formulas, dropdown validations, and formatting from the template — only the `Listing Details` tab should be modified.
- Surface a preview of the final file in the app UI (table view of the populated rows) and provide a download / open link to the saved `.xlsx`.

---

## Functional Requirements

- **Idempotent runs.** Re-running on the same input folder should not duplicate rows; detect existing garment folders by name and skip or update.
- **Manual override at every step.** The user must be able to: (a) regroup mis-clustered photos, (b) edit any researched field, (c) edit the generated description before export.
- **Brand-scoped SKU counter.** Persist the per-brand sequence counter across runs so future uploads continue numbering correctly.
- **Confidence indicators.** Flag any field where the research result was low-confidence (e.g. no good stock photo found, ambiguous brand match) so the user can review.
- **Failure modes.** If a garment cannot be identified online, still create the row with a blank Primary image and a `[NEEDS REVIEW]` prefix on the Title.

## Non-Functional Requirements

- **Inputs:** local folder of `.jpg`/`.jpeg`/`.png`/`.heic` photos.
- **Outputs:** renamed/sorted photo subfolders + final `.xlsx` + in-app preview table.
- **Tech-stack agnostic** at the prompt level — implementation choice (web app, desktop app, CLI + UI) is up to the build phase.

---

## Reference Files

- `sample bulk upload template.xlsx` — the master template; do not alter its hidden tabs (`Dept Cat Subcat Taxonomy`, `Color Mapping`, `Size Taxonomy`, `Options`, `Category`, etc.) — they drive the dropdown validations.
- Tab to populate: **`Listing Details`** (data rows start at row 3).
- Tab to read for departments/categories: **`Dept Cat Subcat Taxonomy`**.
- Tab to read for colors: **`Color Mapping`**.
- Tab to read for sizes: **`Size Taxonomy`** (and `Size Lookup` / `Size Options` for UI dropdowns).

## Acceptance Criteria

1. Given a folder of mixed clothing photos, the app produces correctly grouped and renamed photo subfolders.
2. Each unique garment yields exactly one row in the `Listing Details` tab, starting at row 3.
3. SKUs follow the `<brand-initials><4-digit-seq>` rule and are unique within a brand.
4. All required Poshmark fields (SKU, Title, Description, Department, Category, Quantity, Size, Condition, Brand, Color1, Original Price, Listing Price, Availability, Primary image) are populated for every row, or flagged for review.
5. The final `.xlsx` opens cleanly in Excel/Numbers with the template's original dropdown validations still intact.
6. The user can preview the populated table inside the app before exporting.
