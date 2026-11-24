# Metadata of the resources of Project 6

Metadata of the data resources (inputs and cleaned tables) and of the knowledge
resources (ontology file, entity file) produced for *Territory and Education
Facilities*. The metadata follow the scheme used in the course: for each resource
the title, the phase, the type, the format, the size, the number of rows or
statements, the provenance, the transformation applied and the rights statement.

## 1. Data resources — original datasets

All sources are public administrative data of the Autonomous Province of Trento and
of ISTAT, downloaded in September 2026 and redistributed here for coursework only.

| # | File | Type | Size (bytes) | Content |
|---|---|---|---|---|
| 1 | `Trentino Commune List.csv` | CSV | 3 725 | 176 municipalities with name, ISTAT code, surface area |
| 2 | `educational_institutions.csv` | CSV | 10 422 | school institutions with level and municipality |
| 3 | `institutes_list_id_number.csv` | CSV | 99 666 | institution identifiers and addresses |
| 4 | `school_list_cleaned_with_id_institute_number.csv` | CSV | 256 762 | school directory with institution identifiers |
| 5 | `schools_in_trentino.csv` | CSV | 127 870 | alternative school directory |
| 6 | `number_scuole_infanzia.csv` | CSV | 2 604 | kindergarten count per municipality |
| 7 | `number_scuole_elementari.csv` | CSV | 2 603 | primary school count per municipality |
| 8 | `number_scuole_medie.csv` | CSV | 2 607 | lower secondary school count per municipality |
| 9 | `numero_scuole_superiori_e_professionali.csv` | CSV | 2 645 | upper secondary school count per municipality |
| 10 | `unitn_institute_final.ttl` | Turtle | 4 477 | the single higher-education institution |

Provenance: open data portals of the Provincia Autonoma di Trento and ISTAT; the
university record was prepared by the author from the University of Trento website.

## 2. Data resources — cleaned tables

Produced in Phase 2 with OpenRefine 3.10 and kept under version control in
`Phase 4 - Entity Definition/cleaned/`. Transformation applied to all seven: leading
and trailing whitespace trimmed, municipality names uppercased, `Male'` repaired to
`Malè`, bilingual compound names kept as one string, `Trent` corrected to `Trento`
(ISTAT 22205, 157.9 km2), duplicate rows removed. A backup of every file was kept.

| File | Size (bytes) | Data rows | Used for |
|---|---|---|---|
| `commune-list.csv` | 3 900 | 176 | one `Municipality` entity each |
| `count-kindergarten.csv` | 2 259 | 162 | `has_kindergartenCount` |
| `count-primary.csv` | 2 253 | 162 | `has_primarySchoolCount` |
| `count-lower-secondary.csv` | 2 261 | 162 | `has_lowerSecondarySchoolCount` |
| `count-upper-secondary.csv` | 2 262 | 162 | `has_upperSecondarySchoolCount` |
| `institutions.csv` | 3 523 | 47 | one `EducationalInstitution` each |
| `kindergarten-directory.csv` | 22 098 | 153 | one `Kindergarten` each |
| **Total** | — | **1 024** | 377 entities |

Rights: public administrative data, redistributed for coursework only.

## 3. Knowledge resources

### Ontology file

| Item | Value |
|---|---|
| File | `education-facilities-ontology.ttl` (105 triples), `-aligned.ttl` (117 triples), `education-facilities-ontology.owl` (same content, Protégé serialisation) |
| Namespace | `http://knowdive.disi.unitn.it/etype#` |
| Classes | 8 |
| Object properties | 3 (`has_municipality`, `has_proximity`, `hasEducationalInstitution`) |
| Data properties | 10 |
| Subclass axioms | 4 internal, 6 towards the imported fragments |
| Expressivity | RDFS-level hierarchy, no reasoner used |
| Serialisation | Turtle (deliverable), OWL/XML (inspection) |
| Imported fragments | KTLO, LTLO, full schema.org |
| Identifier policy | UKC identifier stored in `rdfs:comment` of every term that has one (17 of 21) |

### Entity file

| Item | Value |
|---|---|
| File | `education-facilities-kg.ttl` |
| Instance namespace | `http://kg2026.unitn.it/resource/` |
| Statements | 2 724 (2 347 produced by the mapping + 377 `rdfs:label`) |
| Entities | 377 |
| `rdfs:label` policy | one label per entity, equal to its `has_name` value, so that generic RDF viewers show a readable caption instead of the IRI local name |

### Identifier policy of the instances

Entities are minted as `<namespace>/<class>/<slug>`, where the slug is the cleaned
name. Where the name is ambiguous the slug is disambiguated with the ISTAT code. The
IRI is stable across runs of `build_kg.py`; the labels are regenerated with
`add_labels.py`.

## 4. Tools and versions

| Stage | Tool | Version |
|---|---|---|
| cleaning | OpenRefine | 3.10 |
| mapping | Karma | 2.5.8 |
| ontology inspection | Protégé | 5.6.3 |
| storage and querying | GraphDB (Graphwise Sandbox) | 11.5.3 |

## 5. Reproduction

```bash
python build_kg.py        # writes instances/education-facilities-kg.ttl (2 347 triples)
python add_labels.py      # appends 377 rdfs:label triples -> 2 724
python test_queries.py    # runs the eight competency queries of Appendix B
```
