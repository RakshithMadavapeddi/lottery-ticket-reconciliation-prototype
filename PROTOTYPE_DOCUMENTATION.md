# Prototype Documentation

## 1. Overview

This prototype explores barcode-assisted lottery ticket reconciliation through two competing scanning flows.

The goal is not production readiness. The goal is to evaluate which interaction model gives users better clarity, confidence, and control while reconciling scratch-ticket inventory.

## 2. Primary comparison

### Flow 02 — Inline scanning

Flow 02 keeps scanning inside the counting screen using an overlay scan window. This direction keeps users inside the same context but mixes capture and review in one place.

### Flow 03 — Dedicated scanner + review

Flow 03 separates capture from verification.

Users move through:

1. dedicated scanner screen
2. structured review screen
3. uncounted / counted states
4. quantity edit modal when correction is needed
5. final summary screen

This flow is designed to make progress more visible and manual correction easier.

## 3. Files and responsibilities

### Entry and navigation

- `index.html` — redirects to the dashboard
- `dashboard.html` — opens Flow 02 or Flow 03 and resets stored state

### Flow 02 assets

- `flowTwo.html` — inline scanning flow
- `scanWindow.html` — isolated scan-window reference used for scanner UI exploration

### Flow 03 assets

- `flowThree.html` — counted / uncounted review screen
- `flowThreeScanner.html` — dedicated scanning interface
- `summary.html` — summary before completion

### Shared logic

- `app.js` — interaction logic, scanner setup, state updates, manual editing, summary generation
- `data.js` — seed ticket loading, CSV parsing, payload parsing, formatting helpers
- `styles.css` — shared styles for Flow 03 and scanner views

### Test assets

- `sampleBarcodes.pdf` — barcode samples used to test the scanner flows

## 4. User flow

### Flow 02

1. Open Flow 02 from the dashboard.
2. Tap scan.
3. Scan a barcode inside the inline overlay.
4. Matching ticket data is added or updated inside the current screen.
5. Open the summary screen.

### Flow 03

1. Open Flow 03 from the dashboard.
2. Start in the **Uncounted** tab.
3. Open the dedicated scanner.
4. Scan a barcode.
5. Return to the review list and verify updates.
6. Move between **Uncounted** and **Counted** through quantity completion.
7. Tap a quantity value to manually correct it.
8. Open the summary only after all tickets are counted.

## 5. Scanner behavior

The prototype uses the ZXing browser library and listens for **Data Matrix** codes.

### Supported behavior

- camera access
- duplicate-scan throttling
- match scanned values to ticket seed data
- update quantity and totals after scan
- optional torch control when supported by device/browser
- vibration and beep feedback after successful scan

### Flow differences

- **Flow 02** updates cards directly from the inline overlay.
- **Flow 03** updates ticket state, then reflects changes in the review list and summary.

## 6. Data model

Ticket data is seeded from CSV and normalized into a shared structure with fields such as:

- game ID
- bundle ID
- game title
- unit price
- units per bundle
- quantity
- total price
- last scanned code
- last updated timestamp

When the external CSV is unavailable, the prototype falls back to embedded data in `data.js`.

## 7. Barcode payload parsing

`data.js` parses barcode values into:

- game ID
- bundle ID
- ticket number
- quantity left
- scanned unit price

The parsed data is then matched against the seeded ticket list.

## 8. Manual fallback and correction

Flow 03 includes a quantity modal so users can correct or enter quantities manually.

This is important because the prototype is validating not only scanning, but also how the workflow supports recovery when scanning is incomplete, unavailable, or uncertain.

## 9. Summary behavior

The summary view aggregates counted items and shows total amount before completion.

Flow 03 intentionally blocks summary access until all tickets have quantities.

## 10. State and reset

Prototype state is stored in browser `localStorage`.

- `flowThree.ticketState.v1`
- `flowTwo.scannedItems.v1`

The dashboard reset action clears both keys so each session can start clean.

## 11. Running the prototype

Use a local server or GitHub Pages.

### Python

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`.

## 12. Limitations

This is a prototype, not production software.

Current limitations include:

- no backend persistence
- browser/device dependence for camera and torch support
- seeded test data only
- no authentication or role handling
- summary and ticket data are prototype-level only

## 13. Suggested GitHub Pages setup

Repository root can be published directly because the app already uses `index.html` as the entry point.

Recommended deployment target:

- branch: `main`
- folder: `/root`

## 14. Recommended future improvements

- add a clear test-mode banner
- add scan success and failure logs
- provide seeded scenarios for different stores
- add a review-state explanation for first-time testers
- export scanned session data for comparison across prototype runs
