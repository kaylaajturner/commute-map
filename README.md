# Commute Range Map

How far can you live from work and keep your drive under a time limit?
Enter a work address, when you need to arrive, when you leave, and your max commute.
The map colors every reachable area by drive time and hatches the areas over your limit.
Hover any area to see its neighborhood name and exact morning and evening drive times.

Everything runs in the browser on free tiers. No server is needed.

## What it uses (all free)

| Piece | Service | Free allowance |
|---|---|---|
| Base map | CARTO Positron via MapLibre | No key needed |
| Drive-time zones | Mapbox Isochrone API | 100,000 requests / month |
| Exact times on hover | Mapbox Directions API | 100,000 requests / month |
| Address search + neighborhood names | Mapbox Geocoding API | 100,000 requests / month |
| Hex grid + math | Turf.js (runs in your browser) | Free |

A normal session uses roughly 6 requests per time combination plus 3 per new area you hover.
Results are cached in your browser, so revisiting a time combination costs nothing.
You would have to work very hard to leave the free tier.

## Setup

### 1. Get a Mapbox token
1. Create a free account at [mapbox.com](https://account.mapbox.com/auth/signup). Without a payment method this is a "demo access" account: one default token, no URL restrictions, and Mapbox can't charge you. If you hit a usage cap, access just pauses until the cap resets.
2. Copy your default public token (it starts with `pk.`).

### 2. Keep the token out of the repo
Leave `MAPBOX_TOKEN` blank in `index.html`. Since the token can't be restricted, anyone who found it in a public repo could use up your free allowance.

Instead, open the live page and paste the token into the **Mapbox public token** box. It's saved only in that browser, so do this once on each device you use. Anyone else who visits will see the box and need their own free token.

### 3. Publish on GitHub Pages
1. Create a new public repo called `commute-map`.
2. Upload `index.html` and this `README.md`.
3. **Settings → Pages →** Deploy from a branch, `main`, `/ (root)`.
4. It will be live at `yourdomain/commute-map/` in a minute or two.

### Testing locally
You can also open `index.html` straight from your computer and paste the token there. It works the same way.

### If you ever add a payment method
At that point overage charges become possible. Before you add one, create a new token with URL restrictions for your site, and stop using the default one.

## How it works

- **Coloring:** Mapbox returns drive-time zones (5, 10, 15 … 60 minutes) from your work address. The page lays a hex grid over them and gives each hex its zone for the morning and evening legs. The slider and "Color by" options only restyle the map, so they're instant and cost no requests.
- **Morning vs. evening:** Evening zones are measured leaving work at your end time. Mapbox can only measure *outward* from a point, so morning zones are an estimate that uses traffic from 30 minutes before your start time. The hover tooltip fixes this with a real "arrive by" route from that spot.
- **Traffic:** All times use typical traffic for the next Tuesday, so they reflect normal rush hour rather than whatever is happening right now.
- **Times snap to 15 minutes** so results can be cached and reused.

## Known limits

- Driving only. Free transit-time APIs don't cover this well.
- One-way drives over 60 minutes aren't shown (Mapbox's limit).
- Zone colors are in 5-minute steps; hover for exact minutes.
- The "farthest" distance in the summary is straight-line, not road distance.

## Ideas for later

- Show a second work address (for a partner) and highlight the overlap.
- Let users click to pin a few candidate homes and compare them.
- Add rent or home-price data per neighborhood.
