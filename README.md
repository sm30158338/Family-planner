# Family Hub — a free, no-server Skylight-style dashboard for a Fire tablet

A single HTML file that turns an old Amazon Fire HD 8 (or any tablet) into an
always-on family planner: shared calendar, weather, and a chore chart with ⭐
star rewards. No server, no subscription, no Google account on the tablet.

## What's on the screen
- **Clock & date** — big and readable from across the kitchen
- **Calendar** — your family's live Google Calendar (month / week / list views)
- **Weather** — current conditions + 3-day forecast, tucked into the header
  (Open-Meteo, free; Celsius by default — set `UNIT: "F"` for Fahrenheit)
- **Portrait-first layout** — slim header, the calendar takes the full width as
  the main focus, with the chore chart in a grid beneath it
- **Chores** — tap-to-complete chart with per-person star rewards; reset every
  midnight; edited right on the tablet via the ⚙️ button
- **Night dimming** — the screen automatically fades to black between
  `DIM_START` and `DIM_END` hours (default 10 pm – 6 am) but stays on

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

### 5. Keep the screen always on
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
