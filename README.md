# Soil Texture from Spectra

GeoAI lab: predicting the three soil texture fractions — sand, silt and clay — from VisNIR
reflectance spectra, and measuring how much of that ability survives being reduced to what a
satellite would actually see.

The assignment is [`lab.pdf`](lab.pdf). The submission is
[`soil_texture_lab.ipynb`](soil_texture_lab.ipynb) plus `report.pdf`.

## Data

Three CSVs from the [Open Soil Spectroscopy Library](https://soilspectroscopy.github.io),
40 535 rows each, aligned row-for-row on `id`. **They are not in this repository** —
`ossl_tp_dataset.csv` is 133 MB, over GitHub's 100 MB limit. Place them beside the notebook:

| File | Size | Contents |
|---|---|---|
| `ossl_tp_dataset.csv` | 133 MB | 12 metadata columns + `L400`…`L2500` reflectance every 5 nm |
| `ossl_tp_soillab.csv` | 9.6 MB | 58 laboratory properties (organic carbon, pH, CEC, …) |
| `ossl_tp_soilsite.csv` | 26 MB | 36 site and provenance columns |

One row is one soil sample analysed in a laboratory, with its spectrum. The three programmes
present — LUCAS.SSL (22 171), KSSL.SSL (14 728), ICRAF.ISRIC (3 636) — each used a different
instrument, in a different region, at different depths.

## Running it

```bash
pip install pandas numpy scikit-learn matplotlib pyarrow
jupyter lab soil_texture_lab.ipynb      # then Run All
```

The notebook is self-contained: it imports nothing local, reads the three CSVs directly and
writes its figures to `figs/` and its tables to `tables/`. Runtime is a few minutes.

## Layout

```
soil_texture_lab.ipynb   the submission — sections 0-6, every cell labelled by task number
lab.pdf                  the assignment sheet
figs/                    generated figures, one per task (fig_1_15.png = task 1.15)
tables/                  generated tables  (table_1_2.csv = task 1.2)
report/                  the 4-page report source, rendered to report.pdf
```

## Two things worth knowing about this dataset

- **`L400`–`L450` is missing for all 22 171 LUCAS rows** — its XDS instrument starts at
  455 nm. Structural, not random, so all modelling uses the 410-column 455–2500 nm grid.
- **`programme` is perfectly confounded with instrument, region, depth regime and albedo.**
  Leaving one programme out of training varies all five at once, so the resulting drop in
  score cannot be attributed to any single cause.
