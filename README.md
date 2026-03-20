# Lottery Ticket Reconciliation Prototype

A lightweight HTML, CSS, and JavaScript prototype for comparing two scratch-ticket scanning workflows inside a lottery reconciliation flow.

## What this prototype does

This prototype was built to evaluate two directions for ticket scanning inside a reconciliation experience:

- **Flow 02** keeps scanning inside the counting screen as an inline overlay.
- **Flow 03** separates scanning into a dedicated camera screen, then returns users to a structured review flow with **Uncounted / Counted** states, manual quantity editing, and a final summary.

The prototype is intentionally small and front-end only. It is designed for side-by-side workflow comparison, device testing, and barcode-based interaction validation.

## Included screens

- `index.html` — redirects to the dashboard
- `dashboard.html` — launch point for Flow 02 and Flow 03, with reset action
- `flowTwo.html` — inline overlay scanning flow
- `flowThree.html` — review screen with Uncounted / Counted tabs
- `flowThreeScanner.html` — dedicated scanner screen
- `summary.html` — final summary view
- `ticketCard.html` — isolated ticket-card reference
- `scanWindow.html` — isolated scanner-window reference

## Tech stack

- HTML
- CSS
- Vanilla JavaScript
- ZXing browser library for Data Matrix scanning
- `localStorage` for prototype state persistence

## How to run

Because camera access usually requires a local or secure origin, run the prototype with a local server instead of opening the files directly.

### Option 1: Python

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

### Option 2: VS Code Live Server

Open the folder and run it with Live Server.

## Prototype behavior

### Dashboard

The dashboard lets reviewers:

- open **Flow 02**
- open **Flow 03**
- reset prototype state for both flows

### Flow 02

Flow 02 keeps scanning in context. Users scan from the counting screen, and the scan result updates cards directly.

### Flow 03

Flow 03 separates the interaction into clearer steps:

1. open scanner
2. scan ticket
3. return to review list
4. move items between **Uncounted** and **Counted** states
5. manually edit quantity when needed
6. continue to summary only after all tickets are counted

## Test data

Ticket seed data is loaded from `assets/InstantScratchTickets_Data - Sheet1.csv` when available and falls back to embedded CSV data in `data.js`.

Sample barcode values are included in `sampleBarcodes.pdf` for scanner testing.

## Barcode testing notes

- Use the sample barcode PDF on a second screen or printed copy.
- Allow camera access when prompted.
- On supported devices, the dedicated scanner can toggle the torch.
- If your browser blocks camera access from `file://`, use a local server or GitHub Pages.

## State storage

The prototype uses browser `localStorage`.

- `flowThree.ticketState.v1`
- `flowTwo.scannedItems.v1`

Use the dashboard reset button to clear saved prototype progress.

## Project goal

This repository documents a working prototype used to compare scanning approaches and refine the final reconciliation workflow around clarity, confidence, and manual correction.
