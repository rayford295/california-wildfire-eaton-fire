# California Wildfire Eaton Fire

This repository documents and supports a reproducible workflow around the final local Eaton Fire datasets used for street-view damage analysis and street-view plus remote-sensing pairing in Altadena, California.

The project centers on two related datasets:

1. `Eaton_Fire/`: the final class-organized labeled street-view dataset.
2. `Altadena_Images/`: the final sample-organized paired dataset created from the Eaton Fire attachment index, where each sample folder can contain a downloaded street-view image and a matched remote-sensing crop.

## Correction to earlier descriptions

Earlier drafts of the repository documentation described `Altadena_Images/` as an "unlabeled Altadena imagery" folder. That was not the final structure.

The final local dataset is:

- `Eaton_Fire/` for the canonical labeled export and class-organized attachment folders
- `Altadena_Images/` for the attachment-level paired dataset built from the same Eaton Fire attachment index

In other words, `Altadena_Images/` is not a separate fire dataset and not a second independent label source. It is a derived, sample-organized view of the Eaton Fire attachment records with matched remote-sensing imagery.

## Final dataset summary

The table below comes from the final `Eaton_Fire_summary.csv` snapshot.

| Damage category | Points | Attachments |
| --- | ---: | ---: |
| Destroyed (>50%) | 9,419 | 9,490 |
| Major (26-50%) | 70 | 155 |
| Minor (10-25%) | 148 | 312 |
| Affected (1-9%) | 858 | 1,809 |
| Inaccessible | 40 | 33 |
| No Damage | 7,893 | 7,981 |
| Total | 18,428 | 19,780 |

These counts matter because the project works at two levels:

- `points` are damage-inspection locations
- `attachments` are image records linked to those points

One point can have multiple attachments, so point counts and image counts are not expected to match.

## Dataset 1: `Eaton_Fire/`

`Eaton_Fire/` is the canonical labeled dataset. It stores the six damage categories as separate folders and preserves the original tabular exports that describe points and attachment metadata.

### Final contents

```text
Eaton_Fire/
|-- Affected_1-9_/
|   `-- attachments/
|-- Destroyed_50_/
|   `-- attachments/
|-- Inaccessible/
|   `-- attachments/
|-- Major_26-50_/
|   `-- attachments/
|-- Minor_10-25_/
|   `-- attachments/
|-- No_Damage/
|   `-- attachments/
|-- Eaton_Fire_Affected_1-9__points.csv
|-- Eaton_Fire_Destroyed_50__points.csv
|-- Eaton_Fire_Inaccessible_points.csv
|-- Eaton_Fire_Major_26-50__points.csv
|-- Eaton_Fire_Minor_10-25__points.csv
|-- Eaton_Fire_No_Damage_points.csv
|-- Eaton_Fire_attachments_index.csv
`-- Eaton_Fire_summary.csv
```

### Per-class image availability

The final attachment folder counts on disk are:

| Folder | Attachment image files |
| --- | ---: |
| `Destroyed_50_/attachments/` | 9,490 |
| `No_Damage/attachments/` | 7,981 |
| `Affected_1-9_/attachments/` | 1,809 |
| `Minor_10-25_/attachments/` | 312 |
| `Major_26-50_/attachments/` | 155 |
| `Inaccessible/attachments/` | 33 |
| Total | 19,780 |

The `Inaccessible` category is the one visible mismatch between point rows and downloaded attachment images: 40 points but 33 image attachments.

### What each file represents

- `Eaton_Fire_summary.csv`: class-level summary with point and attachment counts
- `Eaton_Fire_*_points.csv`: point-level inspection exports for each damage category
- `Eaton_Fire_attachments_index.csv`: attachment-level index covering the full 19,780 image records
- `attachments/` folders: downloaded street-view image files grouped by damage class

### Point CSV schema

The class-specific point CSVs use columns such as:

- `OBJECTID`: source feature identifier
- `GLOBALID`: unique record identifier from the source system
- `DAMAGE`: human-readable damage label
- `STRUCTURETYPE`: structure description
- `lat`, `lon`: point coordinates
- `_fire`: fire name
- `_category`: category used to organize the export

### Attachment index schema

`Eaton_Fire_attachments_index.csv` uses the following core columns:

- `fire`
- `category`
- `objectid`
- `attachment_id`
- `contentType`
- `name`
- `lat`
- `lon`
- `file`
- `url`

Important note:

- the `file` column is a source-time path reference and should be treated as provenance metadata, not as a portable path on every machine
- the `url` column points to the original ArcGIS attachment endpoint for the street-view image

### Street-view filename convention

Downloaded image filenames follow a coordinate plus ID pattern:

```text
{latitude}_{longitude}_OID{objectid}_A{attachment_id}.jpg
```

Example:

```text
34.204252_-118.131625_OID10_A10.jpg
```

This naming convention lets the repository parse coordinates and source identifiers directly from filenames.

## Dataset 2: `Altadena_Images/`

`Altadena_Images/` is the final paired dataset derived from the Eaton Fire attachment index. It organizes one sample per attachment record and stores street-view plus remote-sensing outputs in a sample-based structure.

### Final contents

```text
Altadena_Images/
|-- Altadena_Images/
|   |-- *.tif
|   `-- *.tif.aux.xml
|-- Eaton_Fire_attachments_index.csv
|-- Eaton_Fire_attachments_index_output/
|   |-- dataset/
|   |   |-- sample_00001/
|   |   |   |-- street_view.jpg
|   |   |   `-- remote_sensing.jpg
|   |   `-- ...
|   |-- dataset_index.csv
|   |-- dataset_index.xlsx
|   `-- download_report.csv
|-- download_attachment_index.py
|-- match_remote_sensing.py
`-- requirements.txt
```

### Final paired-dataset counts

The final on-disk counts are:

| Item | Count |
| --- | ---: |
| Sample folders under `dataset/` | 19,780 |
| `street_view.jpg` files | 19,772 |
| `remote_sensing.jpg` files | 19,754 |
| Complete street-view plus remote-sensing pairs | 19,746 |
| Incomplete sample folders | 34 |
| GeoTIFF tiles in `Altadena_Images/Altadena_Images/` | 91 |
| `.aux.xml` sidecar files | 91 |

The incompleteness breakdown is:

- 26 samples contain `street_view.jpg` but do not yet have `remote_sensing.jpg`
- 8 samples contain `remote_sensing.jpg` but do not have `street_view.jpg`
- 0 sample folders are missing both files

### What this dataset is for

This dataset provides an attachment-level multimodal view of the Eaton Fire records:

- `street_view.jpg` is the downloaded inspection image
- `remote_sensing.jpg` is a matched crop from the remote-sensing tile covering the same location
- each `sample_xxxxx` folder corresponds to one attachment record from `Eaton_Fire_attachments_index.csv`

Because the sample count is exactly 19,780, `Altadena_Images/` should be understood as a paired derivative of the full attachment index rather than a new dataset with different records.

### Key index files

- `dataset_index.csv`: canonical table for the paired sample dataset
- `dataset_index.xlsx`: spreadsheet version of the same index
- `download_report.csv`: street-view download log and status report

### `dataset_index.csv` schema

The paired index includes these key fields:

- `pair_id`
- `sample_folder`
- `street_view_filename`
- `street_view_relative_path`
- `street_view_url`
- `remote_sensing_filename`
- `remote_sensing_relative_path`
- `latitude`
- `longitude`
- `fire`
- `category`
- `objectid`
- `attachment_id`
- `source_sheet`
- `source_row_number`
- `download_status`
- `bytes_written`
- `notes`
- `remote_tile_filename`
- `remote_tile_path`
- `remote_tile_aux_xml_path`
- `remote_match_status`
- `remote_match_notes`
- `remote_pixel_x`
- `remote_pixel_y`
- `remote_crop_box`
- `remote_distance_to_coverage_m`

Important note:

- `remote_tile_path` and related absolute path fields capture the path used when the pairing dataset was generated, so they should be interpreted as provenance metadata rather than guaranteed current-machine paths

### `download_report.csv` schema

The download report includes:

- `sample_id`
- `row_number`
- `source_sheet`
- `status`
- `bytes_written`
- `relative_path`
- `url`
- `message`

## How the repository code maps to the final datasets

The scripts in this repository operate on the two final datasets above:

- `scripts/data_prep/parse_metadata.py` parses the labeled class folders inside `Eaton_Fire/`
- the same script also indexes sample folders under `Altadena_Images/Eaton_Fire_attachments_index_output/dataset/`
- `scripts/data_prep/train_val_test_split.py` creates supervised splits from the labeled `Eaton_Fire/` metadata
- `scripts/features/extract_clip_embeddings.py` extracts features from the labeled dataset and from the sample-organized paired dataset
- `scripts/features/sample_raster_values.py` appends raster values to the labeled point metadata
- `scripts/visualization/` creates QA outputs such as maps and image grids

One naming detail is worth calling out:

- inside the current scripts, the sample-organized `Altadena_Images/` dataset is still referred to as the `unlabeled` split
- that naming is a workflow convenience in code, not a claim that source metadata is absent from the final dataset

## Repository layout

```text
.
|-- README.md
|-- .gitignore
|-- requirements.txt
|-- notebooks/
|   `-- 01_dataset_exploration.ipynb
|-- scripts/
|   |-- data_prep/
|   |   |-- parse_metadata.py
|   |   `-- train_val_test_split.py
|   |-- features/
|   |   |-- extract_clip_embeddings.py
|   |   `-- sample_raster_values.py
|   `-- visualization/
|       |-- map_damage_points.py
|       `-- sample_grid.py
|-- Eaton_Fire/
|-- Altadena_Images/
|-- data/
`-- outputs/
```

## Setup

Create a virtual environment and install the repository dependencies:

```bash
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

If you use the helper scripts inside `Altadena_Images/`, install their extra dependencies as well:

```bash
pip install -r Altadena_Images/requirements.txt
```

## Typical workflow

### 1. Parse metadata

```bash
python scripts/data_prep/parse_metadata.py --root .
```

Generated files:

- `data/eaton_fire_metadata.csv`
- `data/eaton_fire_points.geojson`
- `data/altadena_unlabeled_index.csv`

### 2. Build train, validation, and test splits

```bash
python scripts/data_prep/train_val_test_split.py --root .
```

Generated files:

- `data/splits/train.csv`
- `data/splits/val.csv`
- `data/splits/test.csv`

### 3. Extract CLIP embeddings

```bash
python scripts/features/extract_clip_embeddings.py --root . --split labeled
python scripts/features/extract_clip_embeddings.py --root . --split unlabeled
```

Generated files:

- `data/features/clip_embeddings_labeled.npy`
- `data/features/clip_embeddings_labeled_index.csv`
- `data/features/clip_embeddings_unlabeled.npy`
- `data/features/clip_embeddings_unlabeled_index.csv`

### 4. Sample raster values for the labeled metadata

```bash
python scripts/features/sample_raster_values.py --root . --raster /path/to/dNBR.tif --band_name dNBR
```

Generated file:

- `data/eaton_fire_with_rs_features.csv`

### 5. Build QA outputs

```bash
python scripts/visualization/map_damage_points.py --root .
python scripts/visualization/sample_grid.py --root . --n 6
```

Generated files:

- `outputs/damage_map.html`
- `outputs/sample_grid.png`

## Versioning policy

This repository is intended to version:

- code
- notebooks
- documentation
- folder structure

This repository is not intended to version:

- the full image datasets
- full raster tiles
- generated feature arrays
- generated reports and maps
- local virtual environments

## Notes

- Run `parse_metadata.py` before any split, feature extraction, or visualization step.
- The CLIP feature workflow uses `openai/clip-vit-base-patch32` through Hugging Face Transformers.
- `Eaton_Fire/` is the authoritative labeled export.
- `Altadena_Images/` is the authoritative paired sample dataset built from the Eaton Fire attachment index.
