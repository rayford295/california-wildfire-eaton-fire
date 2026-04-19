# Hurricane Ian Cross-View Dataset

[中文说明](./README_CN.md)

This directory is reserved for the final local Hurricane Ian cross-view dataset included in this repository.

The local dataset is pair-based and contains satellite-view plus street-view image pairs, split files, and caption data.

## Local structure

```text
IAN_hurricane/
|-- images/
|   |-- 000000_sat.png
|   |-- 000000_svi.png
|   `-- ...
|-- pairs.csv
|-- captions.csv
|-- train_split.csv
`-- test_split.csv
```

## Local counts

The current local snapshot contains:

- 4,121 paired records in `pairs.csv`
- 4,121 caption rows in `captions.csv`
- 3,821 training rows in `train_split.csv`
- 300 test rows in `test_split.csv`
- 8,242 image files in `images/`
- 4,121 `*_sat.png` files
- 4,121 `*_svi.png` files

Severity distribution in `pairs.csv`:

| Severity | Count |
| --- | ---: |
| `0_MinorDamage` | 1,407 |
| `1_ModerateDamage` | 1,538 |
| `2_SevereDamage` | 1,176 |
| Total | 4,121 |

## Schema

`pairs.csv`, `train_split.csv`, and `test_split.csv` use:

- `sat_path`
- `svi_path`
- `severity`

`captions.csv` adds:

- `description`

Important note:

- the provided local CSV files do not include latitude or longitude fields
- the current path fields should be treated as file references for this dataset snapshot, not as georeferencing metadata

## Source and attribution

This dataset should be attributed to:

Li, H., Deuser, F., Yin, W., Luo, X., Walther, P., Mai, G., Huang, W., and Werner, M. (2025). *Cross-view geolocalization and disaster mapping with street-view and VHR satellite imagery: A case study of Hurricane IAN*. *ISPRS Journal of Photogrammetry and Remote Sensing*, 220, 841-854. [https://doi.org/10.1016/j.isprsjprs.2025.01.003](https://doi.org/10.1016/j.isprsjprs.2025.01.003)

Based on the publication metadata, the paper introduces a novel Hurricane Ian cross-view dataset named `CVIAN`.

## Repository note

This repository tracks the documentation and folder skeleton for `IAN_hurricane/`, while the actual images and local CSV files remain ignored by Git.
