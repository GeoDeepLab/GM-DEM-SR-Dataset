# DEMSR Release Quality Report

Dataset root: `GM_DEMSR_Dataset_release`

## Metadata Tables

| table | rows | path_columns | path_issues | missing_files |
| --- | --- | --- | --- | --- |
| main_train | 36000 | 4 | 0 | 0 |
| main_val | 4500 | 4 | 0 | 0 |
| main_test | 4500 | 4 | 0 | 0 |
| additional_regions_test | 4500 | 4 | 0 | 0 |
| same_region_cross_source_test | 4500 | 7 | 0 | 0 |
| additional_region_cross_source_test | 4500 | 7 | 0 | 0 |

## Pair Checks

| table | scale | status | pair_count |
| --- | --- | --- | --- |
| additional_region_cross_source_test | 2 | ok | 9000 |
| additional_region_cross_source_test | 4 | ok | 9000 |
| additional_region_cross_source_test | 6 | ok | 9000 |
| additional_regions_test | 2 | ok | 4500 |
| additional_regions_test | 4 | ok | 4500 |
| additional_regions_test | 6 | ok | 4500 |
| main_test | 2 | ok | 4500 |
| main_test | 4 | ok | 4500 |
| main_test | 6 | ok | 4500 |
| main_train | 2 | ok | 36000 |
| main_train | 4 | ok | 36000 |
| main_train | 6 | ok | 36000 |
| main_val | 2 | ok | 4500 |
| main_val | 4 | ok | 4500 |
| main_val | 6 | ok | 4500 |
| same_region_cross_source_test | 2 | ok | 9000 |
| same_region_cross_source_test | 4 | ok | 9000 |
| same_region_cross_source_test | 6 | ok | 9000 |

## Path Checks

Relative path issues: 0

## GeoTIFF Directory Counts

| directory | file_count |
| --- | --- |
| additional_region_cross_source/test/hr | 4500 |
| additional_region_cross_source/test/lr/aw3d30/x2 | 4500 |
| additional_region_cross_source/test/lr/aw3d30/x4 | 4500 |
| additional_region_cross_source/test/lr/aw3d30/x6 | 4500 |
| additional_region_cross_source/test/lr/nasadem/x2 | 4500 |
| additional_region_cross_source/test/lr/nasadem/x4 | 4500 |
| additional_region_cross_source/test/lr/nasadem/x6 | 4500 |
| additional_regions/test/hr | 4500 |
| additional_regions/test/lr/x2 | 4500 |
| additional_regions/test/lr/x4 | 4500 |
| additional_regions/test/lr/x6 | 4500 |
| main/test/hr | 4500 |
| main/test/lr/x2 | 4500 |
| main/test/lr/x4 | 4500 |
| main/test/lr/x6 | 4500 |
| main/train/hr | 36000 |
| main/train/lr/x2 | 36000 |
| main/train/lr/x4 | 36000 |
| main/train/lr/x6 | 36000 |
| main/val/hr | 4500 |
| main/val/lr/x2 | 4500 |
| main/val/lr/x4 | 4500 |
| main/val/lr/x6 | 4500 |
| same_region_cross_source/test/hr | 4500 |
| same_region_cross_source/test/lr/aw3d30/x2 | 4500 |
| same_region_cross_source/test/lr/aw3d30/x4 | 4500 |
| same_region_cross_source/test/lr/aw3d30/x6 | 4500 |
| same_region_cross_source/test/lr/nasadem/x2 | 4500 |
| same_region_cross_source/test/lr/nasadem/x4 | 4500 |
| same_region_cross_source/test/lr/nasadem/x6 | 4500 |

## Failures

Failure rows: 0