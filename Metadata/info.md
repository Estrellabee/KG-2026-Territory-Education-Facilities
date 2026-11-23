# Metadata

This directory collects the metadata of the inputs and outputs produced along the four iTelos phases. The description follows a minimal Dublin Core style profile (title, phase, type, format, size, provenance, rights), extended with the technical fields needed to reproduce the project.

## 1. Inputs (source datasets)

| # | Resource | Phase | Type | Format | Size (bytes) | Rows | Provenance |
|---|---|---|---|---|---|---|---|
| 1 | `Trentino Commune List.csv` | 1–4 | Tabular | CSV | 3,725 | 176 | Municipality list of the Autonomous Province of Trento (ISTAT codes and surface area) |
| 2 | `educational_institutions.csv` | 1–4 | Tabular | CSV | 10,422 | 47 | List of educational institutions of the province (name, address, municipality, ownership type) |
| 3 | `institutes_list_id_number.csv` | 1–4 | Tabular | CSV | 99,666 | — | Kindergarten / school directory with internal institute identifiers |
| 4 | `number_scuole_infanzia.csv` | 1–4 | Tabular | CSV | 2,604 | 166 | Number of kindergartens per municipality |
| 5 | `number_scuole_elementari.csv` | 1–4 | Tabular | CSV | 2,603 | 166 | Number of primary schools per municipality |
| 6 | `number_scuole_medie.csv` | 1–4 | Tabular | CSV | 2,607 | 166 | Number of lower secondary schools per municipality |
| 7 | `numero_scuole_superiori_e_professionali.csv` | 1–4 | Tabular | CSV | 2,645 | 166 | Number of upper secondary schools per municipality |
| 8 | `schools_in_trentino.csv` | 1–4 | Tabular | CSV | 127,870 | — | Reference list of schools in Trentino (secondary source, not used for the mappings) |
| 9 | `school_list_cleaned_with_id_institute_number.csv` | 1–4 | Tabular | CSV | 256,762 | — | Reference list combining school names and institute identifiers (not used for the mappings) |
| 10 | `unitn_institute_final.ttl` | 1–4 | Graph | Turtle | 4,477 | 1 institution | Higher-education institution (University of Trento, Via Calepina 14, Trento), provided as RDF |

All input files are stored unmodified in `Phase 4 - Entity Definition/raw/`.

Rights: the datasets are public data published for the Trentino territory and are redistributed here only for academic coursework. Repository code and documentation are covered by the `LICENSE` file at the root of this repository.

## 2. Intermediate outputs

| # | Resource | Phase | Type | Format | Size (bytes) | Rows | Method |
|---|---|---|---|---|---|---|---|
| 11 | `commune-list.csv` | 4 | Tabular | CSV | 3,900 | 176 | OpenRefine cleaning; `Trent` corrected to `Trento` (ISTAT 22205, 157.9 km²) |
| 12 | `institutions.csv` | 4 | Tabular | CSV | 3,523 | 47 | OpenRefine cleaning; ownership values normalised to `public` / `religious` / `lay` |
| 13 | `count-kindergarten.csv` | 4 | Tabular | CSV | 2,259 | 162 | 4 rows without a municipality removed; municipality names harmonised |
| 14 | `count-primary.csv` | 4 | Tabular | CSV | 2,253 | 162 | As above |
| 15 | `count-lower-secondary.csv` | 4 | Tabular | CSV | 2,261 | 162 | As above |
| 16 | `count-upper-secondary.csv` | 4 | Tabular | CSV | 2,262 | 162 | As above |
| 17 | `kindergarten-directory.csv` | 4 | Tabular | CSV | 22,098 | 153 | Column subset kept; `tipo_instituto` normalised to `kindergarten` |

Cleaned files are stored in `Phase 4 - Entity Definition/cleaned/`. Municipality names were harmonised with the official spelling of the municipality list (apostrophes used instead of accents, bilingual compound names reduced to the Italian form, all-caps forms rewritten); the decisions are documented in `phase4-name-normalization-note.md`.

## 3. Outputs (knowledge resources)

| # | Resource | Phase | Type | Format | Size | Content |
|---|---|---|---|---|---|---|
| 18 | `education-facilities-ontology.ttl` | 3 | Ontology (ETG) | Turtle | 5,278 bytes / 92 triples | 8 classes, 3 object properties, 10 data properties; namespace `http://kg2026.unitn.it/ontology/education#` |
| 19 | `education-facilities-kg.ttl` | 4 | Instance graph | Turtle | 118,103 bytes / 2,347 triples | 377 entities: 176 municipalities, 47 educational institutions, 153 kindergartens, 1 higher-education institution |
| 20 | `phase4-entity-mapping.md` | 4 | Documentation | Markdown | 8,273 bytes | Mapping specification, validation results and open issues |
| 21 | `phase4-name-normalization-note.md` | 4 | Documentation | Markdown | 4,704 bytes | Record of the municipality-name normalisation and cleaning decisions |

Both RDF resources use the same ontology namespace; instance URIs follow the pattern `http://kg2026.unitn.it/resource/{class}/{percent-encoded name}`, with one sub-namespace per entity type (`municipality`, `institution`, `kindergarten`, `university`).

## 4. Tools

OpenRefine 3.10.0 (data cleaning), Karma 2.5.8 (mapping experiments), Python 3.13 with `rdflib` 7.6.0 (graph generation, validation and query execution).
