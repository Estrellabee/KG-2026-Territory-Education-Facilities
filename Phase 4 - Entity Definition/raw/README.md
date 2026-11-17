# Raw Source Datasets

This directory stores the unmodified source datasets used in Phase 4 (Entity Definition). Files are split into core sources, which are mapped to the ontology, and reference sources, which are used for cleaning, entity matching, and school-level assignment.

## Core sources

- `educational_institutions.csv` — institution name, address, municipality, ownership type
- `Trentino Commune List.csv` — municipality name, ISTAT code, area
- `number_scuole_infanzia.csv` — kindergarten count by municipality (2012)
- `number_scuole_elementari.csv` — primary school count by municipality (2012)
- `number_scuole_medie.csv` — lower secondary school count by municipality (2012)
- `numero_scuole_superiori_e_professionali.csv` — upper secondary and vocational school count by municipality (2012)
- `institutes_list_id_number.csv` — kindergarten institution directory
- `unitn_institute_final.ttl` — University of Trento instance

## Reference sources

- `schools_in_trentino.csv` — school directory with education level, used to assign `schoolLevel`
- `school_list_cleaned_with_id_institute_number.csv` — cleaned school directory with institution identifiers, used for matching and deduplication

## Source

Retrieved from the 2022/2023 KGE reference repository (Bortolotti & Robbi), licensed under Apache-2.0, as the course-provided semi-formal resource for topic #6 (Territory and Education Facilities).
