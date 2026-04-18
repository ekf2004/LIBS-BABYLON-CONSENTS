# Babylon Consent Generator

Standalone browser-based tool that reads a Babylon procedure schedule (`.xlsx`) and generates pre-filled LIBS consent forms as a single combined PDF.

## For the user (scheduler)

1. Open the tool URL in a browser
2. Drag the Excel schedule file onto the page
3. Confirm the patient count looks right
4. Click **Make Consents** — a PDF downloads with one consent per patient, ready to email for printing

No install, no account, no internet required after the page loads. All Excel parsing and PDF generation happens locally in the browser — patient data never leaves the computer.

## How it works

- **Excel parsing** via SheetJS (CDN)
- **PDF stamping** via pdf-lib (CDN)
- **Blank consent template** embedded as base64 inside the HTML file
- Patient name, physician name, and full-English procedure text are stamped onto each page

## Procedure abbreviation translation

Excel shorthand is expanded to full consent-form English:

| Excel input | Consent output |
|---|---|
| `BL L3,L4,L5 RFA` | Bilateral L3, L4, L5 Medial Branch Radiofrequency Ablations |
| `BL L4-L5, L5-S1 TFESI` | Bilateral L4-L5 and L5-S1 Transforaminal Epidural Steroid Injections |
| `ML CAUDAL ESI W/GOI` | Midline Caudal Epidural Steroid Injection; Ganglion Impar Block |
| `BL C4,C5,C6 MBB BL PARACERVICAL TPI` | Bilateral C4, C5, C6 Medial Branch Blocks; Bilateral Paracervical Trigger Point Injection |

Full abbreviation dictionary is in the `PROC_NAMES` constant inside `index.html`.

## Expected Excel format

Built against the "COMPREHENSIVE PAIN CARE OF LONG ISLAND · PAIN MANAGEMENT PROCEDURE LOG SHEET" format:

- Header row contains `DOCTOR: DR. FANAEE` and `DATE m/d/yyyy`
- Each patient occupies 3–4 rows
- First row of block starts with a time cell (e.g., `0800AM`, `115PM`)
- Procedure text is in the second cell of the first row
- Patient name appears in LAST, FIRST all-caps format within the block

If the row layout changes, the parser may need adjustment — see `parseScheduleSheet()` inside the HTML.

## Deployment

This folder is served as-is by GitHub Pages. The URL will be:

```
https://<username>.github.io/libs-platform/babylon-consents/
```

No build step required.

## Future work

- Test against real Excel files (first production run)
- GSH booking sheet variant when that workflow becomes schedule-driven
- Integrate with `libs-proc-department.jsx` so the Excel intermediate goes away entirely
