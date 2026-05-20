# 🌍 VJ-GeoIntel

**Geospatial Intelligence Platform** — click anywhere on Earth to get live environmental, weather, and air quality data for that coordinate.

---

## What It Does

VJ-GeoIntel is a single-file HTML application powered by Leaflet.js and free open-data APIs. Click any point on the map and get a full intelligence panel in seconds — no API keys, no backend, no sign-up.

**Data retrieved per location:**
- 🌫️ Air Quality — US AQI, EU AQI, PM2.5, PM10, O₃, NO₂, SO₂, CO, NH₃, Dust
- 🌡️ Weather — Temperature, Feels like, Humidity, Dew point, Wind, Gusts, UV Index, Pressure, Visibility, Precipitation, Cloud cover
- 🌅 Solar — Sunrise, Sunset, Daylight duration
- 🏔️ Geography — Elevation, Soil temperature, Coordinates (Decimal + DMS for India)
- 🇮🇳 India-specific — CPCB AQI (official sub-index method), Monsoon season banner, DMS coordinate display

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


---



## Data Sources & Accuracy

- **Weather & AQI** — Open-Meteo uses ERA5 reanalysis + forecast models. Updates hourly.
- **Geocoding** — Nominatim reverse-geocodes coordinates to human-readable names. Street-level for populated areas, district-level fallback for remote regions.
- **Elevation** — 90m SRTM digital elevation model via Open-Meteo.
- **India CPCB AQI** — Calculated client-side using the official sub-index method (PM2.5 + PM10 breakpoints as per CPCB guidelines). Only shown when coordinates are within India's bounding box.

---



## License

Personal / educational use. All data comes from open, free APIs — see their respective terms:
- [Open-Meteo Terms](https://open-meteo.com/en/terms)
- [OpenStreetMap / Nominatim Policy](https://operations.osmfoundation.org/policies/nominatim/)
- [CartoDB Basemaps](https://carto.com/legal/)

---

*Built by VJ — a lightweight geospatial intelligence tool using only free, open data.*
