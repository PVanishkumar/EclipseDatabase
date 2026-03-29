# EclipseDB — 30,000-Year Eclipse Catalog Browser

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19066334.svg)](https://doi.org/10.5281/zenodo.19066334)
[![License: MIT](https://img.shields.io/badge/Browser%20UI-MIT-blue.svg)](LICENSE)
[![License: CC BY 4.0](https://img.shields.io/badge/Catalog%20Data-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

A fully static web browser for a validated 30,000-year solar and lunar eclipse catalog. **Try the live version at [eclipsedb.org](https://eclipsedb.org/)**, or open `index.html` locally in any browser — no installation, no backend, no build step required.

---

## What this repository contains

|File|Description|
|-|-|
|`index.html`|The complete browser UI — all filtering, search, and display logic|
|`eclipses.sqlite.gz`|Pre-built SQLite database (~3–4 MB compressed), loaded in-browser via [sql.js](https://github.com/sql-js/sql.js/)|

The catalog data, raw source files, generation pipeline, and companion methodology paper are published separately on Zenodo:

> Kumar, P.V. Anish (2026). *A Validated 30,000-Year Solar Eclipse Catalog: Foundations for Statistical Eclipse Identification in Archaeoastronomy* (Version 1) [Dataset]. Zenodo.
> **https://doi.org/10.5281/zenodo.19066334**

---

## Live browser

**[eclipsedb.org](https://eclipsedb.org/)** hosts the browser with the full catalog pre-loaded. No download or setup required.

---

## The catalog

||Solar|Lunar|
|-|-|-|
|Eclipses|70,647|71,914|
|Year span|−13000 to +17000|−13000 to +17000|
|Ephemeris|JPL DE441|JPL DE441|
|Saros series covered|183|—|

Extends the verified eclipse record by **6×** relative to the NASA Five-Millennium Canon (Espenak & Meeus 2006/2009), which covers −1999 to +3000.

**Validation (from companion paper):**

* Tier 1 — External audit vs NASA Canon: 100% Saros agreement, 99.0% type agreement across 11,896 matched events
* Tier 2 — Internal residuals: gamma mean = 0.000, σ = 0.0006
* Tier 3 — Physical consistency: nodal precession period recovered at 9.3064 yr (FAP ≪ 10⁻¹⁰), IAU precession rate at 50.30 arcsec/yr

---

## Browser features

* **70,647 solar + 71,914 lunar eclipses** browsable in a single interface
* Dual-handle year range slider — typeable inputs accept `−5000`, `3000 BCE`, `500 AD`
* Astronomical year notation ↔ BCE/AD toggle (default matches NASA Canon convention)
* Filter by eclipse type, Saros series, Rasi (Lahiri sidereal / Vedic), proximity to solstice or equinox
* Sort by date, magnitude, duration, gamma, tropical longitude, or sidereal longitude
* **Eclipse season pattern** — each event tagged with its adjacent-eclipse context within ±16 days (e.g. L-S-L: a solar flanked by lunars on both sides)
* **Multi-ayanamsha display** — sidereal longitudes and Nakshatra shown under Lahiri, Raman, KP, or Fagan-Bradley; Rasi filter always uses the stored Lahiri column
* **Researcher mode** — an additional filter layer for historical and archaeoastronomical work:
  * Filter by Moon's Nakshatra (all 27 lunar mansions)
  * Filter by Indian season / Ritu (Vasanta, Grishma, Varsha, Sharad, Hemanta, Shishira), derived from Sun's tropical longitude — precession-correct for all 30,000 years
  * Besselian-based **observer location filter**: enter a latitude/longitude and the browser computes visibility (total, partial, or none) at that site for every eclipse in the result set, within a ≤ 2,000-year window
  * Totality-only sub-filter within the location filter
* **Shareable filter links** — copy a permalink that restores your exact filter state
* **ΔT confidence grading** on every row (see table below)
* **Detail panel** for each eclipse: planet positions for all seven classical bodies (tropical + sidereal longitude, Nakshatra, Rasi), Besselian elements, geocentric ephemeris, contact times (P1/U1/C1/C2/U4/P4), local apparent time, and derived quality metrics
* **E101 reference panel** — column definitions, Sanskrit/Vedic constellation names, and methodology notes available in-browser

### ΔT confidence grading

Each eclipse carries a confidence tier derived from its ΔT value, following Morrison & Stephenson (2004). Geographic parameters (longitude of greatest eclipse) grow increasingly uncertain for pre-telescopic epochs. Eclipse type, gamma, duration, and Saros membership are ΔT-independent and reliable across the full span.

|Tier|ΔT|Lon uncertainty|What it means|
|-|-|-|-|
|Certain|< 120 s|< 60 km|Path known to city-level precision|
|Regional|120 – 2,000 s|60 – 1,000 km|±1–3 timezones|
|Continental|2 – 20 ks|1,000 – 10,000 km|Right continent, rough region|
|Speculative|> 20 ks|> 10,000 km|Hemisphere only|

> 1 second of ΔT error ≈ 0.46 km of longitude shift at the equator.

---

## Usage

### View online

Open **[eclipsedb.org](https://eclipsedb.org/)** in any modern browser. The full catalog loads automatically.

### View locally

Download or clone the repository, then open `index.html` directly in any modern browser. No server required.

https://github.com/PVanishkumar/EclipseDatabase/tree/main

```
The browser fetches `eclipses.sqlite.gz` from the same directory, decompresses it using
the browser's native DecompressionStream API, and loads it into sql.js. This happens
once and is then cached.
```

---

## Column reference

The database contains approximately 100 columns per table. Key columns are documented below. The full schema is in the archived README on Zenodo.

### Core identity and timing (both tables)

|Column|ΔT class|Description|
|-|-|-|
|`cat`|—|Sequential catalog number|
|`date_str`|—|Date of greatest eclipse (proleptic Gregorian)|
|`td`|—|Time of greatest eclipse (Terrestrial Dynamical Time)|
|`ge_jd_td`|—|Julian Date of greatest eclipse (TDT)|
|`ge_jd_ut1`|ΔT-sens|Julian Date of greatest eclipse (UT1)|
|`delta_t`|—|TT − UT1 in seconds at epoch|
|`luna`|ΔT-indep|Lunation number (Brown); unique across all 30,000 years|
|`saros`|ΔT-indep|Saros series number|
|`conf_tier`|—|ΔT confidence tier: certain / regional / continental / speculative|
|`uncert_km`|—|Geographic longitude uncertainty in km derived from ΔT|

### Eclipse geometry (both tables)

|Column|ΔT class|Description|
|-|-|-|
|`type`|ΔT-indep|Solar: T=Total, A=Annular, H=Hybrid, P=Partial · Lunar: Ts/Tn=Total, Ps/Pn=Partial, Ns/Nn=Penumbral|
|`gamma`|ΔT-indep|Shadow axis distance from Earth centre in Earth radii|
|`lat`|ΔT-sens|Latitude of greatest eclipse|
|`lon`|ΔT-sens|Longitude of greatest eclipse|

### Solar-specific columns

|Column|ΔT class|Description|
|-|-|-|
|`mag`|ΔT-indep|Eclipse magnitude (Moon/Sun apparent diameter ratio; >1 = total)|
|`dur` / `dur_seconds`|ΔT-indep|Duration of totality/annularity at greatest eclipse|
|`width_km`|ΔT-sens|Path width in km at greatest eclipse|
|`alt`|ΔT-sens|Solar altitude above horizon at greatest eclipse|
|`centrality_idx`|ΔT-indep|1 − \|γ\| (1 = perfectly central)|
|`strength`|mixed|Composite quality score: magnitude × ln(duration) × path width|

### Lunar-specific columns

|Column|ΔT class|Description|
|-|-|-|
|`pen_mag`|ΔT-indep|Penumbral magnitude|
|`umb_mag`|ΔT-indep|Umbral magnitude (— for penumbral-only eclipses)|
|`tot_dur` / `dur_tot_min`|ΔT-indep|Duration of totality (total lunar eclipses only)|
|`dur_par_min`|ΔT-indep|Duration of umbral partial phase|
|`dur_pen_min`|ΔT-indep|Duration of penumbral contact (P1 to P4)|
|`pi_m`|ΔT-indep|Moon's horizontal parallax at greatest eclipse|
|`s_m`|ΔT-indep|Moon's apparent semi-diameter at greatest eclipse|
|`umbral_depth`|ΔT-indep|Umbral penetration depth: umb_mag − 1 (zero for penumbral-only)|
|`opposition_str`|ΔT-indep|Opposition quality score: (2 − \|γ\|) × umb_mag|

### Longitude and season columns (both tables)

|Column|ΔT class|Description|
|-|-|-|
|`trop_lon`|ΔT-indep|Tropical longitude of the eclipsed body (0°=vernal equinox · 90°=summer solstice · 180°=autumnal equinox · 270°=winter solstice)|
|`sid_lon`|ΔT-indep|Sidereal longitude of the eclipsed body (Lahiri ayanamsha)|
|`rasi`|ΔT-indep|Vedic rasi of the eclipsed body (Lahiri)|
|`season_lon_dist`|ΔT-indep|Tropical longitude distance to the nearest solstice or equinox|
|`season_pattern`|ΔT-indep|Adjacent-eclipse pattern within ±16 days: L-S-L, S-L, L-S, S-S, isolated, etc.|
|`ritu`|ΔT-indep|Indian 6-season (Vasanta · Grishma · Varsha · Sharad · Hemanta · Shishira) derived from Sun's tropical longitude|

### Planetary positions at greatest eclipse (both tables)

For each of the seven classical bodies — Sun (`sun`), Moon (`moo`), Mercury (`mer`), Venus (`ven`), Mars (`mar`), Jupiter (`jup`), Saturn (`sat`) — four columns are stored:

|Column pattern|ΔT class|Description|
|-|-|-|
|`{body}_trop`|ΔT-indep|Tropical ecliptic longitude (degrees)|
|`{body}_sid`|ΔT-indep|Sidereal ecliptic longitude, Lahiri ayanamsha (degrees)|
|`{body}_nak`|ΔT-indep|Nakshatra (one of 27 lunar mansions, 13.33° each)|
|`{body}_rasi`|ΔT-indep|Vedic rasi (Lahiri)|

Example: `mer_trop`, `mer_sid`, `mer_nak`, `mer_rasi` for Mercury. The browser's detail panel recomputes Nakshatra and Rasi under all four ayanamshas (Lahiri, Raman, KP, Fagan-Bradley) by applying a fixed offset to the stored Lahiri `_sid` value at display time.

### Besselian elements

Solar eclipses: polynomial coefficients (C0–C3) for shadow-axis x, y, Greenwich Hour Angle μ, Sun declination d, penumbral radius l₁, umbral radius l₂; plus tan f₁, tan f₂, T₀. Column pattern: `x_c0`, `x_c1`, `x_c2`, `x_c3`, `y_c0`, … `tan_f1`, `tan_f2`, `bess_t0`.

Lunar eclipses: polynomial coefficients for x, y, d; plus f₁, f₂ (shadow cone half-angles in degrees), π_m (horizontal parallax), s_m (apparent semi-diameter), T₀.

### Contact times (both tables)

JD (TD and UT1) and geographic coordinates stored for each contact:

|Contact|Meaning|
|-|-|
|P1 / P4|First / last external penumbral contact|
|U1 / U4|First / last umbral contact (umbral eclipses only)|
|C1 / C2|First / second central contact (total or annular)|

Column pattern: `p1_jd_td`, `p1_jd_ut1`, `p1_lat`, `p1_lon`; same for u1, c1, c2, u4, p4.

### Geocentric Cartesian positions — solar only

Moon and Sun X/Y/Z in km (ICRF frame, geocentric, at moment of greatest eclipse in TDT):
`moon_x_km`, `moon_y_km`, `moon_z_km`, `sun_x_km`, `sun_y_km`, `sun_z_km`.

---

## Important caveats

**Geographic coordinates are ΔT-sensitive.** Longitude of greatest eclipse carries growing uncertainty for pre-telescopic epochs — approximately ±62° at 2000 BCE and ±104° at 4000 BCE. The browser displays this explicitly via the confidence tier on every row.

**Eclipse type, gamma, duration, tropical longitude, sidereal longitude, Saros membership, Nakshatra, Rasi, and all planetary longitudes are ΔT-independent** and are reliable across the full 30,000-year span.

For the full uncertainty analysis see the companion paper on Zenodo.

---

## Citation

If you use this catalog or browser in research, please cite the Zenodo dataset:

```bibtex
@dataset{PVanishkumar,
  author    = {Kumar, P.V. Anish},
  title     = {A Validated 30,000-Year Solar Eclipse Catalog: Foundations
               for Statistical Eclipse Identification in Archaeoastronomy},
  year      = {2026},
  publisher = {Zenodo},
  version   = {1},
  doi       = {10.5281/zenodo.19066334},
  url       = {https://doi.org/10.5281/zenodo.19066334}
}
```

---

## References

* Meeus, J. (1998). *Astronomical Algorithms*, 2nd ed. Willmann-Bell.
* Espenak, F. & Meeus, J. (2006). *Five Millennium Canon of Solar Eclipses: −1999 to +3000*. NASA TP-2006-214141.
* Espenak, F. & Meeus, J. (2009). *Five Millennium Canon of Lunar Eclipses: −1999 to +3000*. NASA TP-2009-214172.
* Morrison, L.V. & Stephenson, F.R. (2004). Historical values of the Earth's clock error ΔT and the calculation of eclipses. *Journal for the History of Astronomy*, 35, 327–336.
* Acton, C.H. (1996). Ancillary data services of NASA's Navigation and Ancillary Information Facility. *Planetary and Space Science*, 44(1), 65–70.
* Stephenson, F.R. (1997). *Historical Eclipses and Earth's Rotation*. Cambridge University Press.

---

## License

**Browser UI** (`index.html`): [MIT License](LICENSE) — © 2026 P.V. Anish Kumar

**Catalog data** (`eclipses.sqlite.gz` and all eclipse data derived from it): [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)

The catalog generation pipeline and methodology implementation are proprietary and not published in this repository. The companion paper describing the methodology in full is available on Zenodo.
