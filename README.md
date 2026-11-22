# KG 2026 — Territory and Education Facilities

Knowledge Graph project #6 for the KG 2026 course (University of Trento), built following the iTelos methodology (Purpose → Language → Knowledge → Entity).

## Purpose
The graph models municipalities of the Trentino region and the educational facilities located in them — kindergartens, primary schools, lower/upper secondary schools and higher-education institutions — so that a parent relocating to southern Trentino can compare the educational offer across towns.

## Competency Questions
1. Which types of educational institutions are available in a specific municipality (e.g. Trento or Rovereto)?
2. What educational facilities are located near major towns or natural landmarks?
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
| `Metadata` | Metadata of inputs and outputs |
| `Evaluation` | Validation results and executed competency queries |

## Data
Source datasets: Trentino municipality list, educational institutions list, school-count tables (kindergarten, primary, lower secondary, upper secondary), kindergarten directory, and one higher-education institution.

Ontology namespace: `http://kg2026.unitn.it/ontology/education#`
Instance namespace: `http://kg2026.unitn.it/resource/`

The instance graph contains 2347 triples: 176 municipalities, 47 educational institutions, 153 kindergartens and 1 higher-education institution.

## Tooling
OpenRefine 3.10.0 for data cleaning, Karma for the mapping experiments, `rdflib` for graph generation and validation. SPARQL queries answering the competency questions are documented in `Evaluation`.

## Author
Estrellabee — KG 2026, University of Trento.
