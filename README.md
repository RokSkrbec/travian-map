# Travian rog.x2 – Ancient Europe region map

Interactive map of the [rog.x2.europe.travian.com](https://rog.x2.europe.travian.com/) server: all 87 named regions (official outlines), every village coloured by tribe or alliance, filters, and a sortable players & villages list.

**Live page:** published to GitHub Pages by the workflow below.

## How it updates

Travian regenerates the public `map.sql` every day at midnight server time (UTC). The GitHub Actions workflow [`.github/workflows/update-map.yml`](.github/workflows/update-map.yml) downloads it, rebuilds the page and deploys it on every push to `main`. GitHub's own `schedule:` trigger never fired for this repo, so a Claude cloud routine ("Travian map nightly refresh", https://claude.ai/code/routines/trig_01VVGSmT2iiq3LzjkG5fzhqp) pushes an empty "Nightly map refresh" commit at 00:20 UTC every night. The `schedule:` entries stay as a harmless fallback. It can also be started by hand from the **Actions** tab (**Run workflow**).

## Alliance plans tab

The **Alliance plans** tab holds, per alliance, its teams, the region each team is settling now, the regions already taken and the regions planned next, with live numbers from `map.sql` for each region. On the **Map** tab, the **Alliance plans overlay** panel colours each plan alliance's regions in its own colour (dark = taken, middle = settling now, light = planned next); each viewer can tick which alliances to show. The plan is stored in `plans.json` in this repo, so everyone sees the same plan. Anyone can view it. To edit, open **Editor access** and paste a GitHub token with write access to this repo; the token stays in that browser. Saves are commits marked `[skip ci]`, so they don't trigger a rebuild, and the page always reads the latest `plans.json` straight from GitHub.

## Files

- `build/update_map.py` — downloads `map.sql`, parses every village and writes `travian-regions.html`.
- `build/template.html` — the page (HTML/CSS/JS).
- `build/regions.json` — official region outlines from the in-game region pages (they don't change).

Build locally:

```bash
python3 build/update_map.py
```
