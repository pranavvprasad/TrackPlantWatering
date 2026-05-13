# Plant Watering Tracker

## Overview

Single-file static HTML app hosted on GitHub Pages. Uses Firebase Realtime Database for shared persistent storage (no backend server).

- **Live URL:** https://pranavvprasad.github.io/TrackPlantWatering/
- **Main file:** `index.html`

## Critical: Data Preservation

**Never delete, overwrite, or restructure Firebase data without migrating existing records.**

When modifying code that touches the database:
- Renaming a plant? Move all its records to the new key first.
- Changing key format? Write a migration that copies data to the new structure before removing old keys.
- Adding new fields? Use defaults so existing data continues to render correctly.
- Removing a feature? Leave its data in place unless explicitly asked to clean up.

## Firebase Schema

```
/plants          — Array of plant name strings, e.g. ["Basil", "Tomato"]
/records/{key}   — Value is "watered" or "fertilized"
                   Key format: "{plantName}|{YYYY-MM-DD}"
                   e.g. "Basil|2026-05-13" → "watered"
```

Database rules: open read/write (no auth required).
