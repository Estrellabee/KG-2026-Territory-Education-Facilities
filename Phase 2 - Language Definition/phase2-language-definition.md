# Phase 2 - Language Definition

## Overview
This phase fixes the concepts and terms adopted to describe the information in the Knowledge Graph, so that the final result has a consistent and reusable vocabulary. The input is the formalized purpose, the competency questions, and the resources collected in Phase 1. The terms below are aligned with standard reference ontologies (schema.org, VIVO, Geospatial ontology) wherever possible; mappings are candidate alignments to be confirmed during Phase 3.

**Namespace and prefix.** All terms live in `http://knowdive.disi.unitn.it/etype#` (prefix `edu:`), the base URI used by the course projects. Instance IRIs are kept in a separate namespace (`http://kg2026.unitn.it/resource/`). Every property name carries the `has_` prefix, so that a property can be recognised from its identifier alone and never clashes with a class name.

## Classes

### Class hierarchy
- `Municipality`
- `EducationalInstitution`
  - `Kindergarten`
  - `PrimarySchool`
  - `SecondarySchool`
  - `HigherEducationInstitution`
- `GeoFeature`
  - `NaturalLandmark`

### Class descriptions
| Class | Description | Subclasses |
|---|---|---|
| Municipality | An administrative town or municipality in the Trentino region | — |
| EducationalInstitution | Any education-related facility, top-level class | Kindergarten, PrimarySchool, SecondarySchool, HigherEducationInstitution |
| Kindergarten | Early-childhood education site | — |
| PrimarySchool | Primary-education site | — |
| SecondarySchool | Lower and upper secondary school | — |
| HigherEducationInstitution | University or other tertiary institution | — |
| GeoFeature | A geographic reference point or area | NaturalLandmark |
| NaturalLandmark | Lake, mountain, or similar natural feature | — |

## Object properties
| Domain | Property | Range | Meaning |
|---|---|---|---|
| EducationalInstitution | has_municipality | Municipality | A facility is located in a municipality |
| EducationalInstitution | has_proximity | GeoFeature | A facility is near a town or landmark |
| Municipality | hasEducationalInstitution | EducationalInstitution | The reverse of has_municipality |

## Data properties
| Property | Attached to | Meaning | Notes |
|---|---|---|---|
| has_name | EducationalInstitution, Municipality, GeoFeature | Human-readable name | From source name fields |
| has_address | EducationalInstitution | Street address | From source address field |
| has_ownershipType | EducationalInstitution | Ownership type: public, religious, lay | Source values Pubblico / Religioso / Laico |
| has_schoolLevel | EducationalInstitution | Education level: kindergarten, primary, secondary, higher | Derived from the school-type source tables |
| has_istatCode | Municipality | ISTAT code of the municipality | From the commune list |
| has_area | Municipality | Surface area of the municipality | From the commune list |
| has_kindergartenCount / has_primarySchoolCount / has_lowerSecondarySchoolCount / has_upperSecondarySchoolCount | Municipality | Number of schools per level | Added in Phase 4 as an extension: the count tables describe the municipality, not the facility |

## Vocabulary alignment
| Local term | Type | Reference ontology | Reference term |
|---|---|---|---|
| Municipality | Class | schema.org | AdministrativeArea / City |
| Municipality | Class | VIVO | GeographicLocation, SubnationalRegion |
| EducationalInstitution | Class | schema.org | EducationalOrganization |
| EducationalInstitution | Class | VIVO | Organization |
| HigherEducationInstitution | Class | schema.org | CollegeOrUniversity |
| has_name | Data property | schema.org | name |
| has_address | Data property | schema.org | address |
| has_municipality | Object property | VIVO | geographicLocation / has_municipality |
| GeoFeature | Class | Geospatial ontology | SpatialFeature |

## Universal Knowledge Core anchoring

Terms were first searched in the Universal Knowledge Core (`https://ukc.datascientia.eu`), following the three-tier rule of the course (UKC → external ontology → project-specific identifier). The identifier of each term is stored in its `rdfs:comment` in the ontology file; the full table with glosses is in `phase2-ukc-alignment.md`.

| Local term | UKC concept | Tier |
|---|---|---|
| Municipality | UKC-45537 | 1 |
| EducationalInstitution | UKC-43985 | 1 |
| Kindergarten | UKC-44658 | 1 |
| PrimarySchool | UKC-44664 (label *grade school*) | 1 |
| SecondarySchool | UKC-44031 | 1 |
| HigherEducationInstitution | UKC-24619 (label *university*) | 1 |
| GeoFeature | UKC-45324 (label *geographical area*) | 1 |
| NaturalLandmark | no match (landmark has only anatomical / boundary / historical senses) | 3 |
| has_name | UKC-2 | 1 |
| has_address | UKC-45005 (label *mailing address*) | 1 |
| has_ownershipType | UKC-62515 | 1 |
| has_schoolLevel | UKC-19654 (label *level*) | 1 |
| has_istatCode | UKC-33626 | 1 |
| has_area | UKC-27784 (two-dimensional surface sense) | 1 |
| has_*Count | UKC-3211 (*count*) as shared root | 1 |
| has_municipality | no UKC relation; aligned to schema.org instead | 2 |
| has_proximity | no UKC or KTLO equivalent | 3 |

## Open issues and excluded terms
- `latitude` / `longitude`: not present in the available source data; not modelled. Proximity to landmarks (CQ2) remains an open issue.
- `population`: not present in the commune list; not modelled.
- `NaturalLandmark` (lake, mountain) is kept as a conceptual class, but no landmark instances are available in the current datasets; it is retained for completeness and flagged as unsupported by data.
- GTFS transit vocabulary is listed in the proposal but is not adopted, as this project does not model commuting.
