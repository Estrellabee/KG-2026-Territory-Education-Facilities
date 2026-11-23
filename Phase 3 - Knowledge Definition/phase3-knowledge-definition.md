# Phase 3 - Knowledge Definition

## Overview
This phase formalizes the vocabulary defined in Phase 2 into a machine-readable Entity Type Graph (ETG): a class hierarchy, property domains and ranges, data types, and basic constraints. The inputs are the Phase 1 ER model and competency questions, the selected source datasets, the Phase 2 vocabulary, and the reference ontologies. The outputs are the formal knowledge resources (the ETG) and a CSV-to-ontology mapping design to be applied during Phase 4. Standard ontologies are reused wherever possible, and the candidate alignments from Phase 2 are confirmed here.

## Ontology Selection
| Ontology | Role in this KG | Adoption |
|---|---|---|
| KTLO-KG.owl (course fragment) | Knowledge teleology: OpenStreetMap-derived place taxonomy, 829 classes | Adopted (imported) |
| LTLO-KG.owl (course fragment) | Language teleology of the same taxonomy | Adopted (imported, content-equivalent to KTLO) |
| full-schema-org.owl (course fragment) | General vocabulary for organizations, places, names, and addresses | Adopted (imported) |
| Universal Knowledge Core (UKC) | Lexical anchoring of every term, identifier stored in `rdfs:comment` | Adopted as the first alignment tier |
| VIVO | Academic organization and geographic location terms | Adopted partially |
| Geospatial ontology | Spatial feature concepts | Adopted in a limited way; no coordinate data is available |
| time ontology | Temporal and interval concepts | Not adopted; no time-series modelling in this project |
| GTFS ontology | Transit schedule and route vocabulary | Not adopted; commuting is not modelled |

## Formal Class Hierarchy
- `Municipality` — aligned to schema.org `AdministrativeArea / City`, VIVO `GeographicLocation`
- `EducationalInstitution` — aligned to schema.org `EducationalOrganization`, VIVO `Organization`
  - `Kindergarten`
  - `PrimarySchool`
  - `SecondarySchool`
  - `HigherEducationInstitution` — aligned to schema.org `CollegeOrUniversity`
- `GeoFeature` — aligned to Geospatial ontology `SpatialFeature`
  - `NaturalLandmark`

## Property Definitions

### Object properties
| Property | Domain | Range | Notes |
|---|---|---|---|
| has_municipality | EducationalInstitution | Municipality | A facility is located in a municipality |
| has_proximity | EducationalInstitution | GeoFeature | Unsupported by current source data; kept as a conceptual relation |
| hasEducationalInstitution | Municipality | EducationalInstitution | Inverse of has_municipality |

### Data properties
| Property | Domain | Range / datatype | Notes |
|---|---|---|---|
| has_name | EducationalInstitution, Municipality, GeoFeature | xsd:string | Human-readable name |
| has_address | EducationalInstitution | xsd:string | Street address |
| has_ownershipType | EducationalInstitution | xsd:string, enumerated | public / religious / lay (source values Pubblico / Religioso / Laico) |
| has_schoolLevel | EducationalInstitution | xsd:string, enumerated | kindergarten / primary / secondary / higher |
| has_istatCode | Municipality | xsd:string | ISTAT municipality code |
| has_area | Municipality | xsd:decimal | Surface area of the municipality |
| has_kindergartenCount / has_primarySchoolCount / has_lowerSecondarySchoolCount / has_upperSecondarySchoolCount | Municipality | xsd:integer | Number of schools per level; extension added in Phase 4 |

> Namespace: `http://knowdive.disi.unitn.it/etype#` (prefix `edu:`). The ontology file `education-facilities-ontology.ttl` contains 105 triples: 8 classes, 3 object properties, 10 data properties, one gloss for each of the 21 terms, and the UKC identifier in 17 of them (the other four are documented gaps: NaturalLandmark, has_municipality, has_proximity, hasEducationalInstitution). `education-facilities-ontology-aligned.ttl` adds the three `owl:imports`, six subclass axioms and three sub-property axioms (117 triples in total).

## Constraints
- Every `EducationalInstitution` must be linked to exactly one `Municipality` through `has_municipality`.
- An institution instance is assigned to exactly one `has_schoolLevel` subclass.
- `has_ownershipType` is restricted to the enumerated values public, religious, lay.
- Coordinates, population, courses, and staff persons are not modelled in this KG.

## Mapping Design (CSV to ontology)
| Source table | Source column | Ontology class / property |
|---|---|---|
| educational_institutions.csv | institute_name | EducationalInstitution.has_name |
| educational_institutions.csv | institute_address | EducationalInstitution.has_address |
| educational_institutions.csv | institute_comune | EducationalInstitution has_municipality Municipality |
| educational_institutions.csv | institute_type | EducationalInstitution.has_ownershipType |
| Trentino Commune List.csv | comune-name | Municipality.has_name |
| Trentino Commune List.csv | comune-code | Municipality.has_istatCode |
| Trentino Commune List.csv | area | Municipality.has_area |
| institutes_list_id_number.csv | name, address, comune | Kindergarten instance |
| number_scuole_*.csv | municipality name, school count | Municipality coverage and count by school level (CQ3, Sup6) |
| unitn_institute_final.ttl | institution, address, municipality | HigherEducationInstitution instance, has_municipality Municipality(Trento) |

> School level is derived from the school-type source tables; contact fields (phone, fax, website, email) and staff/person records are out of scope for this KG.
