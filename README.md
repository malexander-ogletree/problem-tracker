# Problem Tracker

A single-file, browser-based ITIL problem management tool. No server, no install — open `problem-tracker.html` directly in your browser and start tracking.

## Features

- **Problem records** — log problem tickets with title, owning group, status, priority, date created, current status narrative, and next steps
- **Cherwell integration** — configure a base URL with an `{id}` placeholder; each problem card links directly to its Cherwell ticket (per-problem overrides supported)
- **7-day incident signal** — log batches of linked incidents with dates; each card shows a sparkbar of incident volume over the trailing 7 days
- **Meeting notes** — timestamped, author-attributed notes per problem, expandable inline
- **Stat tiles** — dashboard header shows open problems, new incidents (7d), active owning groups, total active problems, and closed count
- **Filtering and sorting** — search by title, problem ID, or group; filter by status or group; sort by 7-day incidents, newest, priority, or status
- **Show/hide closed** — closed problems are hidden by default; toggle them in with one click
- **Dark/light theme** — respects system preference; toggle in the top bar; choice persisted
- **Export to Excel** — three-sheet workbook (Problems, Meeting Notes, Incident Log) via SheetJS
- **Backup and import**
  - Manual JSON backup download
  - Import from `.json`, `.xlsx`, `.xls`, or `.csv` (replace or merge)
  - Auto-backup to a local file via the File System Access API (Chrome/Edge); saves on a configurable interval (3, 5, 10, or 15 minutes); loads the backup file automatically on next open

## Data storage

All data is stored in `localStorage` under the key `problemTracker.v1`. If localStorage is unavailable (e.g. a sandboxed iframe), data is kept in memory for the session and a banner warns you. Use the JSON backup to keep a portable copy.

## Problem statuses

| Status | Color |
|--------|-------|
| Open | Amber |
| Investigating | Blue |
| Pending | Purple |
| Known Error | Dark grey |
| Resolved | Green |
| Closed | Light grey (hidden by default) |

## Priority levels

`Critical`, `High`, `Medium` (default), `Low`

## Getting started

1. Open `problem-tracker.html` in Chrome, Edge, Firefox, or Safari
2. Click **New problem** to add your first record
3. Optionally go to **Settings** and enter your Cherwell base URL (e.g. `https://cherwell.yourorg.com/CherwellClient/?Edit=Problem&PublicId={id}`)
4. Use **Settings > Auto-backup** (Chrome/Edge) or **Backup (.json)** to keep your data safe

## Import format

### JSON backup

A backup produced by this app can be re-imported as-is. The structure is:

```json
{
  "app": "problem-tracker",
  "version": 1,
  "exportedAt": "2025-01-01T00:00:00.000Z",
  "settings": { "base": "https://cherwell.yourorg.com/..." },
  "problems": [ ... ]
}
```

### Excel / CSV import

An exported workbook can be re-imported. Column names the importer reads:

**Problems sheet:** `Problem ID`, `Title`, `Owning Group`, `Status`, `Priority`, `Current Status`, `Next Steps`, `Cherwell Link`, `Created`

**Meeting Notes sheet:** `Problem ID` or `Problem Title`, `Date`, `Author`, `Note`

**Incident Log sheet:** `Problem ID` or `Problem Title`, `Incidents Linked`, `Date Linked`, `Note`

## Dependencies

Both are loaded from CDN and are not required for core functionality:

- [SheetJS (xlsx 0.18.5)](https://sheetjs.com/) — Excel export and spreadsheet import
- [Google Fonts](https://fonts.google.com/) — Space Grotesk, Inter, IBM Plex Mono (falls back to system fonts if offline)