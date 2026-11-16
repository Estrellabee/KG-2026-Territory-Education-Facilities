# Phase 3 - Knowledge Definition

## Overview
This phase formalizes the vocabulary defined in Phase 2 into a machine-readable Entity Type Graph (ETG): a class hierarchy, property domains and ranges, data types, and basic constraints. The inputs are the Phase 1 ER model and competency questions, the selected source datasets, the Phase 2 vocabulary, and the reference ontologies. The outputs are the formal knowledge resources (the ETG) and a CSV-to-ontology mapping design to be applied during Phase 4. Standard ontologies are reused wherever possible, and the candidate alignments from Phase 2 are confirmed here.

## Ontology Selection
| Ontology | Role in this KG | Adoption |
|---|---|---|
| schema.org | General vocabulary for organizations, places, names, and addresses | Adopted |
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
| locatedIn | EducationalInstitution | Municipality | A facility is located in a municipality |
| near | EducationalInstitution | GeoFeature | Unsupported by current source data; kept as a conceptual relation |
| hasEducationalInstitution | Municipality | EducationalInstitution | Inverse of locatedIn |

### Data properties
| Property | Domain | Range / datatype | Notes |
|---|---|---|---|
| name | EducationalInstitution, Municipality, GeoFeature | xsd:string | Human-readable name |
| address | EducationalInstitution | xsd:string | Street address |
| ownershipType | EducationalInstitution | xsd:string, enumerated | public / religious / lay (source values Pubblico / Religioso / Laico) |
| schoolLevel | EducationalInstitution | xsd:string, enumerated | kindergarten / primary / secondary / higher |
| istatCode | Municipality | xsd:string | ISTAT municipality code |
| area | Municipality | xsd:decimal | Surface area of the municipality |

## Constraints
- Every `EducationalInstitution` must be linked to exactly one `Municipality` through `locatedIn`.
- An institution instance is assigned to exactly one `schoolLevel` subclass.
- `ownershipType` is restricted to the enumerated values public, religious, lay.
- Coordinates, population, courses, and staff persons are not modelled in this KG.

## Mapping Design (CSV to ontology)
| Source table | Source column | Ontology class / property |
|---|---|---|
| educational_institutions.csv | institute_name | EducationalInstitution.name |
| educational_institutions.csv | institute_address | EducationalInstitution.address |
| educational_institutions.csv | institute_comune | EducationalInstitution locatedIn Municipality |
| educational_institutions.csv | institute_type | EducationalInstitution.ownershipType |
| Trentino Commune List.csv | comune-name | Municipality.name |
| Trentino Commune List.csv | comune-code | Municipality.istatCode |
| Trentino Commune List.csv | area | Municipality.area |
| institutes_list_id_number.csv | name, address, comune | Kindergarten instance |
| number_scuole_*.csv | municipality name, school count | Municipality coverage and count by school level (CQ3, Sup6) |
| unitn_institute_final.ttl | institution, address, municipality | HigherEducationInstitution instance, locatedIn Municipality(Trento) |

> School level is derived from the school-type source tables; contact fields (phone, fax, website, email) and staff/person records are out of scope for this KG.

