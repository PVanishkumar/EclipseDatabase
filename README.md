EclipseDB — 30,000-Year Eclipse Catalog Browser
![DOI](https://doi.org/10.5281/zenodo.19066334)
![License: MIT](LICENSE)
![License: CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
A fully static web browser for a validated 30,000-year solar and lunar eclipse catalog. Open `index.html` in any browser — no installation, no backend, no build step required.
---
What this repository contains
File	Description
`index.html`	The complete browser UI — all filtering, search, and display logic
`eclipses.sqlite.gz`	Pre-built SQLite database (~3–4 MB compressed), loaded in-browser via sql.js
The catalog data, raw source files, generation pipeline, and companion methodology paper are published separately on Zenodo:
> Kumar, P.V. Anish (2026). \*A Validated 30,000-Year Solar Eclipse Catalog: Foundations for Statistical Eclipse Identification in Archaeoastronomy\* (Version 1) \[Dataset]. Zenodo.
> \*\*https://doi.org/10.5281/zenodo.19066334\*\*
---
The catalog
	Solar	Lunar
Eclipses	70,647	71,914
Year span	−13000 to +17000	−13000 to +17000
Ephemeris	JPL DE441	JPL DE441
Saros series covered	183	—
Extends the verified eclipse record by 6× relative to the NASA Five-Millennium Canon (Espenak & Meeus 2006/2009), which covers −1999 to +3000.
Validation (from companion paper):
Tier 1 — External audit vs NASA Canon: 100% Saros agreement, 99.0% type agreement across 11,896 matched events
Tier 2 — Internal residuals: gamma mean = 0.000, σ = 0.0006
Tier 3 — Physical consistency: nodal precession period recovered at 9.3064 yr (FAP ≪ 10⁻¹⁰), IAU precession rate at 50.30 arcsec/yr
---
Browser features
70,647 solar + 71,914 lunar eclipses browsable in a single interface
Dual-handle year range slider — typeable inputs accept `−5000`, `3000 BCE`, `500 AD`
Astronomical year notation ↔ BCE/AD toggle (default matches NASA Canon convention)
Filter by eclipse type, Saros series, Rasi (Lahiri sidereal / Vedic), proximity to solstice or equinox
Sort by date, magnitude, duration, gamma, or longitude uncertainty
ΔT confidence grading
Each eclipse carries a confidence tier derived from its ΔT value, following Morrison & Stephenson (2004). Geographic parameters (longitude of greatest eclipse) grow increasingly uncertain for pre-telescopic epochs. Eclipse type, gamma, duration, and Saros membership are ΔT-independent and reliable across the full span.
Tier	|ΔT|	Lon uncertainty	What it means
Certain	< 120 s	< 60 km	Path known to city-level precision
Regional	120 – 2,000 s	60 – 1,000 km	±1–3 timezones
Continental	2 – 20 ks	1,000 – 10,000 km	Right continent, rough region
Speculative	> 20 ks	> 10,000 km	Hemisphere only
> 1 second of ΔT error ≈ 0.46 km of longitude shift at the equator.
---
Usage
View locally
Download or clone the repository, then open `index.html` directly in any modern browser. No server required.
https://github.com/PVanishkumar/EclipseDatabase/tree/main
```

The browser fetches `eclipses.sqlite.gz` from the same directory, decompresses it using the browser's native `DecompressionStream` API, and loads it into sql.js. This happens once and is then cached.



## Column reference

### Solar eclipses

|Column|Description|
|-|-|
|Cat#|Sequential catalog number|
|Date|Year Mon DD (astronomical year numbering)|
|TD|Time of greatest eclipse (Terrestrial Dynamical Time)|
|ΔT(s)|TT − UT in seconds at epoch|
|Luna|Lunation number (Brown)|
|Sar|Saros series number|
|Type|T=Total, A=Annular, H=Hybrid, P±=Partial|
|QLE|Qualitative limb eclipse code|
|Gamma|Distance of shadow axis from Earth center (Earth radii)|
|Mag|Eclipse magnitude|
|Lat / Lon|Geographic coordinates of greatest eclipse|
|Alt / Azm|Solar altitude and azimuth at greatest eclipse|
|Width|Path width in km (central eclipses only)|
|Dur|Duration of totality/annularity at greatest eclipse|
|TropLon|Sun tropical longitude at greatest eclipse (degrees)|
|SidLon|Sun Lahiri sidereal longitude (degrees)|
|Rasi|Vedic rasi (Lahiri ayanamsha)|

### Lunar eclipses

|Column|Description|
|-|-|
|Cat#|Sequential catalog number|
|Date|Year Mon DD (astronomical year numbering)|
|TD|Time of greatest eclipse (TDT)|
|ΔT(s)|TT − UT in seconds at epoch|
|Luna|Lunation number (Brown)|
|Sar|Saros series number|
|Typ|Ts=Total, Ps=Partial, Ns=Penumbral, Pn=Partial-North|
|Gamma|Distance of shadow axis from Earth center (Earth radii)|
|PenMag|Penumbral magnitude|
|UmbMag|Umbral magnitude (— for penumbral eclipses)|
|Lat / Lon|Geographic sublunar point at greatest eclipse|
|TotDur|Duration of totality (Ts only)|
|UmbDur|Duration of umbral eclipse|
|TropLon|Moon tropical longitude (degrees)|
|SidLon|Moon Lahiri sidereal longitude (degrees)|
|Rasi|Vedic rasi (Lahiri ayanamsha)|

\---

## Important caveats

**Geographic coordinates are ΔT-sensitive.** Longitude of greatest eclipse carries growing uncertainty for pre-telescopic epochs — approximately ±62° at 2000 BCE and ±104° at 4000 BCE. The browser displays this explicitly via the confidence tier on every row.

**Eclipse type, gamma, duration, tropical longitude, and Saros membership are ΔT-independent** and are reliable across the full 30,000-year span.

For the full uncertainty analysis see the companion paper on Zenodo.

\---

## Citation

If you use this catalog or browser in research, please cite the Zenodo dataset:

```bibtex
PVanishkumar,
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
References
Meeus, J. (1998). Astronomical Algorithms, 2nd ed. Willmann-Bell.
Espenak, F. & Meeus, J. (2006). Five Millennium Canon of Solar Eclipses: −1999 to +3000. NASA TP-2006-214141.
Espenak, F. & Meeus, J. (2009). Five Millennium Canon of Lunar Eclipses: −1999 to +3000. NASA TP-2009-214172.
Morrison, L.V. & Stephenson, F.R. (2004). Historical values of the Earth's clock error ΔT and the calculation of eclipses. Journal for the History of Astronomy, 35, 327–336.
Acton, C.H. (1996). Ancillary data services of NASA's Navigation and Ancillary Information Facility. Planetary and Space Science, 44(1), 65–70.
Stephenson, F.R. (1997). Historical Eclipses and Earth's Rotation. Cambridge University Press.
---
License
Browser UI (`index.html`): MIT License — © 2026 P.V. Anish Kumar
Catalog data (`eclipses.sqlite.gz` and all eclipse data derived from it): Creative Commons Attribution 4.0 International (CC BY 4.0)
The catalog generation pipeline and methodology implementation are proprietary and not published in this repository. The companion paper describing the methodology in full is available on Zenodo.
