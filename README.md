# Culver's Flavor Finder

A small web app that shows today's [Flavor of the Day](https://www.culvers.com/flavor-of-the-day) at every Culver's restaurant near you, sorted by distance, with the rest of the week visible at a glance.

**Live: https://dednarts.github.io/Culvers_FOTD/**

Built for the Fox Cities area of Wisconsin, but easy to point at any region by editing one list in the source.

## Why

The official Culver's app shows Flavor of the Day, but it takes too many taps to compare locations. This is one screen: open it, see what's scoopable nearby, done.

## Features

- Shows today's flavor and the rest of the published week for every nearby store.
- Distance slider (3–25 miles) from your current location.
- Flag-a-flavor field — type any keyword (default: `cookie dough`) and any location serving a matching flavor today is highlighted in caramel. Matching days later in the week light up too.
- Reads flavor data straight from culvers.com (no API key, no signup).
- Works as a phone home-screen "app" via Chrome's *Add to Home Screen*.

## How to use it

1. Open the link above on your phone (or any device).
2. Tap **Use my location** and allow the prompt.
3. Adjust the search radius if you want.
4. Tap any flavor name to see the store.

If you don't want to grant location access, tap **Set location manually** and enter a 5-digit US ZIP code (power users: `lat, lng` also accepted).

## Customizing for your area

The store list is hard-coded at the top of the `<script>` block in `index.html`. Each entry looks like:

```js
{ name: "Appleton", slug: "appleton", addr: "N Westhill Blvd", lat: 44.2900, lng: -88.4670 },
```

- **`slug`** is the last path segment of the store's URL on culvers.com — for example `https://www.culvers.com/restaurants/appleton` becomes `appleton`.
- **`lat`/`lng`** can be roughly estimated from a Google Maps search of the address; accuracy to within a few hundred feet is plenty for distance filtering.

Add as many entries as you want — distance filtering keeps the UI clean even if the list is long.

## How it works

The page asks for your geolocation, computes the Haversine distance to each store in the list, and filters by the slider value. For each in-range store, it fetches the public restaurant page from culvers.com and parses the iCalendar data embedded in the "Add to Calendar" links — that's the same data Culver's uses to populate calendar export, and it's stayed stable across their site redesigns.

Because browsers can't fetch culvers.com directly (CORS), requests go through a free public CORS proxy (`api.allorigins.win` with fallbacks). Requests are staggered by 250 ms to avoid burst rate limits.

## Limitations

- Public CORS proxies can rate-limit or go down. For personal use this is rarely an issue.
- The store list is hard-coded; there's no live "find Culver's near me" search.
- "Add to Home Screen" gets you an icon, but not push notifications — this is just a webpage.

## Credit

Flavor data © Culver Franchising System, LLC. This project is an unaffiliated personal tool.
