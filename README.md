# Travian rog.x2 – Ancient Europe region map

Interactive map of the [rog.x2.europe.travian.com](https://rog.x2.europe.travian.com/) server: all 87 named regions (official outlines), every village coloured by tribe or alliance, filters, and a sortable players & villages list.

**Live page:** published to GitHub Pages by the workflow below.

## How it updates

Travian regenerates the public `map.sql` every day at midnight server time (UTC). The GitHub Actions workflow [`.github/workflows/update-map.yml`](.github/workflows/update-map.yml) downloads it, rebuilds the page and deploys it on every push to `main`. GitHub's own `schedule:` trigger never fired for this repo, so a Claude cloud routine ("Travian map nightly refresh", https://claude.ai/code/routines/trig_01VVGSmT2iiq3LzjkG5fzhqp) pushes an empty "Nightly map refresh" commit at 00:20 UTC every night. The `schedule:` entries stay as a harmless fallback. It can also be started by hand from the **Actions** tab (**Run workflow**).

## Files

- `build/update_map.py` — downloads `map.sql`, parses every village and writes `travian-regions.html`.
- `build/template.html` — the page (HTML/CSS/JS).
- `build/regions.json` — official region outlines from the in-game region pages (they don't change).

Build locally:

```bash
python3 build/update_map.py
```
