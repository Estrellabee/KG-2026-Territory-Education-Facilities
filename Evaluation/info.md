# Evaluation

This directory documents the evaluation of the produced knowledge graph: structural validation, data-quality checks and the execution of the competency questions over `Phase 4 - Entity Definition/education-facilities-kg.ttl`.

## 1. Structural validation

The instance graph was parsed with `rdflib` 7.6.0 (Turtle parser) and checked with the following assertions. Every check passed.

| Check | Expected | Result |
|---|---|---|
| Graph parses without error | yes | yes |
| Total triples | — | 2,347 |
| Entities (typed non-blank subjects) | — | 377 |
| Municipalities | 176 | 176 |
| Educational institutions | 47 | 47 |
| Kindergartens | 153 | 153 |
| Higher-education institutions | 1 | 1 |
| `istatCode` values | 176 | 176 |
| `area` values | 176 | 176 |
| Count values (4 properties) | 162 each | 648 triples |
| `locatedIn` links pointing to a non-existent municipality | 0 | 0 |
| Empty `name` literals | 0 | 0 |
| Municipalities without `name` | 0 | 0 |
| URIs referring to a local mapping server | 0 | 0 |

## 2. Data-quality observations

- 41 of 47 educational institutions are linked to a municipality; 6 institutions have no municipality in the source data and are kept as instances without `locatedIn`.
- 149 of 153 kindergartens are linked to a municipality. The municipality was recovered from the address field (the town name following the 5-digit CAP); the 4 unresolved kindergartens are located in municipalities created in 2020 that are not part of the 176-row municipality list.
- The four count tables were reduced from 166 to 162 rows: rows with an empty municipality were removed, because a count row without a municipality would produce an entity with no identity.

## 3. Executed competency questions

All queries below were executed on the instance graph. Results are the output of the final validated version.

| CQ | Query intent | Result |
|---|---|---|
| CQ1-a | Types of educational institutions in the municipality of Trento | 22 educational institutions, 19 kindergartens, 1 higher-education institution |
| CQ1-b | School provision per level in Trento and Rovereto | Trento: 41 kindergartens, 32 primary, 13 lower secondary, 16 upper secondary; Rovereto: 15 / 9 / 6 / 10 |
| CQ3-a | Municipalities with the most kindergartens | Trento 41, Rovereto 15, Pergine Valsugana 6, Predaia 6, Ala 5 |
| CQ3-b | Municipalities with the fewest kindergartens | 25 municipalities report 0 (e.g. Amblar-Don, Bocenago, Bresimo, Caderzone Terme, Caldes) |
| CQ3-c | Municipalities with the most upper secondary schools | Trento 16, Rovereto 10, Cles 4, Tione di Trento 4, Arco 3 |
| CQ4 | Municipalities without a higher-education institution | 175 of 176; only Trento hosts one |
| Sup6 | Municipalities providing at least one kindergarten | 137 |
| CQ2 | Facilities near major towns or natural landmarks | Not answerable: the available data contains no coordinates or landmark references, and the relation `near` remains conceptual |
| Sup5 | Address of a facility | Answerable for institutions and kindergartens (address literals present), but not with geographic coordinates |

### Query examples

```sparql
# CQ1-a: types of institutions located in the municipality of Trento
PREFIX edu: <http://kg2026.unitn.it/ontology/education#>
SELECT ?cls (COUNT(?x) AS ?n) WHERE {
  ?x edu:locatedIn <http://kg2026.unitn.it/resource/municipality/Trento> .
  ?x a ?cls .
} GROUP BY ?cls ORDER BY DESC(?n)
```

```sparql
# CQ3-a: five municipalities with the most kindergartens
PREFIX edu: <http://kg2026.unitn.it/ontology/education#>
SELECT ?mName ?n WHERE {
  ?m edu:name ?mName ; edu:kindergartenCount ?n .
} ORDER BY DESC(?n) LIMIT 5
```

```sparql
# CQ4: number of municipalities without a higher-education institution
PREFIX edu: <http://kg2026.unitn.it/ontology/education#>
SELECT (COUNT(?m) AS ?n) WHERE {
  ?m a edu:Municipality .
  FILTER NOT EXISTS { ?h a edu:HigherEducationInstitution ; edu:locatedIn ?m }
}
```

## 4. Limitations

- CQ2 cannot be answered with the available datasets; no coordinates, lake or mountain references were collected, so the `near` relation is defined in the ontology but not populated.
- CQ4 is technically answerable but its answer is dominated by the fact that only one higher-education institution exists in the source data.
- No OWL reasoner was run: subclass relations were applied explicitly during graph generation instead of being materialised by inference.
- School counts come from aggregate tables and are not reconciled against the individual facility instances.
