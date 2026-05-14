# Plant Watering Tracker

A simple web app to track when you water and fertilize your plants.

## Features

- Track watering and fertilizing per plant per day
- Add, rename, reorder, and remove plants
- Navigate to past dates
- Real-time sync across devices (Firebase)
- Google and email/password sign-in
- Mobile-friendly responsive design

## Usage

Visit the hosted URL, sign in, and click a cell to cycle through:
- Empty (no action)
- Blue checkmark (watered)
- Red checkmark (fertilized)

## Tech Stack

- Single static HTML file (no build step, no server)
- Firebase Realtime Database (persistence + real-time sync)
- Firebase Authentication (Google sign-in + email/password)

## Hosting

The app is deployed via **Cloudflare Pages**, which connects directly to this GitHub repo and auto-deploys on every push to `main`.

- **Build command:** none (static file)
- **Build output directory:** `/`
- **Production branch:** `main`

Firebase handles authentication and data storage. The Firebase API key in `index.html` is safe to be public — security is enforced by database rules, not key secrecy.

