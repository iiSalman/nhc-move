# Darb NHC: UX enhancements on v3 (pending approval for v2)

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

## Verified

- Automated run (headless Chrome): all 12 behaviours exercised, full ride flow, no JavaScript errors.
- Screenshots checked: filtered home sheet, vehicle sheet, trip planner, search, ride display.
- `git apply --check` confirms the patch applies cleanly to `v2/index.html`.

## Replicate on v2 (after approval)

Everything at once:

```bash
cd ~/Desktop/ClaudeTests/nhc-move
git apply v3-ux.patch          # patch already targets v2/index.html
git add v2 && git commit -m "v2: apply approved UX enhancements" && git push
```

Only some items: open `v3-ux.patch`, keep the hunks for the approved UX numbers (each hunk is labelled `# UX-n` in `scratchpad/ux-patch.py`), or ask me to port the approved list.
