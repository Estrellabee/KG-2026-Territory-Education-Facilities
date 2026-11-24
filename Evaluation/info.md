# Evaluation

This directory documents the evaluation of the produced knowledge graph: structural
validation, data-quality checks and the execution of the competency questions over
`Phase 4 - Entity Definition/education-facilities-kg.ttl`.

The complete evaluation is published in **[`coverage-and-queries.md`](coverage-and-queries.md)**.
That file is the authoritative version and contains:

- the metadata of the graph (2 724 statements for the entity layer, 105 for the
  terminology, 2 829 in total, 377 entities, 191 `has_municipality` links);
- the five coverage measures with their formulas (`Cov_E`, `Cov_OP`, `Cov_P`,
  `Cov_UKC`, `Pop_E`) and the four documented gaps;
- the eight competency questions as executed, with the answers returned by the
  repository;
- the validation checks (no empty name, no dangling link, ten facilities kept
  without a municipality on purpose);
- the tools and versions used, and the SPARQL patterns needed to recompute every
  number.

The earlier evaluation of the graph before the `rdfs:label` extension (2 347
statements) is superseded by that file.
