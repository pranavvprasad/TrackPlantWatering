# Plant Watering Tracker — Development History

## Project Overview

A plant watering/fertilization tracker app. Started as a static HTML prototype, now hosted on GitHub Pages with Firebase backend.

- **Repo:** https://github.com/pranavvprasad/TrackPlantWatering
- **Live URL:** https://pranavvprasad.github.io/TrackPlantWatering/
- **Firebase project:** plant-tracker-4d62b
- **Firebase Hosting URL (pending setup):** https://plant-tracker-4d62b.web.app

## Tech Stack

- Single static HTML file (`index.html`) — no build step, no server
- Firebase Realtime Database (data storage)
- Firebase Auth (Google sign-in + email/password)
- Hosted on GitHub Pages (also planning Firebase Hosting for masked URL)

## Feature History (chronological)

### Initial prototype
- Table with plants on vertical axis, dates on horizontal axis
- Click cell to cycle: empty → blue tick (watered) → red tick (fertilized) → empty
- Add/remove plants via modals
- Data stored in localStorage

### Date navigation
- Added nav bar with `«` `‹` Today `›` `»` buttons
- `‹`/`›` shift by 1 day, `«`/`»` shift by NUM_DAYS (full page)
- Can scroll arbitrarily into the past, not into the future
- Today's column always highlighted yellow

### Day of week display
- Column headers show weekday (Mon, Tue...) above the date (May 13)

### Firebase integration
- Replaced localStorage with Firebase Realtime Database
- Real-time sync — changes appear instantly across all open tabs
- Data stored at `/users/{uid}/plants` and `/users/{uid}/records`

### Plant editing
- Rename plants inline (migrates all existing records to new name)
- Reorder plants with up/down arrows
- Remove plants (deletes associated records)

### Mobile optimization
- Tighter padding, fixed table layout
- Plant name column wraps instead of truncating
- Larger tap targets on nav buttons
- Shows 5 days on mobile, 7 on desktop
- Table container max-width 700px on desktop, centered

### Confirmation for past dates
- Clicking a cell that isn't today shows a confirm dialog

### Authentication (multi-tenant)
- Google sign-in + email/password sign-in
- Each user gets isolated data under `/users/{uid}/`
- New users start with "Basil" and "Tomato" as default plants
- Firebase security rules: users can only read/write their own data
- No whitelist — any Google/email account can sign up

### Removed features
- Swipe gestures (removed — buttons work better)

## Firebase Database Structure

```
/users/{uid}/plants    → Array of strings, e.g. ["Basil", "Tomato"]
/users/{uid}/records   → Object, e.g. {"Basil|2026-05-13": "watered", "Tomato|2026-05-12": "fertilized"}
```

Record key format: `{plantName}|{YYYY-MM-DD}`
Record values: `"watered"` or `"fertilized"` (absence = no action)

## Firebase Security Rules

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "auth != null && auth.uid === $uid",
        ".write": "auth != null && auth.uid === $uid"
      }
    }
  }
}
```

## Firebase Console Setup Checklist

- [x] Realtime Database created
- [x] Google sign-in provider enabled (Authentication → Sign-in method)
- [x] Email/Password provider enabled
- [x] Authorized domains: `pranavvprasad.github.io`, `localhost`
- [ ] Firebase Hosting setup (pending — run `firebase init hosting` + `firebase deploy`)

## Key Decisions & Constraints

- **Data preservation is critical** — never delete/restructure Firebase data without migrating existing records (see CLAUDE.md)
- Firebase API key in HTML is safe — security enforced by database rules, not key secrecy
- No build step — everything is a single HTML file with CDN imports
- ES modules (`type="module"`) require `window.*` exports for onclick handlers in HTML
