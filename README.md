# KG 2026 — Territory and Education Facilities

Knowledge Graph project #6 for the KG 2026 course (University of Trento), built following the iTelos methodology (Purpose → Language → Knowledge → Entity).

## Purpose
The graph models municipalities of the Trentino region and the educational facilities located in them — kindergartens, primary schools, lower and upper secondary schools, and higher-education institutions — so that a parent relocating to southern Trentino can compare the educational offer of different towns.

## Competency Questions
1. Which types of educational institutions are available in a specific municipality (e.g. Trento or Rovereto)?
2. What educational facilities are located near major towns or natural landmarks (lakes, mountains)?
3. Which areas in Trentino have the highest or lowest number of schools?
4. Are there any municipalities without access to higher education institutions?

## Repository structure
| Folder | Content |
|---|---|
| `Phase 1 - Purpose Definition` | Purpose, persona, scenario and competency questions |
| `Phase 2 - Language Definition` | Glossary and language-level definitions |
| `Phase 3 - Knowledge Definition` | Ontology (`education-facilities-ontology.ttl`) and its documentation |
| `Phase 4 - Entity Definition/raw` | Original source datasets, unmodified |
| `Phase 4 - Entity Definition/cleaned` | Datasets after cleaning and municipality-name harmonisation |
| `Phase 4 - Entity Definition` | Instance graph (`education-facilities-kg.ttl`), mapping specification and data-cleaning notes |
| `Metadata` | Metadata of the inputs and outputs |
| `Evaluation` | Validation results and the executed competency questions |

## Data
Source datasets: Trentino municipality list (ISTAT codes and surface area), list of educational institutions, four school-count tables (kindergarten, primary, lower secondary, upper secondary), a kindergarten directory, and one higher-education institution.

- Ontology namespace: `http://knowdive.disi.unitn.it/etype#`
- Instance namespace: `http://kg2026.unitn.it/resource/`

The instance graph contains 2,347 triples: 176 municipalities, 47 educational institutions, 153 kindergartens and 1 higher-education institution. The ontology defines 8 classes, 3 object properties and 10 data properties.

## Tooling
OpenRefine 3.10.0 for data cleaning, Karma 2.5.8 for the mapping experiments, Python 3.13 with `rdflib` 7.6.0 for graph generation, validation and query execution. The SPARQL queries answering the competency questions are documented in `Evaluation`.

## Author
HaoqiangZhou — KG 2026, University of Trento.
