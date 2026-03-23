# Changelog

All notable changes to this project will be documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.1.0] — 2025-03-23

### Fixed
- **Linear model now renders as a straight line on log axis.** Previously the linear fit was computed in linear concentration space (`y = m·x + b`), which appeared as a curve when plotted on the logarithmic x-axis. Now fits `y = m·log₁₀(x) + b`, consistent with how GraphPad Prism renders a linear fit on a log-scale axis.
- Slope units updated to `%KD per log₁₀(ng/g)` throughout (stat card, CSV export).

---

## [1.0.0] — 2025-03-23

### Initial release

#### Data pipeline
- Tab 01: RQ file upload → automatic % knockdown calculation via (1 − RQ) × 100
- Tab 02: MSD Workbench CSV upload → log-log linear standard curve fitting → tissue concentration back-calculation
- Tab 03: Merge on sample ID → PK/PD correlation

#### Standard curve fitting
- Log-log linear regression replacing unstable 4PL gradient descent
- Automatic standard vs. sample separation (detects STD/Standard/Cal labels)
- Sub-LLOQ flagging for vehicle/control animals

#### PK/PD modelling
- Three models fitted simultaneously: linear, simple Emax, sigmoid Emax (4-parameter)
- AIC-based model comparison with ΔAIC and plain-language recommendation
- Guidance on when EC50/Emax can and cannot be reliably estimated

#### Chart
- Dose group coloring — one color per group, auto-assigned from RQ file
- Scroll to zoom, Shift+drag to pan, reset zoom button
- Click any data point to select — shows crosshair, floating label, detail panel
- Click empty chart area to deselect
- All three model curves overlaid simultaneously; toggle via legend
- Dynamic y-axis bounds, fixed upper limit at 100%
- Deterministic x-jitter to separate overlapping points

#### Table
- Sortable by any column (click header)
- Group filter dropdown
- Hover sync between table rows and chart dots
- Color-coded animal IDs by dose group

#### Performance
- Web Worker for sigmoid fitting — no UI freeze on merge
- Curve data cached — only recomputed on model/axis change
- Chart animations disabled for instant response
- Event listeners registered once at init (no accumulation leak)
- Shadow blur removed from canvas drawing

#### Deployment
- Single HTML file, no installation required
- `bundle_pkpd.py` script to produce self-contained version with no CDN dependencies
- Self-contained version verified to work fully offline

---

## How to read version numbers

`MAJOR.MINOR.PATCH`

- **MAJOR** — breaking change to input file format or output structure
- **MINOR** — new feature, backwards compatible
- **PATCH** — bug fix
