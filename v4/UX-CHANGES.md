# Darb NHC: v4

## v4: map from the Khuzam landscape master plan

v4 = v3 (all UX-1..19) plus a new map built from the landscape master plan drawing. Live: https://iisalman.github.io/nhc-move/v4/

| # | Change | Notes |
|---|---|---|
| MAP-1 | **Real layout, generalised**: street network by width class (18/25/30/40 m), street blocks, green boulevard, Main Wadi park outline, reservoir and channel, cycle path, boardwalks, bridges, seating terraces | Shapes are simplified (~4–7 m) and snapped to a 2‑unit grid, rescaled to app units (~1.4 m per unit). No survey coordinates, plot lines or construction detail are published |
| MAP-2 | **Asset types as icons**: playgrounds, fitness stations, kiosks, drinking fountains, splash pads, shaded pergolas, event stage, local mosques, school, cluster gardens. Tap one for a card with directions | Pieces are clustered per type, so icons mark an area, not an exact installed position |
| MAP-3 | **Real routing**: shortest path over the generalised roads plus the park's cycle path, replacing v3's fixed grid route | Distances use the plan's real scale |
| MAP-4 | **Places moved to fitting real spots**: stations at bike‑rack clusters, vehicles parked at racks, destinations at the Friday Mosque, the wadi park, the commercial strip, the market and a cluster garden; gathering spots at the pergolas, a park play area, the mosque courtyard, the market plaza and the cluster garden | Names stay "Khuzam" as requested |
| MAP-6 | **Bridges respected in routing and movement**: the wadi can only be crossed on the road bridge or the boardwalk footbridges. A promenade runs along each bank, with entrances to the surrounding streets; the water acts as a barrier for every link in the network and for start and end points, so trips attach on their own side. Bridges and footbridges are drawn where routes cross (timber decks, a road bridge with deck edges) and added to the map legend | Checked on all 4,032 routes between every vehicle, station, destination, gathering spot and asset: 0 water crossings outside a bridge, 0 straight-line shortcuts, 1,798 routes use a bridge or footbridge |
| MAP-5 | **Zones from the plan**: the wadi park is the slow zone (10 km/h), the school is the no‑ride zone, P zones at each destination | |

---

# UX enhancements on v3 (pending approval for v2)

Live preview: https://iisalman.github.io/nhc-move/v3/
v2 (unchanged): https://iisalman.github.io/nhc-move/v2/

All changes are in `v3/index.html`. The same file in `v2/` differs only in two links, so the whole set can be replicated with one command (see the end). Each item can also be ported on its own; the "Where" column names the function or markup to copy.

| # | Enhancement | Why | Where in index.html |
|---|---|---|---|
| UX-1 | Status bar clock shows **7:40**, matching the "31°C at 7:40 PM" copy | The old 9:41 contradicted the evening story the app tells | `.fake-status` markup |
| UX-2 | Toasts can carry an **action button**; the Reserve toast gets **Cancel**. Toast is `role="status" aria-live="polite"`, modal is `role="dialog"`, **Escape** closes modals | Undo for a 10‑minute hold without hunting for the button; screen readers announce feedback | `toast()`, `#toast`, `#modal`, `toggleReserve()` |
| UX-3 | Filter chips, segmented controls and tabs expose `aria-pressed` / `aria-current`; the selected filter chip **scrolls into view** | Assistive tech can tell what is selected; the "Gatherings" chip used to be selected off‑screen | `#filters` click handler, `#chSeg`, `#lbSeg`, `setTab()` |
| UX-4 | **Phone/browser back button closes pushed pages** (search, scan, photo, receipt, profile, tree). Focus moves into the opened page | Android back gesture and iOS swipe-back now work like a native app instead of leaving the site | `openPage()`, `closePage()`, new `popstate` listener |
| UX-5 | **Home sheet follows the active filter**: "3 stations nearby" with a station list, "3 gatherings happening now" with live spots first, "4 e‑bikes within 420 m" for a mode | The headline and list used to always talk about rides even when the user filtered for stations or gatherings | `sheetExplore()` (`h1`, `nearList`), sheet click handler |
| UX-6 | **Dashed walking line** from you to the selected vehicle | Shows how to reach the vehicle before tapping "Scan to ride" | `<path id="walkLine">`, `selectVehicle()`, `clearRoute()` |
| UX-7 | Trip planner shows the **price under every mode** (Free / Included / 14 SAR) | Users compared time but had to tap each mode to see cost | `renderPlan()` mode rows |
| UX-8 | Ride display adds **"1.5 km left · about 6 min"**, then **"Arrived · park in the P zone"**; the end button reads **"End ride and park here"** on arrival | Lime/Careem-style ETA; a clear arrival state tells the rider what to do next | `#hudEta` markup, `rideTick()`, `renderRideSheet()` |
| UX-9 | **Search**: input focused on open, **Recent** places when empty, results grouped into **Gathering spots** (with live dot) and **Places**, matches on description too ("coffee" finds the Majlis) | Faster to repeat a trip; gatherings were mixed in with places | `renderSearch()` |
| UX-10 | **Leaving a challenge asks first** and says progress is kept | One tap on "Joined" used to silently drop the user out | `#chList` click handler |
| UX-11 | Streak card says **"Ends at midnight · a short walk keeps it"** when today's trip is still due | Turns a vague nudge into a deadline | `plantCardInner()` |
| UX-12 | Vehicle pins get an accessible name and keyboard focus; the manual code field is auto-focused | Basic accessibility of the map; one less tap in the scan fallback | `drawPins()`, `#enterCode` modal |

## Balance pass (less text, more visual weight)

| # | Enhancement | Why | Where in index.html |
|---|---|---|---|
| UX-13 | Home: the weather sentence becomes **three condition tiles** (31° · 6–10 PM best time · Shade routes on) | Scannable at a glance; the sentence repeated what the tiles now show | `sheetExplore()`, `.cond` CSS |
| UX-14 | **Sidr card**: bigger tree art, stage name, streak chip and a one‑word **state pill** (Safe today / Move by midnight / Losing leaves / Plant again). Three lines of copy removed | The tree is the message; the card now reads in under a second | `plantCardInner()`, `.pill` CSS |
| UX-15 | **Tree page**: status is one short line ("One trip today keeps day 7."); the five rule paragraphs become **four icon tiles** (Trips grow it · Streaks speed it up · Missed days shrink it · A week off resets it) | Rules are glanceable instead of read; the fifth rule already lives under the prize tiers | `plantStatus()`, `renderPlant()`, `.howgrid` CSS |
| UX-16 | **Challenges**: descriptions cut to 4–6 words and clamped to one line; Ramadan banner and footer caption shortened | Cards are icon + name + reward, as in Sehhaty/SFA | `CHAL` data, `#p-challenges` markup, `.ch p` CSS |
| UX-17 | **Rewards**: the off‑season paragraph becomes a **season strip** (moon Ramadan · gift Eid · bag School · flag National Day) with the next one highlighted; the in‑season intro is one line | Shows the calendar instead of describing it | `renderSeason()`, `.seasons` CSS |
| UX-18 | Shorter supporting copy: shaded‑route toggle, community goal, passes note, three onboarding lines | Every sentence trimmed to its point | `renderPlan()`, `#p-community`, `#p-rewards`, `#onb` |

## Arabic / English

| # | Enhancement | Why | Where in index.html |
|---|---|---|---|
| UX-19 | **Arabic ⇄ English toggle** (ع / EN button on the map, in Settings → Language, and on the intro screens). Arabic flips the whole app to right‑to‑left with the IBM Plex Sans Arabic face; the choice is remembered on the device. Every screen, sheet, toast and pop‑up is translated, including dynamic text (distances, prices, streak lines, prize names). Western digits are kept, as most Saudi apps do | Khuzam residents are Arabic‑first; the demo can be shown in either language | `AR` dictionary + `ARX` patterns, `setLang()`, the `MutationObserver` translation layer, `.app[dir=rtl]` CSS, `.langbtn` / `#langOnb` / `#langRow` markup |

How it works: English stays the source of truth in every template. In Arabic, a translation layer swaps text nodes using the dictionary (exact phrases) and patterns (strings with numbers or names), and re-applies itself whenever a screen re-renders. To add a string later, add its English → Arabic pair to `AR`.

Note on UX-4: the back‑button handling was reworked during this pass to use history state bookkeeping only (no `history.back()` calls from code), which is more robust across browsers.

## Verified

- Automated run (headless Chrome): all 19 behaviours exercised, full ride flow, no JavaScript errors.
- Screenshots checked: filtered home sheet, vehicle sheet, trip planner, search, ride display, tree page, rewards, challenges.
- `git apply --check` confirms the patch applies cleanly to `v2/index.html`.

## Replicate on v2 (after approval)

Everything at once:

```bash
cd ~/Desktop/ClaudeTests/nhc-move
git apply v3-ux.patch          # patch already targets v2/index.html
git add v2 && git commit -m "v2: apply approved UX enhancements" && git push
```

Only some items: open `v3-ux.patch`, keep the hunks for the approved UX numbers (each hunk is labelled `# UX-n` in `scratchpad/ux-patch.py`), or ask me to port the approved list.
