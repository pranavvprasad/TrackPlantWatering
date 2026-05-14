# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture

Single-file static HTML app (`index.html`) — no build step, no bundler, no server. All CSS, HTML, and JS live in one file. JavaScript uses ES modules via CDN `<script type="module">`.

**Stack:**
- Firebase Realtime Database (persistence + real-time sync)
- Firebase Auth (Google sign-in + email/password)
- GitHub Pages (hosting)
- Firebase Hosting (alternate masked URL, pending setup)

**Because `type="module"` scopes all functions**, any function called from HTML `onclick` attributes must be explicitly exported to `window` (e.g., `window.signIn = signIn`).

## Running Locally

Firebase Auth rejects `file:///` origins. To test locally:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`. Ensure `localhost` is in Firebase Console → Authentication → Authorized domains.

## Firebase Schema (Multi-Tenant)

```
/users/{uid}/plants    → Array of strings, e.g. ["Basil", "Tomato"]
/users/{uid}/records   → Object with keys "{plantName}|{YYYY-MM-DD}"
                         Values: "watered" or "fertilized"
```

New users are automatically initialized with `["Basil", "Tomato"]`.

Security rules enforce that each user can only read/write their own `/users/{uid}/` subtree.

## Critical: Data Preservation

**Never delete, overwrite, or restructure Firebase data without migrating existing records.**

- Renaming a plant → copy all its records to the new key before removing old keys.
- Changing key format → write a migration that moves data to the new structure.
- Adding new fields → use defaults so existing data renders correctly.
- Removing a feature → leave its data in place unless explicitly asked to clean up.

## Deployment

Push to `main` branch → GitHub Pages auto-deploys within ~60 seconds.

Live URL: `https://pranavvprasad.github.io/TrackPlantWatering/`

## Responsive Behavior

- Desktop (≥768px): shows 7 days, table max-width 700px centered
- Mobile (<768px): shows 5 days, full-width table with compact padding
- `«`/`»` buttons shift by NUM_DAYS (full page), `‹`/`›` shift by 1 day
