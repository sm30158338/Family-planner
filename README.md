# Family Hub — a free, no-server Skylight-style dashboard for a Fire tablet

A single HTML file that turns an old Amazon Fire HD 8 (or any tablet) into an
always-on family planner: shared calendar, weather, and a chore chart with ⭐
star rewards. No server, no subscription, no Google account on the tablet.

## What's on the screen
Four tabs sit under the slim header (clock, date, weather):

- **Calendar** — your family's live Google Calendar (month / week / list views)
  with the **chore chart beneath it**: tap-to-complete, per-person ⭐ star
  rewards, midnight reset, and each person gets a **colour you pick in
  Settings** (a colour swatch per family member — pink approved 🩷)
- **Meals** — an editable 7-day meal grid for the current week; today's row is
  highlighted; saves as you type
- **Shopping** — a checkable list; add by typing and hitting Add (or Enter);
  “Clear ✓” removes completed items
- **To-Do** — powered by **Google Tasks** (official API, two-way): once signed
  in with Google, tasks added on the wall appear in the “Family Hub” list in
  Google Tasks, and anything added from Gmail/Calendar on your phones appears
  on the wall within a minute. Without sign-in it falls back to a local list.
  See “One-time Google setup” below.

Everything else:
- **Weather** — current conditions + 3-day forecast in the header
  (Open-Meteo, free; Celsius by default — set `UNIT: "F"` for Fahrenheit)
- **Themes** — “Paper” (light, calm, e-ink-ish) by default; switch to **Dark**
  in Settings → Theme
- **Night dimming** — the screen automatically fades to black between
  `DIM_START` and `DIM_END` hours (default 10 pm – 6 am) but stays on
- **Fullscreen** — tap ⛶ in the header (or tap the page once) to hide the
  browser chrome

## One-time Google setup (for the To-Do tab) — ~10 minutes
The To-Do tab talks to the official Google Tasks API, which needs two things:
a web address for the dashboard, and an OAuth Client ID.

### 1. Put the dashboard on a web address (GitHub Pages — free)
1. Repo → **Settings → Pages** → Source: **Deploy from a branch** → branch
   `main`, folder `/ (root)` → Save
2. After a minute the dashboard is live at
   `https://sm30158338.github.io/Family-planner/` — point the Fire tablet's
   Silk browser there (then tap once for fullscreen as usual). No more
   downloading files to the tablet: future updates go live by editing
   `index.html` on GitHub.

### 2. Create the OAuth Client ID
1. Go to [console.cloud.google.com](https://console.cloud.google.com) and
   create a project (any name, e.g. “Family Hub”)
2. **APIs & Services → Library** → search “Tasks API” → **Enable**
3. **APIs & Services → OAuth consent screen**: type **External**, fill in the
   app name and your email; under **Test users** add both your and your wife's
   Google accounts (Google shows a “testing” warning you can click through)
4. **APIs & Services → Credentials → Create credentials → OAuth client ID**:
   type **Web application**, and add
   `https://sm30158338.github.io` to **Authorized JavaScript origins**
5. Copy the Client ID (ends in `.apps.googleusercontent.com`)
6. On the tablet (and any phone you want to add tasks from): open the
   dashboard → ⚙️ → **Google Tasks** → paste the Client ID → **Save Client ID**
7. To-Do tab → **Sign in with Google** → choose the account → done

Each person signs in with their own Google account on their own device; tasks
land in the same shared “Family Hub” list. Sign-in lasts about an hour and
refreshes automatically while the page is open.

## Syncing lists between phones and the tablet (optional)
The Meals / Shopping / To-Do tabs live on each device by default. To sync them
across your family's phones and the tablet, the dashboard can use a **private
GitHub Gist** as a tiny free database — no server needed:

1. Every person who wants to edit lists needs a GitHub account, and one shared
   “personal access token (classic)” with the **gist** scope
   (github.com/settings/tokens → Generate new token → tick `gist`)
2. On any device: ⚙️ Settings → **Sync lists between devices** → paste the
   token → tap **Connect / create sync** (leave Gist ID empty the first time)
3. It creates a private Gist and shows its ID. On other devices, paste the
   token AND the same Gist ID, then Connect.
4. Lists now push changes within seconds and pull every minute (and whenever
   you come back to the page). The token and Gist ID are stored per-device.

Calendar and chores stay device-local: chores are deliberately done at the
wall, and the calendar comes straight from Google.

## One-time setup (10 minutes)

### 1. Get your Google Calendar embed link
1. On a computer, open [calendar.google.com](https://calendar.google.com)
2. Gear icon ⚙️ → **Settings** → click your calendar in the left sidebar
3. At the top of its settings page, make the calendar **public**
   ("Make available to public" — check the box; choose "See only free/busy" if
   you prefer to hide event details from strangers)
4. Scroll to **Integrate calendar → Embed code**, and copy just the URL inside
   `src="..."` (the part starting with `https://calendar.google.com/calendar/embed?...`)

### 2. Find your town (for weather)
Just the name, e.g. `Austin` or `Portland, OR`.

### 3. Edit the file
Open `index.html` in any text editor. Near the bottom of the `<script>`
section you'll find a `CONFIG` block:

```js
const CONFIG = {
  CALENDAR_SRC: "",   // paste your calendar embed URL here, between the quotes
  CITY: "",           // your town here
  TZ: "",             // optional, e.g. "America/New_York" (blank = tablet clock)
  DIM_START: 22,      // night dimming start hour (10 pm)
  DIM_END: 6,         // night dimming end hour (6 am)
};
```

Fill in `CALENDAR_SRC` and `CITY`, save.

### 4. Put it on the tablet
1. Copy `index.html` onto the Fire tablet (email it to yourself, or use a USB
   cable, or open this GitHub page in the tablet's Silk browser and save it)
2. Open it with **Silk** (Files app → tap the file → open with Silk)
3. Tip: tap Silk's ⋮ menu → **Add page to** → favorites, so it's one tap away

### 5. Go full screen

In Silk, tap the **⛶** button in the dashboard's header — the address bar and
status bar disappear (most Fire OS 7+ tablets support this; if Silk refuses,
tap the page once — the browser requires a touch before allowing fullscreen,
so the dashboard also auto-enters fullscreen on your first tap). To leave
fullscreen, tap ⛶ again or press the back button.

For a true appliance-style setup (launches full screen by itself, nothing else
touchable), install the free **[WallPanel](https://github.com/thecowan/wallpanel-android)**
APK (side-load it: download on the tablet with Silk → allow unknown apps →
install) and point it at the dashboard, or use **Fully Kiosk** (~$8). WallPanel
needs a URL, so either serve `index.html` from any static host (e.g. GitHub
Pages, which this repo can enable for free), or use a
`file:///sdcard/Download/index.html` URL if your build accepts it.

### 6. Keep the screen always on
Same trick as any Fire-wall-display:
**Settings → Device Options → tap "Serial Number" 7 times → Developer Options
→ enable "Stay Awake"** (screen then never sleeps while charging).

Hang it on the wall, plug it in, done. ⭐

## Editing chores & people
Tap the **⚙️** in the top-right corner of the dashboard:
- Add/rename/remove family members
- Add chores and assign them to a person (or "Anyone")
- Reset star totals

People's names appear as colored pills; tap one to filter the chore list.
Completing a chore gives that person a ⭐; un-completing takes it back.
Chores reset every night at midnight; star totals keep growing (reset anytime
in settings).

## Customizing
Since it's just one HTML file, everything is yours to tweak — colors live in
the CSS `:root` block at the top. Font sizes, layout ratios, extra panels
(family message board, meal plan, animated photo rotation) are easy additions.

## Privacy note
Making the calendar public means anyone with the embed link can view it
(be sure to check **"See only free/busy (hide details)"** rather than
"See all event details" if the calendar is sensitive).
