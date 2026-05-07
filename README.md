# Global Mountain DEM Super-Resolution Dataset

## What This Dataset Is

The Global Mountain DEM Super-Resolution dataset is a geospatial benchmark for learning and evaluating digital elevation model super-resolution over mountainous terrain.

Each sample is a georeferenced GeoTIFF patch pair:

- HR: high-resolution target DEM patch.
- LR: lower-resolution input DEM patch.

The dataset focuses on mountain regions, where terrain is steep, high-frequency, and difficult for DEM super-resolution methods.

## Intended Uses

The dataset can be used for:

- DEM super-resolution model training.
- Cross-source generalization experiments.
- Evaluation of terrain-aware SR methods.
- Robustness tests across mountain systems and additional held-out regions.
- Geospatial deep learning workflows that require georeferenced raster patches.

## Public Release Directory

The public release is stored in:

```text
GM_DEMSR_Dataset_release/
```

All metadata paths are relative to this directory.

## Dataset Tasks

### 1. Main Super-Resolution Task

Path:

```text
main/
```

Splits:

```text
main/train/
main/val/
main/test/
```

Each split contains:

```text
hr/
lr/x2/
lr/x4/
lr/x6/
```

Meaning:

- HR is the 30 m target DEM.
- x2 LR is 60 m.
- x4 LR is 120 m.
- x6 LR is 180 m.

This is the standard supervised DEM SR task.

### 2. Same-Region Cross-Source Task

Path:

```text
same_region_cross_source/test/
```

Structure:

```text
hr/
lr/nasadem/x2/
lr/nasadem/x4/
lr/nasadem/x6/
lr/aw3d30/x2/
lr/aw3d30/x4/
lr/aw3d30/x6/
```

Meaning:

- HR target uses the same 30 m target grid as the main test regions.
- LR input comes from another DEM source, NASADEM or AW3D30.
- This tests whether a model trained on one degradation/source condition generalizes to other DEM sources over the same test regions.

### 3. Additional Regions Task

Path:

```text
additional_regions/test/
```

Structure:

```text
hr/
lr/x2/
lr/x4/
lr/x6/
```

Meaning:

- Additional held-out mountain regions.
- Uses the same degradation-style LR setup as the main task.
- Intended for geographic generalization tests.

### 4. Additional-Region Cross-Source Task

Path:

```text
additional_region_cross_source/test/
```

Structure:

```text
hr/
lr/nasadem/x2/
lr/nasadem/x4/
lr/nasadem/x6/
lr/aw3d30/x2/
lr/aw3d30/x4/
lr/aw3d30/x6/
```

Meaning:

- Additional held-out regions.
- Cross-source LR from NASADEM and AW3D30.
- Intended for cross-source and cross-region robustness evaluation.

## Patch Size and Resolution

All HR patches:

```text
96 x 96 pixels at 30 m
```

LR patches:

```text
x2: 48 x 48 pixels at 60 m
x4: 24 x 24 pixels at 120 m
x6: 16 x 16 pixels at 180 m
```

Each LR patch has the same geographic bounds as its paired HR patch.

## Metadata

Metadata CSVs are in:

```text
metadata/
```

Important files:

```text
metadata/main_train.csv
metadata/main_val.csv
metadata/main_test.csv
metadata/additional_regions_test.csv
metadata/same_region_cross_source_test.csv
metadata/additional_region_cross_source_test.csv
metadata/all_tasks.csv
```

Main task CSV path columns:

```text
hr_path
lr_x2_path
lr_x4_path
lr_x6_path
```

Cross-source CSV path columns:

```text
hr_path
lr_nasadem_x2_path
lr_nasadem_x4_path
lr_nasadem_x6_path
lr_aw3d30_x2_path
lr_aw3d30_x4_path
lr_aw3d30_x6_path
```

Example row path:

```text
main/train/hr/core_train_000001.tif
main/train/lr/x2/core_train_000001.tif
```

## Data Sources

The dataset was built from publicly available DEM sources downloaded via OpenTopography:

- COP30: primary 30 m HR reference DEM.
- NASADEM: cross-source DEM.
- AW3D30: cross-source DEM.
- COP90 and SRTMGL3: native-LR application sources in the broader project workflow.

NASADEM and AW3D30 were converted to EGM2008 before cross-source alignment.

## Processing Summary

The processing workflow is:

1. Generate mountain block regions from the GMBA mountain inventory.
2. Build DEM download plans for each source and task.
3. Download DEMs through OpenTopography.
4. Convert NASADEM and AW3D30 vertical datum to EGM2008.
5. Build COP30 30 m UTM reference grids.
6. Align NASADEM and AW3D30 to COP30 reference grids.
7. Generate 96 x 96 HR patch candidates.
8. Sample patches by mountain and original block using a fair round-robin strategy.
9. Generate HR/LR GeoTIFF patch pairs at x2, x4, and x6.
10. Reorganize files into release layout with relative-path metadata.
11. Run release QA.

## Cross-Source Alignment Note

Cross-source aligned rasters only contain values where NASADEM or AW3D30 source data were downloaded.

This is expected:

- test regions with downloaded cross-source DEMs have values;
- train/val areas without cross-source downloads remain nodata.

The final release was checked at the patch level, so every listed HR/LR pair in the metadata has valid data and matching geospatial bounds.

## Quality Report

The release QA report is in:

```text
quality_report/
```

Main report:

```text
quality_report/quality_report.md
```

QA summary:

```text
Metadata tables: 6
Pair checks: 202500
Failures: 0
Path issues: 0
```

The QA checked:

- all metadata paths are relative;
- all referenced files exist;
- HR/LR pairs are complete;
- HR patch shape is 96 x 96;
- LR patch shapes match x2/x4/x6;
- CRS matches between HR and LR;
- HR and LR geographic bounds match;
- LR pixel size is exactly 2/4/6 times HR pixel size;
- nodata ratio is below threshold;
- elevation ranges are plausible.

## Loading Example

Python example:

```python
from pathlib import Path
import pandas as pd
import rasterio

root = Path("GM_DEMSR_Dataset_release")
df = pd.read_csv(root / "metadata" / "main_train.csv")

row = df.iloc[0]
hr_path = root / row["hr_path"]
lr_path = root / row["lr_x2_path"]

with rasterio.open(hr_path) as hr_src:
    hr = hr_src.read(1)

with rasterio.open(lr_path) as lr_src:
    lr = lr_src.read(1)
```

For cross-source evaluation:

```python
df = pd.read_csv(root / "metadata" / "same_region_cross_source_test.csv")
row = df.iloc[0]

hr_path = root / row["hr_path"]
nasadem_lr = root / row["lr_nasadem_x2_path"]
aw3d30_lr = root / row["lr_aw3d30_x2_path"]
```

## Recommended Citation and License

### Dataset License

The GM-DEM-SR dataset is released under the Creative Commons Attribution 4.0 International License (CC BY 4.0), unless otherwise noted.

Users are free to share and adapt this dataset for research, education, and commercial or non-commercial applications, provided that appropriate credit is given to the GM-DEM-SR dataset and the original elevation data providers.

This dataset contains derived DEM patches generated from multiple open elevation data products. The original elevation data remain subject to the licenses and terms of use of their respective providers.

### Source Data Acknowledgement

The high-resolution reference DEM patches are derived from Copernicus DEM GLO-30. Copernicus DEM GLO-30 and GLO-90 are available worldwide under a free license, and users are requested to cite the Copernicus DEM DOI when using the data.

Cross-source evaluation subsets are derived from NASADEM and AW3D30. NASADEM data distributed by LP DAAC are public domain / CC0. AW3D30 is provided by JAXA and can be used free of charge under the JAXA terms of use; users should acknowledge JAXA as the original data provider.

DEM data were accessed through OpenTopography. Users should also acknowledge OpenTopography as the data access platform.

