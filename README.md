# Travian rog.x2 – Ancient Europe region map

Interactive map of the [rog.x2.europe.travian.com](https://rog.x2.europe.travian.com/) server: all 87 named regions (official outlines), every village coloured by tribe or alliance, filters, and a sortable players & villages list.

**Live page:** published to GitHub Pages by the workflow below.

## How it updates

Travian regenerates the public `map.sql` every day at midnight server time (UTC). The GitHub Actions workflow [`.github/workflows/update-map.yml`](.github/workflows/update-map.yml) downloads it, rebuilds the page and deploys it on every push to `main`. GitHub's own `schedule:` trigger never fired for this repo, so a Claude cloud routine ("Travian map nightly refresh", https://claude.ai/code/routines/trig_01VVGSmT2iiq3LzjkG5fzhqp) pushes an empty "Nightly map refresh" commit at 00:20 UTC every night. The `schedule:` entries stay as a harmless fallback. It can also be started by hand from the **Actions** tab (**Run workflow**).

## Shareable links

The page keeps its settings in the address bar: map position and zoom (`v=x,y,zoom`), display options and filters that differ from the defaults (`o`), colour mode (`by`), highlighted regions (`r`), shown tribes (`t`), player (`p`), minimum population (`pop`), alliance filter (`a`), overlay alliances hidden (`hide`) and the open side panel (`panel`, `list`). **🔗 Copy link** copies it. A link with any of these replaces the settings saved in the browser; a plain link keeps them.

## Alliance plans tab

The **Alliance plans** tab lists friendly and enemy alliances. Enemy alliances are just a list of the regions they hold, prefilled from `map.sql` with the regions where they have the most population (3+ villages), to be checked by hand. For each friendly alliance it holds its teams, the region each team is settling now, the regions already taken and the regions planned next, with live numbers from `map.sql` for each region. On the **Map** tab, the **Alliance plans overlay** panel colours each plan alliance's regions in its own colour (taken = the alliance's colour, settling now = darker, planned next = lighter; enemy regions hatched). Each alliance's colour is a hex code saved in `plans.json`, set with the colour picker on its card. B.U., BUPhx, BU-UN, NEFES, NnFn and C&D default to the coalition map's colours; other alliances get a generated colour (cool for friendly, warm for enemy); each viewer can tick which alliances to show. The plan is stored in `plans.json` in this repo, so everyone sees the same plan. Anyone can view it. To edit, open **Editor access** and paste a GitHub token with write access to this repo; the token stays in that browser. Saves are commits marked `[skip ci]`, so they don't trigger a rebuild, and the page always reads the latest `plans.json` straight from GitHub.

## Farms & changes tab

Every build saves a compact copy of that day's `map.sql` (village, owner, population) to `history/YYYY-MM-DD.json` and the workflow commits it (`[skip ci]`). The page gets the last 8 days:

- **Potential farms**: villages of players whose total population hasn't grown for N days (or, optionally, just the village's population), with distance from your coordinates, filters for distance, population and alliance (friendly plan alliances are left out by default), and a copyable list. Active players almost always grow every day, so no growth usually means inactive. It can't see troops or hidden resources.
- **My farms**: paste the rally point's Farm List page (select all, copy) and the page reads every list. The page text has no coordinates, so each target is matched to the village with that name at that distance from the list's own village (taken from the village list on the same page). Each target gets a status: inactive, growing, new owner, gone, or not found (oases, renamed villages). Potential farms gets a Farm list column and an "in / not in my farm lists" filter. The lists stay in that browser only (localStorage) and the pasted text isn't kept.
- **Changes**: new villages (and new players), conquered villages, villages that disappeared, population drops (catapults, conquest; summed per village, with % of its peak), and alliance joins/leaves/switches since a chosen day.

Tracking started on 12 Nov 2026, so the history only goes back to then.

## Files

- `build/update_map.py` — downloads `map.sql`, parses every village and writes `travian-regions.html`.
- `build/template.html` — the page (HTML/CSS/JS).
- `build/regions.json` — official region outlines from the in-game region pages (they don't change).
- `history/` — one snapshot per day, written by the build.

Build locally:

```bash
python3 build/update_map.py
```
