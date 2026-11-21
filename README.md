# County-Level Radiologist Workforce Density, Cancer Mortality, and the Modeled Impact of AI-Augmented Diagnostic Capacity

[![DOI](https://zenodo.org/badge/1286286234.svg)](https://doi.org/10.5281/zenodo.24428106)

Code and county-level data supporting the manuscript:

> Zhao, Y. *County-Level Radiologist Workforce Density, Cancer Mortality, and the Modeled Impact of AI-Augmented Diagnostic Capacity: A U.S. Ecological Analysis.* Preprint: Research Square, https://doi.org/10.21203/rs.3.rs-11324025/v1

This repository contains the Python (Jupyter) notebooks used for data processing, regression analysis, spatial error modeling, AI-equivalent capacity scenario modeling, and figure generation, together with the intermediate and final analytic datasets and the result files reported in the manuscript.

---

## Repository structure

Two parallel pipelines are provided. They share the same script structure and numbering and differ only in the NPPES workforce snapshot and HUD crosswalk release used.

| Pipeline | NPPES snapshot | HUD ZIP–County crosswalk | Role in manuscript |
|---|---|---|---|
| **2021** | July 2021 (NBER archive) | 2021 Q2 | Primary analysis (M1, M2, S1–S6, S8–S11b, spatial models, AI scenarios, figures) |
| **2026** | March 2026 (CMS) | 2025 Q4 | Sensitivity analysis S7 only |

### 2021 pipeline (primary analysis)

| Script | Input | Output |
|---|---|---|
| `0. npi_filter_2021.ipynb` | `npi20217.parquet` | `radiologist_pathologist_filtered_2021_07.csv` |
| `1. county build 2021.ipynb` | `radiologist_pathologist_filtered_2021_07.csv`, `ZIP-COUNTY_062021.xlsx` | `county_radiologist_pathologist_counts_2021_07.csv` |
| `2. census merge 2021.ipynb` | county counts (above), `co-est2024-alldata.csv`, `death_2023.csv`, `incd_uscs_2022.csv`, `Ruralurbancontinuumcodes2023.csv`, `analytic_data2023_0.csv` | `analysis_dataset_2021_07.csv` |
| `3. ols regression 2021.ipynb` | `analysis_dataset_2021_07.csv`, `analysis_dataset_2026_fixed.csv` (S7), `death_2023.csv`, `death_65.csv`, `cc-est2024-agesex-all.csv` (S11b) | `ols_results_2021_07.csv` |
| `4. ai scenario 2021.ipynb` | `analysis_dataset_2021_07.csv` | `ai_scenario_results_2021_07.csv`, `ai_scenario_gradient_2021_07.png` (Figure 2) |
| `5. spatial regression 2021.ipynb` | `analysis_dataset_2021_07.csv`, TIGER/Line 2021 county shapefile | `spatial_regression_results_2021_07.csv` |
| `6. county map 2021.ipynb` | `analysis_dataset_2021_07.csv`, TIGER/Line 2021 county shapefile | `map1_radiologist_density_2021_07.png` (Figure 1), `map2_ai_mortality_reduction_2021_07.png` (Figure 3) |

### 2026 pipeline (sensitivity analysis S7)

| Script | Input | Output |
|---|---|---|
| `0. npi_filter 2026.ipynb` | `npidata_pfile_20050523-20260308.csv` | `radiologist_pathologist_filtered_2026_03.csv` |
| `1. county build 2026.ipynb` | `radiologist_pathologist_filtered_2026_03.csv`, `ZIP_COUNTY_122025.xlsx` | `county_radiologist_pathologist_counts_2026_fixed.csv` |
| `2. census merge 2026.ipynb` | county counts (above) and the same Census, NCI, USDA, and County Health Rankings files used in the 2021 pipeline | `analysis_dataset_2026_fixed.csv` |

`analysis_dataset_2026_fixed.csv` is read by `3. ols regression 2021.ipynb` to estimate S7. No other analyses are run on the 2026 pipeline.

### Data files included in this repository

| File | Description |
|---|---|
| `radiologist_pathologist_filtered_2021_07.csv` | Provider-level records (diagnostic radiologists and pathologists) extracted from the July 2021 NPPES snapshot |
| `radiologist_pathologist_filtered_2026_03.csv` | Provider-level records extracted from the March 2026 NPPES file |
| `analysis_dataset_2021_07.csv` | County-level analytic dataset, primary analysis (N = 3,135 counties; 3,073 with complete mortality and covariate data) |
| `analysis_dataset_2026_fixed.csv` | County-level analytic dataset built from the 2026 snapshot (S7) |
| `ols_results_2021_07.csv` | Coefficients, standard errors, 95% CIs, p-values, R², and N for M1, M2, and S1–S11b |
| `spatial_regression_results_2021_07.csv` | Moran's I, Lagrange multiplier diagnostics, spatial error model (Queen and k-nearest-neighbor weights) |
| `ai_scenario_results_2021_07.csv` | Modeled mortality differentials by shortage subgroup and AI-equivalent capacity scenario |

---

## Raw data sources (not included; publicly available)

Raw source files are not redistributed in this repository because of their size. All are publicly available:

| File used in scripts | Source |
|---|---|
| `npi20217.parquet` | NPPES NPI file, July 2021 snapshot, National Bureau of Economic Research archive: https://data.nber.org/npi/2021/csv/ |
| `npidata_pfile_20050523-20260308.csv` | NPPES Full Replacement NPI file, March 2026, Centers for Medicare & Medicaid Services: https://download.cms.gov/nppes/NPI_Files.html |
| `ZIP-COUNTY_062021.xlsx`, `ZIP_COUNTY_122025.xlsx` | HUD USPS ZIP Code Crosswalk Files (2021 Q2 and 2025 Q4): https://www.huduser.gov/portal/datasets/usps_crosswalk.html |
| `co-est2024-alldata.csv` | U.S. Census Bureau county population estimates (CO-EST2024-ALLDATA): https://www.census.gov/data/tables/time-series/demo/popest/2020s-counties-total.html |
| `cc-est2024-agesex-all.csv` | U.S. Census Bureau county population estimates by age and sex (CC-EST2024-AGESEX): https://www.census.gov/data/tables/time-series/demo/popest/2020s-counties-detail.html |
| `death_2023.csv`, `death_65.csv`, `incd_uscs_2022.csv` | NCI State Cancer Profiles (all-ages and ages-65+ mortality, 2019–2023; incidence, 2018–2022): https://statecancerprofiles.cancer.gov |
| `Ruralurbancontinuumcodes2023.csv` | USDA ERS Rural-Urban Continuum Codes, 2023: https://www.ers.usda.gov/data-products/rural-urban-continuum-codes |
| `analytic_data2023_0.csv` | County Health Rankings & Roadmaps, 2023: https://www.countyhealthrankings.org/health-data/methodology-and-sources/data-documentation/national-data-documentation-2010-2023 |
| `tl_2021_us_county/tl_2021_us_county.shp` | U.S. Census Bureau TIGER/Line 2021 county shapefile: https://www.census.gov/cgi-bin/geo/shapefiles/index.php?year=2021&layergroup=Counties+%28and+equivalent%29 |

---

## How to reproduce

1. Download the raw source files listed above and place them in the same folder as the notebooks (the shapefile in a subfolder named `tl_2021_us_county/`).
2. Run the 2021 pipeline notebooks in order: 0 → 1 → 2.
3. Run the 2026 pipeline notebooks in order: 0 → 1 → 2 (required for S7).
4. Run the remaining 2021 notebooks: 3 → 4 → 5 → 6.

Steps 0–2 can be skipped by using the provider-level and analytic datasets included in this repository. Notebooks 3–6 can be run directly from `analysis_dataset_2021_07.csv` and `analysis_dataset_2026_fixed.csv`.

### Software

Analyses were run in Python (Anaconda distribution) using pandas 2.2.2, NumPy 1.26.4, SciPy 1.13.1, and statsmodels 0.14.6, together with geopandas, libpysal, spreg, matplotlib, and pyarrow (for reading the NBER parquet file).

---

## Notes

- **Provider identification.** Individual providers (Entity Type Code = 1) with an active NPI, a primary practice location in the 50 states or the District of Columbia, and a diagnostic radiology (2085R0202X) or pathology (207ZP0102X, 207ZP0101X, 207ZC0500X) taxonomy code in any of the 15 taxonomy fields were retained. No credential-based filtering was applied.
- **Dual-specialty providers.** Providers holding both radiologist and pathologist codes were assigned by primary taxonomy designation where resolvable and otherwise excluded (Script 1 of each pipeline).
- **Matched-taxonomy rule.** In `0. npi_filter_2021.ipynb`, the `matched_taxonomy` field prioritizes a target code flagged as primary (switch = Y); in `0. npi_filter 2026.ipynb`, the first matching target code is used. Dual-specialty providers are re-resolved by primary designation in Script 1 of both pipelines, so this difference affects only the primary-designation flag for a small number of records.
- **ZIP-to-county allocation.** Providers in ZIP codes spanning multiple counties were allocated proportionally using HUD business-address ratios, with residential- and other-address ratios as sequential fallbacks. County identity was determined by FIPS code alone.
- **Connecticut.** Connecticut was excluded because NCI State Cancer Profiles use legacy county FIPS codes, whereas Census CO-EST2024 uses the post-2022 planning-region geography.
- **Scenario values.** AI-equivalent capacity scenario values (25th percentile, median, and 75th percentile of nonzero county density) and the shortage cutoff are computed within Scripts 4 and 6 from the loaded dataset rather than hardcoded. Script 4 uses the analytic sample (n = 3,073); Script 6 uses the full county file (n = 3,135) for mapping.

---

## Contact

Yueming Zhao — yz767@rutgers.edu
