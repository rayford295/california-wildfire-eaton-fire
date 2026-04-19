This directory represents the final sample-organized paired dataset built from the Eaton Fire attachment index.

It is not a separate fire dataset and not an unrelated unlabeled image dump. Instead, it is the attachment-level pairing workspace that links street-view images to remote-sensing crops.

Final structure:

```text
Altadena_Images/
|-- Altadena_Images/
|   |-- *.tif
|   `-- *.tif.aux.xml
|-- Eaton_Fire_attachments_index.csv
|-- Eaton_Fire_attachments_index_output/
|   |-- dataset/
|   |   `-- sample_00001/
|   |       |-- street_view.jpg
|   |       `-- remote_sensing.jpg
|   |-- dataset_index.csv
|   |-- dataset_index.xlsx
|   `-- download_report.csv
|-- download_attachment_index.py
|-- match_remote_sensing.py
`-- requirements.txt
```

Final on-disk counts:

| Item | Count |
| --- | ---: |
| Sample folders | 19,780 |
| `street_view.jpg` files | 19,772 |
| `remote_sensing.jpg` files | 19,754 |
| Complete paired samples | 19,746 |
| Incomplete sample folders | 34 |
| `.tif` files | 91 |
| `.aux.xml` files | 91 |

Key notes:

- each `sample_xxxxx` folder corresponds to one attachment record from `Eaton_Fire_attachments_index.csv`
- `dataset_index.csv` is the main paired-data index
- `download_report.csv` records street-view download status
- absolute path fields inside the generated index files should be treated as provenance metadata, not portable paths
- the repository scripts currently refer to this paired dataset as the `unlabeled` split, even though source metadata such as `fire`, `category`, `objectid`, and `attachment_id` is preserved

Git preserves the code and folder layout here, but the full paired image dataset and raster tiles remain local and are not intended to be committed.
