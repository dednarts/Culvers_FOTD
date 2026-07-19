# Culver's Flavor Finder

A single-page web app that shows today's [Flavor of the Day](https://www.culvers.com/flavor-of-the-day) at every Culver's near you, sorted by distance. Tap any store to see its next 7 days.

**Live: https://dednarts.github.io/Culvers_FOTD/**

Covers every Culver's nationwide, always current — the data comes live from Culver's own store locator, so there's nothing to maintain.

## Why

The official Culver's app shows Flavor of the Day, but it takes too many taps to compare locations. This is one screen: open it, see what's scoopable nearby, done.

## Features

- Today's flavor for every store within your radius, nearest first — from a single request.
- Distance slider (3–25 miles), by geolocation or ZIP code.
- Flag-a-flavor field (default: `cookie dough`): any store serving a matching flavor today is highlighted in caramel, and matching days in a store's forecast light up too.
- Tap a card for that store's next 7 days.
- Flags stores marked temporarily closed instead of showing a stale flavor.
- Works as a phone home-screen "app" via Chrome's *Add to Home Screen*.

## How to use it

1. Open the link above on your phone (or any device).
2. Tap **Use my location**, or open **Set location manually** and enter a 5-digit US ZIP code.
3. Adjust the radius if you like; tap any store for its week ahead.

Power users: the manual field also accepts raw `lat, lng` coordinates.

## How it works

It's one file — `index.html` — with no build step and no data files.

- **Nearby stores + today's flavor:** a single call to Culver's public locator, `https://www.culvers.com/api/locator/getLocations?lat=…&long=…&radius=…`. The response includes each store's address, coordinates, slug, and today's flavor, so one request covers the whole list.
- **Location:** the browser's geolocation, or a ZIP geocoded via the free [zippopotam.us](https://api.zippopotam.us) service.
- **The 7-day forecast** (on tap) comes from that store's own page, read out of the embedded `__NEXT_DATA__` JSON. The fetch verifies the page's own slug matches the store requested, so a mismatched/cached response can't show the wrong store's week.
- **CORS:** both culvers.com endpoints block direct browser calls, so requests route through a free public CORS proxy (`api.allorigins.win`, with fallbacks). Since the store list is a single request, proxy rate-limiting is a non-issue; the forecast is one more request only when you tap.

## Deploying

Just `index.html`. Drop it in the repo (named `index.html`), enable GitHub Pages on the branch, and it's live. No `locations.json`, no build script — earlier versions used those, and they've been removed.

## Limitations

- Public CORS proxies can rate-limit or go down. For personal use this is rarely an issue.
- The forecast depends on Culver's publishing it; some stores list fewer days.
- "Add to Home Screen" gets you an icon, but not push notifications — this is just a webpage.

## Credit

Flavor and location data © Culver Franchising System, LLC. This project is an unaffiliated personal tool.
