# EclipseDB — 30,000-Year Eclipse Catalog Browser

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19066334.svg)](https://doi.org/10.5281/zenodo.19066334)
[![License: MIT](https://img.shields.io/badge/Browser%20UI-MIT-blue.svg)](LICENSE)
[![License: CC BY 4.0](https://img.shields.io/badge/Catalog%20Data-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

A fully static, single-file web browser for a validated 30,000-year solar and lunar eclipse catalog.

**Browse the live catalog at [eclipsedb.org](https://eclipsedb.org/)**

---

## How it works

`index.html` is the entire application — there is no backend. It is a self-contained HTML/JS file that fetches the eclipse database from the same server, decompresses it in the browser using the native `DecompressionStream` API, and loads it into [sql.js](https://github.com/sql-js/sql.js/) (SQLite compiled to WebAssembly). All filtering, searching, and display runs entirely client-side.

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

## Data availability

The core plain-text catalog files (`solar_eclipse_final.txt`, `lunar_eclipse_final.txt`), companion methodology paper, and column-schema README are permanently archived and citable on Zenodo:

> Kumar, P.V. Anish (2026). *A Validated 30,000-Year Solar Eclipse Catalog: Foundations for Statistical Eclipse Identification in Archaeoastronomy* (Version 1) [Dataset]. Zenodo.
> **https://doi.org/10.5281/zenodo.19066334**

The live browser at [eclipsedb.org](https://eclipsedb.org/) provides an enhanced, continually updated view of the same catalog — with additional columns, filters, and display features not present in the archived plain-text files. The extended database is not distributed as a downloadable file; it is accessible only through the browser.

---

## Browser features

* **70,647 solar + 71,914 lunar eclipses** browsable in a single interface
* Dual-handle year range slider — typeable inputs accept `−5000`, `3000 BCE`, `500 AD`
* Astronomical year notation ↔ BCE/AD toggle (default matches NASA Canon convention)
* Filter by eclipse type, Saros series, Rasi (Lahiri sidereal / Vedic), proximity to solstice or equinox
* Sort by date, magnitude, duration, gamma, tropical longitude, or sidereal longitude
* **Eclipse season pattern** — each event tagged with its adjacent-eclipse context within ±16 days (L-S-L, S-L, L-S, isolated, etc.)
* **Multi-ayanamsha display** — sidereal longitudes and Nakshatra shown under Lahiri, Raman, KP, or Fagan-Bradley
* **Researcher mode** — additional filters for historical and archaeoastronomical work:
  * Filter by Moon's Nakshatra (all 27 lunar mansions)
  * Filter by Indian season / Ritu (Vasanta, Grishma, Varsha, Sharad, Hemanta, Shishira), derived from Sun's tropical longitude — precession-correct for all 30,000 years
  * Besselian-based observer location filter: enter a latitude/longitude and the browser computes visibility (total, partial, or none) at that site for every eclipse in the filtered result set, within a ≤ 2,000-year window
  * Totality-only sub-filter within the location filter
* **Shareable filter links** — copy a permalink that restores your exact filter state
* **ΔT confidence grading** on every row (see table below)
* **Detail panel** for each eclipse: planetary positions for all seven classical bodies (tropical + sidereal longitude, Nakshatra, Rasi under any ayanamsha), Besselian elements, geocentric ephemeris, contact times (P1/U1/C1/C2/U4/P4), local apparent time, and derived quality metrics
* **E101 reference panel** — column definitions, Sanskrit/Vedic constellation names, and methodology notes in-browser

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

**Plain-text catalog data** (`solar_eclipse_final.txt`, `lunar_eclipse_final.txt`): [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)

The extended database accessible via eclipsedb.org, the catalog generation pipeline, and the methodology implementation are proprietary and not distributed.
