# Setup Guide

This connects the site to a Google Sheet acting as your database, via a Google Apps Script Web App.

## 1. Create the Google Sheet

1. Go to [sheets.new](https://sheets.new) to create a blank spreadsheet. Name it something like **Merida YSA Newsroom Data**.
2. Create two tabs (bottom-left `+`), named exactly:
   - `Activities`
   - `Users`
3. In the **Activities** tab, add this header row (row 1):

   | id | title | date | time | category | location | description |
   |----|-------|------|------|----------|----------|-------------|

   Leave the rest of the sheet empty — the app fills it in.

4. In the **Users** tab, add this header row (row 1):

   | username | passwordHash | salt | displayName |
   |----------|--------------|------|-------------|

   You'll fill in one row per leader in step 4 below.

## 2. Add the script

1. In the Sheet, go to **Extensions → Apps Script**.
2. Delete any starter code in `Code.gs`, then paste in the contents of this repo's [`Code.gs`](./Code.gs).
3. Click the **Save** icon (or `Ctrl+S` / `Cmd+S`).

## 3. Deploy as a Web App

1. In the Apps Script editor, click **Deploy → New deployment**.
2. Click the gear icon next to "Select type" and choose **Web app**.
3. Set:
   - **Execute as:** Me
   - **Who has access:** Anyone
4. Click **Deploy**, and authorize the script when prompted (click through the "Google hasn't verified this app" warning — it's your own script).
5. Copy the **Web app URL** it gives you (ends in `/exec`). You'll need it in step 5.

> Whenever you edit `Code.gs` later, use **Deploy → Manage deployments → Edit (pencil icon) → New version** to publish the changes — just saving isn't enough.

## 4. Create your first leader account

1. Back in the Apps Script editor, open `Code.gs`.
2. Find the `generateUserRow()` function near the bottom and edit the three placeholder values:
   ```js
   const username = 'jdoe';
   const password = 'a-strong-password-here';
   const displayName = 'Jane Doe';
   ```
3. In the toolbar, select `generateUserRow` from the function dropdown, then click **Run**.
4. Open **View → Logs** (or `Ctrl+Enter`) — it prints a row of values.
5. Copy those four values into a new row in the **Users** tab (username, passwordHash, salt, displayName).
6. Repeat for each additional leader, using a different username/password each time.

> Passwords are never stored in plain text — only a salted SHA-256 hash. Leaders can change their own password later from the **Change Password** button in the Leader Panel.

## 5. Point the site at your Web App

1. Open `index.html` in this repo, find `API_URL: 'PASTE_YOUR_APPS_SCRIPT_WEB_APP_URL_HERE'` near the bottom, and replace it with the URL from step 3.
2. Do the same in `leaders.html`.
3. Save both files.

## 6. Publish with GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, pick `main` and `/ (root)`, then **Save**.
4. GitHub gives you a URL like `https://yourname.github.io/repo-name/` — that's your public site. Leaders go to the same URL + `/leaders.html`.

## Troubleshooting

- **"Apps Script URL isn't set yet"** — you missed step 5 in one of the two HTML files.
- **Activities aren't loading** — make sure the deployment's access is set to "Anyone," and that you created a *new version* after any edits (see the note in step 3).
- **Login fails for a real leader** — double-check the row in `Users` has exactly 4 columns in order (username, passwordHash, salt, displayName) and no extra spaces in the username.
- **Sessions log out too fast/slow** — adjust `TOKEN_TTL_SECONDS` at the top of `Code.gs` (default 6 hours) and redeploy.
