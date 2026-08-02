# 🌍 VJ-GeoIntel

**Geospatial Intelligence Platform** — click anywhere on Earth to get live environmental, weather, and air quality data for that coordinate.

<<<<<<< HEAD
Zero backend. Zero API keys. Zero build step. One HTML file, deployed on GitHub Pages, running entirely in the browser.

🔗 **Live demo:** [add your GitHub Pages link here]
🎥 **Video walkthrough:** [add your YouTube link here]

=======
>>>>>>> 72654aa41c509d14ebe0c71f79f587ee040d1a4e
---

## What It Does

<<<<<<< HEAD
VJ-GeoIntel is a single-file HTML application powered by Leaflet.js and free, open-data APIs. Click any point on the map and get a full intelligence panel in seconds.

**Data retrieved per location:**
- 🌫️ **Air Quality** — India CPCB AQI, US AQI, EU AQI, PM2.5, PM10, O₃, NO₂, SO₂, CO, NH₃, Dust
- 🌡️ **Weather** — Temperature, Feels like, Humidity, Dew point, Wind, Gusts, UV Index, Pressure, Visibility, Precipitation, Cloud cover
- 🌅 **Solar** — Sunrise, Sunset, Daylight duration
- 🏔️ **Geography** — Elevation, Soil temperature, Coordinates (Decimal + DMS for India)
- 🇮🇳 **India-specific** — CPCB AQI (official sub-index method), Monsoon season banner, DMS coordinate display
- 🕐 **Live local clock** — ticks in real time using the location's actual UTC offset, not a cached snapshot
=======
VJ-GeoIntel is a single-file HTML application powered by Leaflet.js and free open-data APIs. Click any point on the map and get a full intelligence panel in seconds — no API keys, no backend, no sign-up.

**Data retrieved per location:**
- 🌫️ Air Quality — US AQI, EU AQI, PM2.5, PM10, O₃, NO₂, SO₂, CO, NH₃, Dust
- 🌡️ Weather — Temperature, Feels like, Humidity, Dew point, Wind, Gusts, UV Index, Pressure, Visibility, Precipitation, Cloud cover
- 🌅 Solar — Sunrise, Sunset, Daylight duration
- 🏔️ Geography — Elevation, Soil temperature, Coordinates (Decimal + DMS for India)
- 🇮🇳 India-specific — CPCB AQI (official sub-index method), Monsoon season banner, DMS coordinate display
>>>>>>> 72654aa41c509d14ebe0c71f79f587ee040d1a4e

---

## Features

| Feature | Detail |
|---|---|
| Map Layers | Dark, Light, Voyager, OpenStreetMap, Satellite, Topographic, Streets + Labels overlay |
| Search | City name, place name, or raw coordinates |
| GPS Locate | One-click locate to your current position |
| °C / °F Toggle | Switch temperature units on the fly; re-renders all values |
| Dark / Light Theme | Auto-detects system preference, persists via localStorage |
| Recent Locations | Last 6 locations, collapsible, click to revisit |
| Export JSON | Downloads full raw data payload for the selected point |
| Pin Location | Drop persistent pins on the map |
| Share Link | Copies a URL with `?lat=&lng=` params — opens and auto-loads on visit |
| Copy Coords | Copy decimal coordinates to clipboard in one click |
| URL State | Location pushed to browser history — back/forward works |
<<<<<<< HEAD
| Request Timeouts | Any hung API call aborts after 12s with a clear error, instead of spinning forever |

---

## Air Quality Methodology

AQI is shown in three standards, in this order: **India CPCB → US EPA → EU**.

- **India CPCB AQI** is calculated client-side from PM2.5 and PM10 readings, using the exact breakpoints published in CPCB's *National Air Quality Index* Table 3.11. The "Severe" band (PM2.5 ≥ 250 µg/m³, PM10 ≥ 430 µg/m³) is officially open-ended in the CPCB standard — there's no upper concentration bound — so readings at or above that floor are reported as the scale maximum, AQI 500, rather than an invented extrapolation.
- **US AQI** and **EU AQI** are returned directly by Open-Meteo's air quality API, computed from their respective standards.
- Composite AQI requires at least PM2.5 or PM10 to be present; if both are missing for a location, the CPCB tile shows `—`.

---

## How It Works (for anyone extending this)

The whole app is one HTML file with three parts:

1. **Map layer** — Leaflet.js renders the base map and handles click/search/GPS events. Every interaction resolves to a `{lat, lng}` pair.
2. **Data layer** — `inspectLocation(lat, lng)` fires four parallel, keyless fetches:
   - Nominatim (OpenStreetMap) → place name via reverse geocoding
   - Open-Meteo Forecast API → weather + timezone
   - Open-Meteo Air Quality API → pollutant concentrations + US/EU AQI
   - Open-Meteo Elevation API → terrain height
3. **Render layer** — `renderPanel()` takes the combined response and builds the intel panel HTML. India-specific CPCB AQI, DMS coordinates, and the monsoon banner are computed client-side (see `calcIndiaAQI()`, `toDMS()`, `getMonsoon()`).

A few implementation details worth knowing if you're learning from this:

- **Timezone handling is the trickiest part.** Open-Meteo returns local timestamps as naive strings with no UTC offset (e.g. `"2026-08-02T14:30"`). Passing that straight into `new Date()` gets it reinterpreted in the *viewer's* browser timezone, not the *location's* — a classic and easy-to-miss bug. The fix (`parseLocalParts()`) parses the numeric components directly instead of trusting `Date` to guess. The live clock (`tickLocalClock()`) then uses the location's `utc_offset_seconds` against the device's real clock so the time keeps ticking accurately, decoupled from how stale the last data fetch is.
- **All fetches are keyless and rate-limit-friendly.** Nominatim in particular asks for reasonable use — this app makes one request per click, no polling.
- **XSS caution:** place names come from OpenStreetMap, which anyone can edit. They're escaped (`esc()`) before being injected into the DOM.

---

## Data Sources & Accuracy

- **Weather & AQI** — [Open-Meteo](https://open-meteo.com), using ERA5 reanalysis + forecast models. Updates roughly hourly.
- **Geocoding** — [Nominatim](https://nominatim.org) reverse-geocodes coordinates to human-readable names. Street-level for populated areas, district-level fallback for remote regions.
- **Elevation** — 90m SRTM digital elevation model via Open-Meteo.
- **India CPCB AQI** — Calculated client-side per the official CPCB sub-index method. Only shown when coordinates fall within India's bounding box.

No data source here is a substitute for official government advisories during pollution or weather emergencies — always defer to local authorities for anything safety-critical.

---

## Run It Locally

No build step, no dependencies to install:

```bash
git clone https://github.com/<your-username>/vj-geointel.git
cd vj-geointel
python3 -m http.server 8000
# open http://localhost:8000
```

Or just double-click `index.html` — it works as a static file too (some browsers restrict clipboard/geolocation APIs on `file://`, so a local server is safer).

## Deploy to GitHub Pages

1. Push `index.html` to your repo's default branch (or a `docs/` folder / `gh-pages` branch, per your settings).
2. Repo **Settings → Pages** → set source to that branch.
3. Your live URL will be `https://<username>.github.io/<repo>/`.

---

## Built to Learn From — Fork It

This project exists as much to be **taken apart and extended** as to be used. If you're learning frontend dev, geospatial tooling, or working with public APIs, here are some project ideas built on top of this codebase:

- **Historical mode** — swap the Forecast API for Open-Meteo's Historical Weather API and let people compare a location's air quality across dates.
- **Multi-point comparison** — pin 2–3 locations and render their stats side by side.
- **Heatmap overlay** — pull AQI for a grid of points around a city and render a pollution heatmap layer on the map.
- **PWA / offline shell** — add a service worker so the UI shell loads offline (data still needs network, obviously).
- **Alerts** — client-side threshold watcher that notifies when AQI at a saved location crosses into "Poor" or worse.
- **Other national AQI standards** — China, Indonesia, and others each have their own breakpoint tables; add them the same way CPCB was added.

Pull requests, forks, and "I built X on top of this" shout-outs are all welcome.

---

## License

Personal / educational use. All data comes from open, free APIs — respect their individual terms if you build on this:
=======


---



## Data Sources & Accuracy

- **Weather & AQI** — Open-Meteo uses ERA5 reanalysis + forecast models. Updates hourly.
- **Geocoding** — Nominatim reverse-geocodes coordinates to human-readable names. Street-level for populated areas, district-level fallback for remote regions.
- **Elevation** — 90m SRTM digital elevation model via Open-Meteo.
- **India CPCB AQI** — Calculated client-side using the official sub-index method (PM2.5 + PM10 breakpoints as per CPCB guidelines). Only shown when coordinates are within India's bounding box.

---



## License

Personal / educational use. All data comes from open, free APIs — see their respective terms:
>>>>>>> 72654aa41c509d14ebe0c71f79f587ee040d1a4e
- [Open-Meteo Terms](https://open-meteo.com/en/terms)
- [OpenStreetMap / Nominatim Policy](https://operations.osmfoundation.org/policies/nominatim/)
- [CartoDB Basemaps](https://carto.com/legal/)

---

<<<<<<< HEAD
*Built by VJ — a lightweight geospatial intelligence tool using only free, open data, made to be learned from and built upon.*
=======
*Built by VJ — a lightweight geospatial intelligence tool using only free, open data.*
>>>>>>> 72654aa41c509d14ebe0c71f79f587ee040d1a4e
