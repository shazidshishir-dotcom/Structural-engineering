# Bangladesh Wind & EQ Map (BNBC-2020)

An interactive, browser-based lookup tool for **BNBC-2020 site design loads** across
Bangladesh. Pick any location and instantly read its **basic wind speed (V)** and
**seismic zone / zone coefficient (Z)** — by clicking the map, entering coordinates,
or searching a thana/district.

**Live:** https://bdmap-wind-eq.vercel.app/

---

## What it does

Two data layers, switchable with a single **Wind / Seismic** toggle:

- **Wind** — BNBC-2020 basic wind speed *V*, shown in both **km/h** and **m/s**.
- **Seismic** — BNBC-2020 **seismic zone (1–4)** and **zone coefficient Z**
  (0.12 / 0.20 / 0.28 / 0.36 = PGA in *g* on rock).

Three ways to read a value at any location:

1. **Click / tap** a point on the map.
2. **Enter coordinates** (`lat, lng`, e.g. `23.75, 90.38`) — auto-swaps if given as `lng, lat`.
3. **Search** a thana or district (autocomplete dropdown appears directly under the box).

Each result shows the division, district (and thana where applicable), the value in the
selected units, and — for wind — an exact upazila reading where relevant.

---

## Coverage

| Level | Count |
|---|---|
| Divisions | 8 |
| Districts | 64 (all covered in both layers) |
| Thanas / upazilas | 542 |

---

## Data sources & methodology

The tool is deliberately **traceable to source data** rather than to any interpolated
third-party map. Every value comes from a validated BNBC-2020 table.

### Wind speed (V)
- Source: user-supplied **BNBC-2020 basic wind speed table**, thana-wise, in km/h.
- Values are uniform per district **except two anomalies**, which are handled at
  upazila level so they read exactly:
  - **Hatiya** (Noakhali) — 260 km/h vs the district's 184 km/h.
  - **Ishwardi** (Pabna) — 225 km/h vs the district's 202 km/h.

### Seismic zone (Z)
- Source: user-supplied **BNBC-2020 seismic zone table**, thana-wise.
- Mapping: Zone-1 = 0.12, Zone-2 = 0.20, Zone-3 = 0.28, Zone-4 = 0.36.
- Zone is uniform per district across all 64 districts in this dataset.

### Administrative boundaries
- District (ADM2) polygons: OCHA / BBS boundary data (64 districts).
- Upazila (ADM3) polygons added only for **Noakhali** and **Pabna** (18 upazilas)
  so the Hatiya and Ishwardi anomalies render as distinct colored areas and resolve
  correctly by coordinate.
- Geometry is topology-simplified (mapshaper) to keep the whole app lightweight.

### Wind colour bands (km/h)
`< 150` · `150–169` · `170–189` · `190–204` · `205–219` · `220–239` · `240–259` · `≥ 260`
(pale → deep maroon; higher wind = warmer, i.e. coastal cyclone belt is darkest).

---

## Technical notes

- **Single self-contained file.** Leaflet, fonts, and all geo/attribute data are
  embedded in one `index.html` (~440 KB). No build step, no external data files.
- **Online + offline.** Online it loads a light street basemap (CartoDB) with city
  labels; offline the basemap simply doesn't load, while the colored zones and **all
  lookups keep working** — open the file directly in a browser, no server needed.
- **Responsive.** Works on laptop/desktop and mobile (tap to read, pinch to zoom),
  with iOS safe-area handling so overlays don't clip under notches or browser bars.
- **Stack:** Leaflet 1.9.4 · GeoJSON choropleth · client-side point-in-polygon for
  coordinate lookup · custom autocomplete · Space Grotesk + JetBrains Mono (embedded).

---

## Deploy (Vercel)

The file must be served as **`index.html`** at the project root.

- **Drop:** vercel.com/new → drag the project folder onto the page.
  Use the **same project name** to update the same URL.
- **CLI:** `vercel` then `vercel --prod` from the folder (keeps the same URL).
- **Git:** push to a repo → import at vercel.com/new (Output Directory = `.`).

No `vercel.json` or build configuration is required.

> **Mobile testing note:** in-app browsers (Messenger / Facebook) cache aggressively
> and can show a stale build. After deploying, open the link in Safari/Chrome directly
> (or hard-refresh) to see the latest version.

---

## Offline use

Open `index.html` in any browser (double-click). Everything except the street basemap
works with no internet — suitable for laptops, phones, USB drives, or a site office.

---

## Limitations & engineering notes

- Values are a **reference lookup**, not a substitute for a project-specific design
  calculation. Always confirm against the governing BNBC-2020 tables for the actual
  design.
- Wind and seismic layers come from **two separate BNBC-2020 tables**; the map
  boundaries are an independent administrative dataset (attribution in the app footer).
- Point-in-polygon lookups use district-level polygons (plus the two upazila-split
  districts). A coordinate near a border resolves to the containing district polygon.

---

## Roadmap (planned)

- **Seismic base shear + governing-load check** — compute BNBC-2020 base shear
  (Cs·W from Z, I, R, soil site class, period T, weight W) and flag whether seismic
  governs versus a user-supplied wind base shear.
- **Site response spectrum** curve *S_a(T)* for the selected location.
- **Batch / CSV portfolio lookup** — paste or upload tower coordinates and export
  wind V + seismic zone/Z for every site.
- Optional **Bangla ⇄ English** labels; per-site "load summary" export.

---

## Standards & credits

- Basic wind speed and seismic zoning per **BNBC-2020** (user-validated tables).
- Administrative boundaries: **OCHA / BBS** (ADM2), with ADM3 upazila polygons for
  Noakhali and Pabna.
- Basemap: **CartoDB** (online only). Map rendering: **Leaflet**.
