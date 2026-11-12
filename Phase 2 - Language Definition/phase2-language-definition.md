# Phase 2 - Language Definition

## Overview
This phase fixes the concepts and terms adopted to describe the information in the Knowledge Graph, so that the final result has a consistent and reusable vocabulary. The input is the formalized purpose, the competency questions, and the resources collected in Phase 1. The terms below are aligned with standard reference ontologies (schema.org, VIVO, Geospatial ontology) wherever possible; mappings are candidate alignments to be confirmed during Phase 3.

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
| EducationalInstitution | locatedIn | Municipality | A facility is located in a municipality |
| EducationalInstitution | near | GeoFeature | A facility is near a town or landmark |
| Municipality | hasEducationalInstitution | EducationalInstitution | The reverse of locatedIn |

## Data properties
| Property | Attached to | Meaning | Notes |
|---|---|---|---|
| name | EducationalInstitution, Municipality, GeoFeature | Human-readable name | From source name fields |
| address | EducationalInstitution | Street address | From source address field |
| ownershipType | EducationalInstitution | Ownership type: public, religious, lay | Source values Pubblico / Religioso / Laico |
| schoolLevel | EducationalInstitution | Education level: kindergarten, primary, secondary, higher | Derived from the school-type source tables |
| istatCode | Municipality | ISTAT code of the municipality | From the commune list |
| area | Municipality | Surface area of the municipality | From the commune list |

## Vocabulary alignment
| Local term | Type | Reference ontology | Reference term |
|---|---|---|---|
| Municipality | Class | schema.org | AdministrativeArea / City |
| Municipality | Class | VIVO | GeographicLocation, SubnationalRegion |
| EducationalInstitution | Class | schema.org | EducationalOrganization |
| EducationalInstitution | Class | VIVO | Organization |
| HigherEducationInstitution | Class | schema.org | CollegeOrUniversity |
| name | Data property | schema.org | name |
| address | Data property | schema.org | address |
| locatedIn | Object property | VIVO | geographicLocation / locatedIn |
| GeoFeature | Class | Geospatial ontology | SpatialFeature |

## Open issues and excluded terms
- `latitude` / `longitude`: not present in the available source data; not modelled. Proximity to landmarks (CQ2) remains an open issue.
- `population`: not present in the commune list; not modelled.
- `NaturalLandmark` (lake, mountain) is kept as a conceptual class, but no landmark instances are available in the current datasets; it is retained for completeness and flagged as unsupported by data.
- GTFS transit vocabulary is listed in the proposal but is not adopted, as this project does not model commuting.

