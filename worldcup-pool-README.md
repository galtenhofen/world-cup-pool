# World Cup Pool 2026

A single-file web app for running a World Cup pool with friends. No backend, no database, no monthly fees.

## Quick Setup

### 1. Change the Admin Password
Open `index.html` and find line ~510:
```js
const ADMIN_PASSWORD = 'worldcup2026'; // ← CHANGE THIS
```
Change it to whatever you want.

### 2. Update the Teams (if needed)
The 48 teams and tier assignments are near the top of the `<script>` section. Edit the `TEAMS` object to match the actual 2026 World Cup roster.

### 3. Update the Groups
The `GROUPS` object maps group letters (A–L) to team IDs. Update these once the official draw is made.

## Free Deployment Options

### Option A: GitHub Pages (Recommended)
1. Create a free GitHub account
2. New repository → upload `index.html` → commit
3. Go to Settings → Pages → Source: main branch, root `/`
4. Your URL: `https://yourusername.github.io/reponame`

### Option B: Netlify Drop
1. Go to https://app.netlify.com/drop
2. Drag and drop the `index.html` file
3. Get a free URL instantly (e.g. `something.netlify.app`)
4. Optional: rename the site under Site Settings

### Option C: Cloudflare Pages
Similar to Netlify, also free and very fast CDN delivery worldwide.

## How Data Works

All data is stored in the browser's `localStorage` on the device where you're running admin. This means:

- **Participants enter picks** on their own devices — their data lives in THEIR browser
- **Admin** manages scores from YOUR device (the admin device)
- Each device has its own localStorage — they don't sync automatically

### Sharing Data Across Devices

Since there's no backend, use the **Admin → Manage Picks → Export Data** button to:
1. Export a JSON file from one device
2. Import it on another device via the Import section

**Practical workflow for a friend pool:**
- Have everyone enter their picks on the site (you'll collect them manually or have them screenshot)
- **You** enter all the picks via the Admin panel on your device
- You update scores on your device
- Share the URL and everyone can view the leaderboard from their phones

Or — for a more seamless experience — look into adding a free backend (see below).

## Adding a Backend (Optional)

If you want real-time sync across all devices, you can add a free backend later without rewriting the app. Options:
- **Supabase** (free tier): replace the `S.get/S.set` functions with Supabase calls
- **Firebase Realtime Database**: Google's free tier is generous for small pools
- **JSONbin.io**: Simple REST storage, free tier available

The app is architected so you'd only need to change the `getData()` and `saveData()` functions.

## Adding New Pages

The app uses a simple router. To add a new page:

1. Add a nav button in the `#nav` section:
```html
<button class="nav-btn" onclick="navigate('mypage')">My Page</button>
```

2. Add the page name to the pages array in `navigate()`:
```js
const pages = ['home','picks','leaderboard','matches','popularity','rules','admin','mypage'];
```

3. Add the page function:
```js
function mypage(el) {
  el.innerHTML = `<h2 class="page-title">My Page</h2><div class="card">...</div>`;
}
```

4. Add it to the pages object in `render()`:
```js
const pages = { home, picks, leaderboard, matches, popularity, rules, admin, mypage };
```

That's it. Nothing else breaks.

## Admin Guide

### Entering Group Scores
- Go to Admin → Group Matches
- Enter home and away scores for each match
- Leave blank if not played yet
- Click "Save All Group Scores"

### Marking Advancement
- Go to Admin → Advancement
- Check "Advanced" for teams that made the knockout round
- Check "Won Group" for group winners (they get +1 bonus point)

### Entering Knockout Bracket
- Go to Admin → Knockout
- Enter team IDs (e.g. "ESP", "FRA") for each matchup by round
- Enter scores once played
- Team IDs autocomplete from the full team list

### Managing Participants
- Go to Admin → Manage Picks
- Remove participants or export/import data

## Scoring Reference

| Event | Points |
|-------|--------|
| Group match win | 3 |
| Group match draw | 1 |
| Advancing from groups | +1 |
| Winning group | +1 |
| R32 win | 4 |
| R16 win | 6 |
| QF win | 10 |
| SF win | 16 |
| Champion | 24 |
