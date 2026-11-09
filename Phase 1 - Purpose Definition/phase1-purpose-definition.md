# Phase 1 - Purpose Definition

## Project Name
**Territory and Education Facilities**

## Purpose
The purpose of this project is to engineer a Knowledge Graph which models various geographical features and different education-related facilities within a territory (e.g., Trentino), based on a precise set of user-oriented Competency Questions (CQs) that the Knowledge Graph can answer. The scope focuses on municipalities around Trento and Rovereto, and covers local educational sites such as kindergartens, primary schools, secondary schools and higher-education institutions.

## Persona
**Anna** is a parent who plans to move to southern Trentino. Before choosing which municipality to settle in, she needs to compare the educational services available in different towns: what types of institutions exist, whether facilities are close to where she would live, how school supply varies across towns, and whether any town lacks higher-education options.

## Scenario
Anna is comparing municipalities near Trento and Rovereto. She explores the knowledge graph to answer: which educational institutions each town provides, which facilities lie near major towns or natural landmarks such as lakes and mountains, which areas have the most or fewest schools, and whether any municipality has no higher-education institution nearby. She uses these queries to support her family relocation decision-making.

## Competency Questions (CQs)

### Core CQs (from project proposal #6)
1. Which types of educational institutions are available in a specific municipality (e.g., Trento or Rovereto)?
2. What educational facilities are located near major towns or natural landmarks (like lakes or mountains)?
3. Which areas in Trentino have the highest or lowest number of schools?
4. Are there any municipalities without access to higher education institutions?

### Supporting queries (optional)
5. What are the address and geographic coordinates of a selected educational facility?
6. Which municipalities provide kindergarten / primary-school / secondary-school facilities?

> **Open issues to verify against data**: CQ2 (near landmarks), CQ3 (highest/lowest school count) and CQ4 (no higher-education access) can only be confirmed after inspecting the source CSV fields. Any CQ that the available data cannot support will be documented transparently as an open issue, rather than solved with a self-searched new data source.

## Scope

**In scope**
- Municipalities around Trento and Rovereto, within the Trentino region.
- Educational institutions: kindergartens, primary schools, secondary schools, higher-education institutions.
- Geographical features: major towns, lakes, mountains, and facility addresses / coordinates.

**Out of scope**
- Public-transport commuting routes, bus/train reachability, and bike/walk routes (these belong to proposal #5, not this project).
- Time-series enrollment statistics, exam scores, and reviews, unless provided by the assigned datasets.
## Initial Resources

### Knowledge resources
- Shared project folder: https://drive.google.com/drive/folders/1P6mqXZVkrJTMq8YycwsG_p9DXrcBjyjpp?usp=sharing

### Semi-formal resources
- Past education KG repository (ETG datasets): https://github.com/samuelebortolotti/Education-Trentino/tree/main/Datasets/Data%20Integration/ETG

### Informal resources
- Comune di Trento - student sites download: https://www.comune.trento.it/Areemtematiche/Cartografia/Download/Studentati
- Open Data Trentino - Digital university

### Schema references
- VIVO ontology (LOV): https://lov.linkeddata.es/dataset/lov/vivo
- Schema.org: https://schema.org/docs/full.html
- Schema.org (LOV): https://lov.linkeddata.es/dataset/lov/vocabs/schema
- Geospatial ontology: https://github.com/subhashishhh/Geospatial-Ontology
- Time ontology (LOV): https://lov.linkeddata.es/dataset/lov/vocabs/time
- GTFS static reference: https://gtfs.org/schedule/reference/
- GTFS ontology (LOV): https://lov.linkeddata.es/dataset/lov/vocabs/gtfs
- GTFS ontology (Subhashish): https://github.com/subhashishhh/GTFSOntology
  - Note: GTFS is a transit vocabulary; it is listed in the proposal but is not adopted here because this project does not model commuting.

### Open data catalogs
- Open Data Trentino: https://dati.trentino.it/
- KGE Catalog resources: https://datascientiafoundation.github.io/LiveData-KGE/datasets/

## Preliminary ER Model

> First-pass conceptual model; it will be refined into the formal ontology in Phase 2 (Language Definition) and Phase 3 (Knowledge Definition).

### Entities
| Class | Description | Subclasses |
|---|---|---|
| Municipality | A local administrative town in Trentino | — |
| EducationalInstitution | Any education-related facility, top-level class | Kindergarten, PrimarySchool, SecondarySchool, HigherEducationInstitution |
| Kindergarten | Early-childhood education site | — |
| PrimarySchool | Primary-education site | — |
| SecondarySchool | Lower / upper secondary school | — |
| HigherEducationInstitution | University or other tertiary institution | — |
| GeoFeature | A geographic reference point or area | Town, NaturalLandmark |
| NaturalLandmark | Lake, mountain, or similar natural feature | Lake, Mountain |

### Relationships
| Domain | Property | Range | Meaning |
|---|---|---|---|
| EducationalInstitution | locatedIn | Municipality | A facility is located in a municipality |
| EducationalInstitution | near | GeoFeature | A facility is near a town or landmark |
| Municipality | hasEducationalInstitution | EducationalInstitution | The reverse of locatedIn |

### Data properties
| Property | Attached to | Meaning |
|---|---|---|
| name | EducationalInstitution, Municipality, GeoFeature | Human-readable name |
| address | EducationalInstitution | Street address |
| latitude / longitude | EducationalInstitution, GeoFeature | Geographic coordinates |
| institutionType | EducationalInstitution | Type of institution |
| istatCode | Municipality | ISTAT code of the municipality, if provided by the dataset |

## CQ-to-model mapping

| CQ | Question | Main entities / relationships used |
|---|---|---|
| CQ1 | Institution types available in a municipality | EducationalInstitution, locatedIn, institutionType, Municipality |
| CQ2 | Facilities near towns or landmarks | EducationalInstitution, near, GeoFeature, NaturalLandmark |
| CQ3 | Municipalities with the most / fewest schools | Municipality, hasEducationalInstitution, count aggregation |
| CQ4 | Municipalities without a higher-education institution | Municipality, HigherEducationInstitution, absence check |

