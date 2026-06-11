# matsulakes — Project Memory

## Project Overview

Analysis and writing related to the **Mat-Su Borough Volunteer Lake Monitoring Program (MSBVLMP)**, a discontinued water quality monitoring program in the Matanuska-Susitna Borough, Alaska. The program ran until 2017. This project revisits the data from McMillan (2016) to explore historical conditions and inform potential reinstatement.

Core questions:
- What does the existing dataset reveal about water quality conditions and trends?
- Which monitoring approaches and metrics were most informative?
- What would a reinstated/restructured program look like?
- How can the data be made user-friendly and accessible?

## Key Reference

McMillan, B. (2016). *Mat-Su Borough Volunteer Lake Monitoring Program: Water Quality Assessment*. Located at `other/documents/McMillan_Thesis_Final_April_20163.pdf`.

## Data

The full dataset lives in [Rivulet Research's Google Drive](https://drive.google.com/drive/folders/1y3B4upVhJYAWOhTmYgeo30O0YO3W0ciL?usp=sharing) and is excluded from version control (see `.gitignore`). A local working copy is at `other/input/MatSuBorough_Lake_WQ/`.

### Primary Data Files

| File | Description | Key Columns |
|------|-------------|-------------|
| `lake_profiles_all_46.csv` | Main depth-profile observations for 46 lakes | `lake`, `date`, `year`, `month`, `depth`, `temp`, `conductivity`, `DO`, `pH`, `morph`, `HUC_12`, `HUC_10`, `region`, `Mat_or_Su` |
| `lake_tsi.xlsx` | Trophic State Index data | `Lake`, `date`, `year`, `month`, `phos.ug`, `chlorophyll.a`, `secchi.disk`, `tsi.phos`, `tsi.chl`, `tsi.sd` |
| `lake_46_medians.csv` | Per-lake medians | `lake`, `temp`, `DO`, `sc`, `pH`, `dD`, `d18O` |
| `lake_46_medians_ph_sc_temp_DO_d18_dD.csv` | Medians for additional variables | — |
| `lake_46_mean.csv`, `lake_46_maximum.csv`, `lake_46_minimum.csv` | Per-lake summary stats | — |
| `lake_46_range.csv`, `lake_46_std_dev.csv`, `lake_46_sample_size.csv` | Per-lake summary stats | — |
| `MSBVLMP/lakes_all_variables.xlsx` | Master file with response & predictor variable inventory by lake; messy multi-row headers | — |

### Loading Data

```r
library(tidyverse)
library(readxl)

profiles <- read_csv("other/input/MatSuBorough_Lake_WQ/lake_profiles_all_46.csv")
tsi      <- read_excel("other/input/MatSuBorough_Lake_WQ/lake_tsi.xlsx")
medians  <- read_csv("other/input/MatSuBorough_Lake_WQ/lake_46_medians.csv")
```

### Notes on Data Quality

- `date` in `lake_profiles_all_46.csv` is stored as a character string (e.g., `"8/8/06"`); parse with `lubridate::mdy()`.
- `lake_tsi.xlsx` stores numeric fields (`phos.ug`, `chlorophyll.a`, etc.) as character columns — coerce to numeric after loading.
- `MSBVLMP/lakes_all_variables.xlsx` has a two-row header with merged cells; skip rows or handle manually when reading.

## Directory Structure

```
other/
  agent_context/     # (empty — for AI context files)
  documents/         # PDF reference materials (incl. McMillan 2016)
  input/
    MatSuBorough_Lake_WQ/
      MSBVLMP/       # Raw program files, master Excel, maps
      lake_*.csv     # Processed summary tables
      lake_tsi.xlsx
  output/            # (empty — for analysis outputs)
```
