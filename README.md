# Travian rog.x2 – Ancient Europe region map

Interactive map of the [rog.x2.europe.travian.com](https://rog.x2.europe.travian.com/) server: all 87 named regions (official outlines), every village coloured by tribe or alliance, filters, and a sortable players & villages list.

**Live page:** published to GitHub Pages by the workflow below.

## How it updates

Travian regenerates the public `map.sql` every day at midnight server time (UTC). The GitHub Actions workflow [`.github/workflows/update-map.yml`](.github/workflows/update-map.yml) downloads it, rebuilds the page and deploys it. It can also be started by hand from the **Actions** tab (**Run workflow**). Don't use **Re-run** on an old run: the deploy fails with duplicate `github-pages` artifacts.

GitHub's own `schedule:` trigger never fired for this repo, so a [cron-job.org](https://cron-job.org) job starts the workflow every night at 00:20 UTC through the API:

```
POST https://api.github.com/repos/RokSkrbec/travian-map/actions/workflows/update-map.yml/dispatches
Accept: application/vnd.github+json
Authorization: Bearer <fine-grained token, this repo only, Actions: read and write>
X-GitHub-Api-Version: 2022-11-28
Content-Type: application/json

{"ref":"main"}
```

GitHub answers `204 No Content` when the run was started. The token expires; when it does, create a new one and paste it into the cron job. The `schedule:` entries stay in the workflow as a harmless fallback.

## Files

- `build/update_map.py` — downloads `map.sql`, parses every village and writes `travian-regions.html`.
- `build/template.html` — the page (HTML/CSS/JS).
- `build/regions.json` — official region outlines from the in-game region pages (they don't change).

Build locally:

```bash
python3 build/update_map.py
```
