This directory represents the final labeled Eaton Fire dataset used by the repository.

It contains:

- six class-organized `attachments/` folders
- six class-specific point CSV exports
- one global attachment index file
- one summary CSV

Final summary from `Eaton_Fire_summary.csv`:

| Damage category | Points | Attachments |
| --- | ---: | ---: |
| Destroyed (>50%) | 9,419 | 9,490 |
| Major (26-50%) | 70 | 155 |
| Minor (10-25%) | 148 | 312 |
| Affected (1-9%) | 858 | 1,809 |
| Inaccessible | 40 | 33 |
| No Damage | 7,893 | 7,981 |
| Total | 18,428 | 19,780 |

Expected structure:

```text
Eaton_Fire/
|-- Affected_1-9_/attachments/
|-- Destroyed_50_/attachments/
|-- Inaccessible/attachments/
|-- Major_26-50_/attachments/
|-- Minor_10-25_/attachments/
|-- No_Damage/attachments/
|-- Eaton_Fire_Affected_1-9__points.csv
|-- Eaton_Fire_Destroyed_50__points.csv
|-- Eaton_Fire_Inaccessible_points.csv
|-- Eaton_Fire_Major_26-50__points.csv
|-- Eaton_Fire_Minor_10-25__points.csv
|-- Eaton_Fire_No_Damage_points.csv
|-- Eaton_Fire_attachments_index.csv
`-- Eaton_Fire_summary.csv
```

Key notes:

- `Eaton_Fire_attachments_index.csv` is the attachment-level master table
- class-specific point CSVs store point-level inspection records
- downloaded image filenames follow `{latitude}_{longitude}_OID{objectid}_A{attachment_id}.jpg`
- this folder is the authoritative labeled source used by `scripts/data_prep/parse_metadata.py`

Git preserves this directory structure, but the full image contents remain local and are not intended to be versioned in the repository.
