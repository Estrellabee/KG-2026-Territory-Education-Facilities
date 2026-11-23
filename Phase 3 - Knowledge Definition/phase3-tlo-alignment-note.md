# Phase 3 — Alignment to the supplied KTLO / LTLO / schema.org fragments

Date: 2026-11-23
Author: project team (KG 2026, project 6 — Territory and Education Facilities)

This note records one decision that was missing from the first version of Phase 3: the
domain ontology of the project (`education-facilities-ontology.ttl`) was written from
scratch and never linked to the upper-level resources distributed by the course. The
aligned version is `education-facilities-ontology-aligned.ttl`; the earlier file is kept
unchanged for traceability.

---

## 1. Which supplied resource is what

The course distributes a folder named **KG Composable Fragments** (owner M. Bagchi,
2025-09-19 / 2025-10-17). Project slide 6 lists it as *Knowledge resources* and the
project template requires the inputs:

> Input - 3 Datasets **[AND] Knowledge Teleology (KTLO)** **[AND] Reference Fragments
> [AND] Language Teleology (LTLO)**

So the folder is a library of ontology fragments, not project data. It contains 20
entries. The table below states, for every entry, whether the project uses it and why.
This is the answer to "the resources were already provided — did you use them?"

| # | File | Role | Used? | Reason |
|---|------|------|-------|--------|
| 1 | `KTLO-KG.owl` (524 KB) | Knowledge Teleology | **imported** | Upper layer the domain ontology must align to (slide 6: "build TLO/ETG aligned to KTLO"). 829 classes under the single root `ktlo:openstreetmap_place`: `education → school / kindergarten / college / university`, `places → city / town / village / hamlet / locality / county / region`, plus `natural`. 16 datatype properties (`name`, `population`, `code`, `st_astext`, `osm_id`, …) and 1 object property (`schema:geo`). |
| 2 | `LTLO-KG.owl` (520 KB) | Language Teleology | **imported** | Required as a separate input by the slide. Verified by parsing: it has the same ontology IRI, the same 829 classes and the same properties as `KTLO-KG.owl` (4775 vs 4739 triples; the 36-triple difference is not structural). Both are imported so that the (KTLO AND LTLO) requirement is literally satisfied; the ontology header records that they are content-equivalent. |
| 3 | `full-schema-org.owl` (1.9 MB) | Reference fragment — schema.org (full) | **imported** | Provides the classes used by the original ontology: `schema:AdministrativeArea`, `schema:EducationalOrganization`, `schema:School`, `schema:CollegeOrUniversity`, `schema:Place` (3958 classes, 1332 object properties, 186 datatype properties). |
| 4 | `dataproperties-schema-org.owl` | schema.org datatype-property module | covered | Module of #3; importing the full file gives the same terms. |
| 5 | `objectproperties-schema-org.owl` | schema.org object-property module | covered | Module of #3. |
| 6 | `place-schema-org.owl` | schema.org `Place` module | covered | Module of #3 (`Place`, `AdministrativeArea`, `CivicStructure`, `City`). |
| 7 | `organization-schema-org.owl` | schema.org `Organization` module | covered | Module of #3 (`EducationalOrganization`, `School`). |
| 8 | `intangible-schema-org.owl` | schema.org `Intangible` module | covered | Module of #3; no separate import needed. |
| 9 | `schema_ontologies-zip.zip` | packaging of modules #4–#8 | covered | Same content as #3, in archive form. |
| 10 | `dublincore.owl` | Dublin Core metadata | referenced | Not imported into the KG (it is a metadata vocabulary, not a domain one), but its terms are the ones used in the `Metadata/` section of the report and in the `dcterms:source` annotation of the ontology header. |
| 11 | `time.n3` | OWL-Time | **no** — documented limitation | The school-count datasets are tied to the 2012/2013 school year, but the year is recorded as a `rdfs:comment`, not modelled as a time entity. Adding `time:Interval` nodes would be a correct extension; it was not done because the sources carry no other temporal information (no validity dates per school). Recorded in the report as an open issue. |
| 12 | `osm.owl` | OpenStreetMap tagging ontology | **no** — redundant | `KTLO-KG.owl` is itself an OSM-derived taxonomy with the same POI vocabulary (`school`, `kindergarten`, `university`, `amenity`-style classes). Importing `osm.owl` as well would duplicate the same classes under two IRIs. |
| 13 | `GTFS.n3` | public-transport feed ontology | **no** — out of scope | Belongs to project 5 (Transportation and Education Facilities). Project 6 has no transit dataset and CQ2 asks for proximity to towns and landmarks, not reachability. |
| 14 | `foaf.n3` | FOAF (people, agents) | **no** — no person data | The selected datasets describe facilities and municipalities, not people. Persons appear only in the source `responsibles` files, which are outside the project scope. |
| 15 | `event-schema-org.owl` | schema.org `Event` module | **no** — no events | No event data in the eight selected tables. |
| 16 | `action-schema-org.owl` | schema.org `Action` module | **no** — no actions | The KG is descriptive (which facility is where, how many), it does not model enrolment or commute actions. |
| 17 | `creativeworks-schema-org.owl` | schema.org `CreativeWork` | **no** — off-domain | No documents, courses or publications in the selected tables. |
| 18 | `product-schema-org.owl` | schema.org `Product` | **no** — off-domain | No commercial entities. |
| 19 | `biochemistry-schema-org.owl` | schema.org biochemistry | **no** — off-domain | No life-science data. |
| 20 | `medical-schema-org.owl` | schema.org medical | **no** — off-domain | No health data (project 3 covers health facilities). |

Rule applied: modules of `full-schema-org.owl` are covered by the single full import;
fragments whose domain does not exist in the eight selected tables are excluded rather
than imported "for completeness", because a KG that imports ten unrelated vocabularies
is harder to validate, not richer.

---

## 2. How the alignment was expressed

Only two mechanisms are used: `rdfs:subClassOf` for classes and `rdfs:subPropertyOf`
for properties. No instance was re-typed and no triple of the entity graph was rewritten.

| Project term | Aligned to | Note |
|---|---|---|
| `edu:Municipality` | `ktlo:places`, `schema:AdministrativeArea` | a municipality is both a populated place and an administrative unit |
| `edu:EducationalInstitution` | `ktlo:school`, `schema:EducationalOrganization` | `ktlo:school ⊑ ktlo:education ⊑ ktlo:public ⊑ ktlo:pois ⊑ ktlo:openstreetmap_place` |
| `edu:Kindergarten` | `ktlo:kindergarten` | sibling of `ktlo:school` under `ktlo:education` |
| `edu:HigherEducationInstitution` | `ktlo:university`, `schema:CollegeOrUniversity` | |
| `edu:PrimarySchool`, `edu:SecondarySchool` | inherited from `edu:EducationalInstitution` | the KTLO has no separate primary/secondary classes (its `primary`/`secondary` classes are OSM *road* types and must not be reused) |
| `edu:GeoFeature` | `ktlo:openstreetmap_place` | keeps the geographical branch inside the supplied ontology |
| `edu:NaturalLandmark` | `ktlo:natural` | class only — no landmark coordinates exist in the sources |
| `edu:name` | `ktlo:name` | all project classes end up below `ktlo:openstreetmap_place`, so the KTLO domain restriction holds |
| `edu:istatCode` | `ktlo:code` | the official identifier of a municipality |
| `edu:locatedIn` | `schema:containedInPlace` | domain/range are compatible (`EducationalInstitution` is a `School`, hence a `CivicStructure`, hence a `Place`) |

Deliberately **not** aligned, with reasons (these are project extensions):

- `edu:near` — the KTLO has no spatial relation at all (it is a taxonomy, not a
  relation ontology), and no fragment offers a "near" property;
- `edu:hasEducationalInstitution` — inverse of `locatedIn`, no counterpart;
- `edu:address`, `edu:area` — `schema:address` and `schema:area` are declared as **object**
  properties in `full-schema-org.owl` (schema.org allows both a literal and an entity
  value), so a datatype property cannot be declared a sub-property of them without
  making the ontology inconsistent;
- `edu:ownershipType`, `edu:schoolLevel`, and the four `*Count` properties — no
  counterpart in the fragments (`ktlo:population` counts residents, not facilities).

---

## 3. Verification performed

Parsing `education-facilities-ontology-aligned.ttl` with rdflib 7.6.0, merging it with the
three imported fragments:

- aligned ontology: **118 triples** (92 in the inception version + 26 alignment axioms);
- `owl:imports` resolved to the three fragment IRIs;
- ancestor check, **8 of 8** project classes land in the expected supplied class:

```
Municipality               -> ktlo:places, schema:AdministrativeArea, ktlo:openstreetmap_place
EducationalInstitution     -> ktlo:school, schema:EducationalOrganization, ktlo:openstreetmap_place
Kindergarten               -> ktlo:kindergarten, ktlo:openstreetmap_place
HigherEducationInstitution -> ktlo:university, schema:CollegeOrUniversity, ktlo:openstreetmap_place
PrimarySchool              -> ktlo:school, ktlo:openstreetmap_place
SecondarySchool            -> ktlo:school, ktlo:openstreetmap_place
GeoFeature                 -> ktlo:openstreetmap_place
NaturalLandmark            -> ktlo:natural, ktlo:openstreetmap_place
```

- entity graph unchanged: **2347 triples**, and every property/class it uses is declared
  by the ontology (no undeclared terms);
- the competency questions return the same numbers as before the alignment:
  CQ1 Trento → 42 institutions, CQ3 → 162 municipalities with primary schools,
  CQ4 → 175 of 176 municipalities without a higher-education institution.

---

## 4. Where the imported files live

`owl:imports` points to the repository, so the fragments must be present in the repo:

```
fragments/KTLO-KG.owl
fragments/LTLO-KG.owl
fragments/full-schema-org.owl
```

The folder name is lowercase in the repository and the `owl:imports` IRIs match it
exactly (raw.githubusercontent.com is case-sensitive).

They are redistributed here unchanged, only so that the ontology resolves without
requiring access to the course Drive folder.

## 5. What this does not fix

- CQ2 ("facilities near towns or natural landmarks") stays unanswerable: the KTLO offers
  classes for `natural` features but no coordinates, and none of the supplied datasets
  contains positions. The alignment makes the gap *explicit in the model*, it does not
  close it.
- `time.n3` is not used; school-year is a comment. If the report needs a temporal
  dimension, that is an extension, not a defect of the alignment.
