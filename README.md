# California Wildfire Eaton Fire

[中文说明](./README_CN.md)

This repository contains the code, documentation, and folder skeletons for a reproducible post-fire image analysis workflow built around the final Eaton Fire datasets used in this project.

The GitHub repository is intentionally lightweight:

- code and documentation are versioned
- raw images, raster tiles, and most generated outputs stay local
- directory skeletons are preserved so the workflow can be rebuilt on another machine

## Final datasets

### 1. `Eaton_Fire/`

The canonical labeled street-view dataset.

- organized by six damage classes
- includes class-level point CSV exports
- includes the master attachment index and summary CSV
- final scale: 18,428 points and 19,780 image attachments

Detailed notes: [`Eaton_Fire/README.md`](./Eaton_Fire/README.md)

### 2. `Altadena_Images/`

The paired sample-organized dataset derived from the Eaton Fire attachment index.

- one sample folder per attachment record
- each sample can contain `street_view.jpg` and `remote_sensing.jpg`
- includes pairing indexes and download logs
- final scale: 19,780 sample folders, 19,746 complete pairs, 91 GeoTIFF tiles

Detailed notes: [`Altadena_Images/README.md`](./Altadena_Images/README.md)

## Dataset summary

The following totals come from the final `Eaton_Fire_summary.csv` snapshot.

| Damage category | Points | Attachments |
| --- | ---: | ---: |
| Destroyed (>50%) | 9,419 | 9,490 |
| Major (26-50%) | 70 | 155 |
| Minor (10-25%) | 148 | 312 |
| Affected (1-9%) | 858 | 1,809 |
| Inaccessible | 40 | 33 |
| No Damage | 7,893 | 7,981 |
| Total | 18,428 | 19,780 |

`points` are inspection locations, while `attachments` are image records linked to those locations, so the two counts are not expected to match.

## Repository layout

```text
.
|-- README.md
|-- README_CN.md
|-- requirements.txt
|-- Eaton_Fire/
|-- Altadena_Images/
|-- scripts/
|   |-- data_prep/
|   |-- features/
|   `-- visualization/
|-- notebooks/
|-- data/
`-- outputs/
```

## Main scripts

- `scripts/data_prep/parse_metadata.py`: parse labeled Eaton Fire files and index paired sample folders
- `scripts/data_prep/train_val_test_split.py`: create supervised train, validation, and test splits
- `scripts/features/extract_clip_embeddings.py`: extract CLIP embeddings for labeled and paired samples
- `scripts/features/sample_raster_values.py`: sample raster values at street-view point locations
- `scripts/visualization/map_damage_points.py`: build an interactive damage map
- `scripts/visualization/sample_grid.py`: export sample image grids for QA

## Typical workflow

### 1. Install dependencies

```bash
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
pip install -r Altadena_Images/requirements.txt
```

### 2. Parse metadata

```bash
python scripts/data_prep/parse_metadata.py --root .
```

Outputs:

- `data/eaton_fire_metadata.csv`
- `data/eaton_fire_points.geojson`
- `data/altadena_unlabeled_index.csv`

### 3. Build train/validation/test splits

```bash
python scripts/data_prep/train_val_test_split.py --root .
```

### 4. Extract CLIP features

```bash
python scripts/features/extract_clip_embeddings.py --root . --split labeled
python scripts/features/extract_clip_embeddings.py --root . --split unlabeled
```

### 5. Add raster features when needed

```bash
python scripts/features/sample_raster_values.py --root . --raster /path/to/dNBR.tif --band_name dNBR
```

### 6. Generate QA visualizations

```bash
python scripts/visualization/map_damage_points.py --root .
python scripts/visualization/sample_grid.py --root . --n 6
```

## Versioning policy

Versioned in GitHub:

- source code
- notebooks
- documentation
- folder skeletons and `.gitkeep` placeholders

Kept local and ignored by Git:

- raw street-view images
- raster tiles and pairing outputs
- generated feature arrays and metadata tables
- generated maps, figures, and reports
- virtual environments and archive files

## Notes

- `Eaton_Fire/` is the authoritative labeled dataset for this project.
- `Altadena_Images/` is the authoritative paired sample dataset built from the same attachment index.
- The current code uses the name `unlabeled` for the paired split, but that is only a workflow label inside scripts.
