# Switch

Interview prep tracker. Three tracks (DSA / LLD / system design), a daily log with a
20-minute floor, and a streak that only breaks after two consecutive misses.

Static site, no build step, no backend. All data lives in `localStorage` on the device
that made it.

## Deploy to GitHub Pages

```bash
git init
git add .
git commit -m "switch"
git branch -M main
git remote add origin git@github.com:<you>/switch.git
git push -u origin main
```

Then **Settings → Pages → Source: Deploy from a branch → main / (root)**.

Give it a minute. It'll be live at `https://<you>.github.io/switch/`.

Make the repo **public** — Pages on private repos needs a paid plan.

All paths are relative, so the subdirectory URL works without changes.

## Install on the phone

**Install before you log anything.** iOS gives a home-screen app its own storage
container, separate from Safari — anything you tick in the browser first won't carry over.

- **iPhone** — open the URL in Safari (not Chrome), Share → Add to Home Screen.
- **Android** — open in Chrome, menu → Install app / Add to Home screen.

Deleting the home-screen icon deletes the data with it. That's the main way you lose the log.

## Backup

The Data tab has Export file, Copy as text, and Import backup. Export takes five seconds;
do it every few weeks. The file is plain JSON:

```json
{ "start": "2026-08-14", "log": { "2026-08-14": "full" }, "done": { "d01": 1 } }
```

Import replaces whatever is on the device — it does not merge. That's deliberate: a real
merge needs per-field timestamps, which is the sync project, not this one.

## Shipping a change

Edit `index.html`, then **bump `CACHE` in `sw.js`** (`switch-v1` → `switch-v2`) and push.
Without the bump, installed phones keep serving the old cached copy.

The page itself is network-first, so a new version usually appears on the next open anyway.
Other assets are cache-first and need the bump.

## Files

| | |
|---|---|
| `index.html` | Everything — markup, styles, app logic, content |
| `manifest.webmanifest` | Name, icons, standalone display |
| `sw.js` | Offline cache |
| `icon-*.png` | Home-screen icons |

## Editing the plan

The three content arrays at the top of the `<script>` are `DSA`, `LLD`, and `SD`.
Each entry is `[module name, day estimate, [[id, label, days], ...]]`.

**IDs are the storage key.** Change an existing id and that item's tick is orphaned.
Add new items with fresh ids.
