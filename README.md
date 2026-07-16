# Merida YSA Newsroom

A lightweight activity calendar and photo memory-lane for the Merida YSA branch (Ormoc Stake) — with a password-protected leader panel for publishing activities. No build step, no server to maintain: static pages + a Google Sheet as the database, deployable for free on GitHub Pages.

## Features

**Public site (`index.html`)**
- Upcoming Activities calendar, sorted automatically by date
- "Next up" hero card that always surfaces the soonest activity
- Search and category filters (Devotional, Fellowship, Service, Seminar, Sports)
- Countdown badges ("Today", "Tomorrow", "In 5 days…")
- One-tap "Add to phone calendar" (.ics download) on every activity
- Gallery tab with a lightbox for past-event photos
- Skeleton loading state, offline/error fallback to sample data

**Leader Panel (`leaders.html`)**
- Username + password sign-in (bcrypt-style salted SHA-256 hashes, not shared secrets)
- Publish, edit, and delete activities
- Self-service **Change Password**
- Search and filter the activity list while managing it
- Session persists for the browser tab via a short-lived server token

**Backend (`Code.gs`)**
- A single Google Apps Script Web App, no separate hosting needed
- Reads/writes a Google Sheet as the activities + leader-accounts database
- Session tokens expire automatically (6 hours)

## Tech stack

- [Tailwind CSS](https://tailwindcss.com/) (CDN build, no bundler)
- [Alpine.js](https://alpinejs.dev/) for interactivity
- [Font Awesome](https://fontawesome.com/) for icons
- Google Apps Script + Google Sheets for the backend

## Project structure

```
.
├── index.html      # Public calendar + gallery
├── leaders.html     # Leader sign-in and activity management panel
├── Code.gs           # Google Apps Script backend (deploy this to your Sheet)
├── favicon.svg
├── SETUP.md          # Step-by-step deployment guide
└── README.md
```

## Quick start

1. Follow [SETUP.md](./SETUP.md) to create the Google Sheet, add the `Code.gs` script, and deploy it as a Web App.
2. Paste your deployed Web App URL into the `API_URL` constant near the bottom of **both** `index.html` and `leaders.html`.
3. Push this repo to GitHub and enable **GitHub Pages** (Settings → Pages → Deploy from branch → `main` / root).
4. Share the Pages URL with your branch — leaders sign in at `/leaders.html`.

## License

MIT — see [LICENSE](./LICENSE).
