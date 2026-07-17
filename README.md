# Culver's Flavor Finder

A small web app that shows today's [Flavor of the Day](https://www.culvers.com/flavor-of-the-day) at every Culver's restaurant near you, sorted by distance, with the rest of the week visible at a glance.

**Live: https://dednarts.github.io/Culvers_FOTD/**

Covers all ~1,000 Culver's locations nationwide. Enter a ZIP code (or use geolocation) and see the closest ones.

## Why

The official Culver's app shows Flavor of the Day, but it takes too many taps to compare locations. This is one screen: open it, see what's scoopable nearby, done.

## Features

- Shows today's flavor and the rest of the published week for the ten closest stores in range.
- Distance slider (3–25 miles) from your current location or entered ZIP.
- Flag-a-flavor field — type any keyword (default: `cookie dough`) and any location serving a matching flavor today is highlighted in caramel. Matching days later in the week light up too.
- Reads flavor data straight from culvers.com (no API key, no signup).
- Works as a phone home-screen "app" via Chrome's *Add to Home Screen*.

## How to use it

1. Open the link above on your phone (or any device).
2. Tap **Use my location** and allow the prompt, or open **Set location manually** and enter a 5-digit US ZIP code.
3. Adjust the search radius if you want.

Power users: the manual field also accepts raw `lat, lng` coordinates.

## Regenerating the location list

The app loads `locations.json` — a list of every US Culver's with coordinates — at startup. To rebuild that file (say, once a year to catch new store openings):

```
python build_locations.py
```

The script reads culvers.com's sitemap, fetches each restaurant page, extracts the coordinates from the "Get Directions" link, and writes `locations.json`. Takes about 10-15 minutes for the full ~1,000 stores. Commit the updated file alongside `index.html`.

Requires only Python 3.10+ standard library — no `pip install` needed.

## How it works

The page loads `locations.json` at startup, then asks for your geolocation (or geocodes a ZIP via the free [zippopotam.us](https://api.zippopotam.us) service). It computes Haversine distance to every store, filters by the slider value, and shows the ten closest stores in range. For each of those, it fetches the restaurant's public page from culvers.com and parses the iCalendar data embedded in the "Add to Calendar" links — that's the same data Culver's uses for calendar export, and it's stayed stable across their site redesigns.

Because browsers can't fetch culvers.com directly (CORS), requests go through a free public CORS proxy (`api.allorigins.win` with fallbacks). Requests are staggered by 250 ms and capped at 10 concurrent to avoid burst rate limits.

## Limitations

- Only the ten closest stores in your radius get today's flavor loaded. Anything beyond that is counted but not shown ("…and 5 more within 15 miles").
- Public CORS proxies can rate-limit or go down. For personal use this is rarely an issue.
- "Add to Home Screen" gets you an icon, but not push notifications — this is just a webpage.

## Credit

Flavor data © Culver Franchising System, LLC. This project is an unaffiliated personal tool.
