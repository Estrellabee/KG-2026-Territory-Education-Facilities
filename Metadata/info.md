# Metadata

This directory collects the metadata of the inputs and outputs produced along the
four iTelos phases: the source datasets, the cleaned tables, the ontology and the
entity graph, with the technical fields needed to reproduce the project.

The complete metadata is published in **[`kg-metadata.md`](kg-metadata.md)**.
That file is the authoritative version and contains:

- the ten original datasets, each with type, byte size and content;
- the seven cleaned tables, each with byte size and number of data rows
  (1 024 rows in total, from which 377 entities are minted);
- the metadata of the ontology (namespace, 8 classes, 3 object properties,
  10 data properties, serialisations, imported fragments, UKC policy);
- the metadata of the entity graph (instance namespace, 2 724 statements,
  IRI and label policy);
- the tools and versions, and the commands that regenerate both files.

The earlier metadata of the graph before the `rdfs:label` extension (2 347
statements) is superseded by that file.
