# Deadlock Counter-Buy Helper

A tiny web app for the game **Deadlock**: click the enemy heroes and it ranks the defensive
items to buy, weighted by how many of those enemies each item shuts down — so you spend souls
on the items that cover the most threats at once.

### ▶ Live: **https://andreas-ja.github.io/deadlock-counter-buy/**

No install, no login, works on phones. Share the link with your team and check counters mid-match.

---

## What it does

- **Pick the enemy line-up** from a grid of all 38 heroes (with portraits). Up to 6 = a full team.
- **Ranked counter items** appear on the right, sorted by *overlap* — the badge shows `3 of 5`,
  meaning that item counters 3 of your 5 selected enemies.
- **Hard-counter tier.** Items that are near-mandatory against a specific hero get a gold
  `★ Hard counter` tag and a scoring bump, so they never get buried under a lower-value pick —
  even when they only counter one enemy.
- **"Don't buy" warnings.** Traps like *Indomitable vs Abrams/Dynamo* or *Slowing Hex vs The Doorman*
  are flagged so you don't waste souls.
- **Every counter is explained** — each item lists which enemy it answers and why.
- **Item icons + hero photos**, light/dark themes, and a responsive layout for desktop or phone.
- **Installable (PWA).** On phones it can be added to the home screen, launches full-screen
  without browser chrome, and works offline — no app store, no install file.

## How the ranking works

Each selected enemy contributes the items that counter it. For every item:

```
score = (number of enemies it counters) + (number of those it HARD-counters)
```

So a hard-counter is worth roughly one extra threat covered. Items are then split into
**Priority buys** (hard-counters, or anything covering 2+ enemies) and **Situational**
(single-target answers). Ties break toward the hard-counters.

## Project structure

```
index.html             The entire app — data + UI + logic in one file, no build step.
img/*.webp             38 hero portrait icons.
img/items/*.webp       33 item shop icons.
manifest.webmanifest   PWA metadata (name, icons, theme) that makes it installable.
sw.js                  Service worker: network-first HTML, cache-first assets, offline support.
icon-*.png             App icons, including a maskable variant for Android.
```

All the game knowledge lives in a few JavaScript objects near the top of the `<script>` in
`index.html`:

| Object     | What it holds                                                             |
|------------|---------------------------------------------------------------------------|
| `DATA`     | Per-hero counter items + an `avoid` list, each with a one-line reason.     |
| `MUST`     | The hard-counter item(s) per hero (drives the gold `★` tag and the bump).  |
| `IMG`      | Hero → portrait filename.                                                  |
| `ITEMIMG`  | Item → shop-icon filename.                                                 |
| `BLURBS`   | Short general description shown under each item.                           |

To tweak advice, edit `DATA` / `MUST`. To add a hero or item, add its entry **and** drop its
icon in `img/` or `img/items/`.

## Running locally

It's a static file — just open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## Deploying updates

The live site is GitHub Pages served from `main`. Push and it rebuilds in ~30–60s:

```bash
git add -A && git commit -m "update counter data" && git push
```

## Credits & data

- **Counter-item advice** is compiled from a community Deadlock counter-buying guide (video),
  condensed into one-line reasons. It's one creator's opinion and is situational — adapt it to
  your own hero and build.
- **Hero and item artwork** comes from the community asset mirror at
  [deadlock-api.com](https://deadlock-api.com). Art belongs to Valve.

## Disclaimer

Not affiliated with or endorsed by Valve. *Deadlock* and all related artwork are trademarks and
property of Valve Corporation. This is a non-commercial fan tool.
