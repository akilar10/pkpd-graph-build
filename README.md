# pkpd-graph-build
Replaces the Excel + GraphPad workflow for drug PK/PD analysis. Upload qPCR and MSD exports, get tissue concentration vs. mRNA knockdown correlations with automatic model selection. Runs entirely in the browser.
# PK/PD Correlator — Originally developed for Oligonucleotide Neuroscience. Widely applicable.

An interactive, browser-based tool for correlating tissue drug concentrations with mRNA knockdown in oligonucleotide (siRNA/ASO) neuroscience programs. Designed for mouse and NHP studies with terminal tissue collection.

## What it does

Most PK/PD analysis workflows for oligonucleotide programs require exporting data from multiple instruments, manually back-calculating concentrations in Excel, and importing everything into a general-purpose tool like GraphPad Prism. This app collapses that entire pipeline into a single session:

1. **Upload your qPCR export** → automatically converts RQ values (2^−ΔΔCt) to % mRNA knockdown
2. **Upload your MSD Workbench export** → fits a standard curve and back-calculates tissue concentrations
3. **View your PK/PD correlation** → fits linear, simple Emax, and sigmoid Emax models simultaneously, compares them using AIC, and tells you which is appropriate for your data

---

## Key features

- **MSD standard curve fitting** using log-log linear regression — robust across 3–4 concentration decades
- **Automatic model comparison** with AIC, ΔAIC, and a plain-language recommendation (linear vs sigmoid Emax)
- **Dose group coloring** — animals colored by dose level automatically from your RQ file's group column
- **Interactive chart** — scroll to zoom, Shift+drag to pan, click any data point to see a detail panel
- **Sortable, filterable table** — sort by any column, filter to a single dose group
- **Export** — PNG chart and CSV summary with fit parameters included
- **Self-contained option** — run the bundler script to produce a version with zero external dependencies, suitable for use with proprietary data

---

## Validated against published data

The tool has been validated against publicly available datasets from:

- **Brown et al., *Nature Biotechnology* 2022** — Alnylam C16-siRNA CNS platform, rat SOD1 IT dosing, frontal cortex PK/PD
- **Barker et al., *Science Translational Medicine* 2024** — Denali OTV-ASO platform, IV-dosed hTau × TfR KI mouse, MAPT knockdown

---

## Getting started

### Option A — Use immediately (requires internet)

Download `pkpd_v2.html` and open it in any modern browser. No installation required.

### Option B — Self-contained version (recommended for proprietary data)

The default app loads JavaScript libraries from external CDN servers. For use with unpublished compound data, generate a fully self-contained version with no external dependencies:

**Requirements:** Python 3.6+, internet access (one time only)

```bash
python3 bundle_pkpd.py pkpd_v2.html pkpd_standalone.html
```

The output file works completely offline. Verify by disconnecting from the internet and opening it — if it loads fully, it's self-contained.

---

## Input file format

### RQ file (CSV)

| sample_id | group | RQ |
|---|---|---|
| M01 | 0.3mg IT | 0.621 |
| M02 | 0.3mg IT | 0.583 |
| M03 | aCSF | 0.991 |

- `sample_id` — unique animal identifier, must match MSD file
- `group` — dose level label, used for color coding
- `RQ` — relative quantity from qPCR, normalized to vehicle control (RQ = 1 = no knockdown)

Column names are auto-detected — common variants (e.g. `animal_id`, `relative_quantity`) are recognised automatically.

### MSD file (CSV)

| Sample Name | MSD Signal (ECL) | Known Concentration (ng/g) |
|---|---|---|
| STD-1 | 142 | 0.488 |
| STD-2 | 387 | 1.95 |
| M01 | 4100 | |
| M02 | 3718 | |

- Standards are identified by name (STD, Standard, Cal) or by having a known concentration populated
- Sample rows leave the concentration column blank — these are back-calculated
- Units: ng/g tissue

---

## Model selection guide

| Situation | Recommended model |
|---|---|
| Fewer than 3 dose groups | Linear |
| Data doesn't reach 50% knockdown | Linear — EC50 would be extrapolated |
| Fewer than 8 animals | Linear regardless of AIC |
| Data spans full S-curve | Sigmoid Emax |
| ΔAIC (sigmoid vs linear) < −2 | Sigmoid Emax |

The app fits all three models automatically and recommends the most appropriate one. EC50 and Emax are only reported when the sigmoid model is supported by the data.

---

## Data security

This tool runs entirely in your browser. Your data is never transmitted anywhere — all processing happens locally in JavaScript. You can verify this by disconnecting from the internet before uploading your files; the analysis runs identically.

The only external requests are to load JavaScript libraries from CDN servers on startup (in the default version). These requests carry no data. To eliminate these entirely, use the self-contained version described above.

---

## Limitations

- Designed for terminal tissue collection studies (single timepoint per animal) — not longitudinal PK modelling
- Standard curve fitting assumes log-linear relationship between signal and concentration; non-linear MSD assays may need adjustment
- Not validated for GLP or clinical use — research phase tool only
- Model fitting uses gradient descent; may occasionally find local rather than global optima for sigmoid Emax with very sparse data

---

## Citation

If you use this tool in published work, please cite:

> Akila Ram (2025). PK/PD Correlator for Oligonucleotide Neuroscience Programs [Software]. GitHub. https://github.com/akilar10/pkpd-correlator

A Zenodo DOI will be added here once the repository is deposited.

---

## License

MIT License — free to use, modify, and distribute with attribution.

---

## Contributing

Bug reports and feature requests welcome via GitHub Issues. Pull requests accepted.

Areas where contributions would be most useful:
- Additional delivery route support (IV, subcutaneous)
- Multi-timepoint longitudinal PK
- Protein knockdown as a second PD readout
- NHP-specific standard curve ranges

---

## Contact

[Your name and institutional affiliation]
[Email or LinkedIn]
