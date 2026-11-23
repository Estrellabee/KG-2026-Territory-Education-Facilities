# Phase 2 — Language Definition: UKC alignment (course three-tier rule)

This note records the terminology alignment for the *Territory and Education Facilities*
project following the procedure taught in **KG-L17 Language Definition**, slide
"UKC Coverage and Gaps": every term is first looked up in the **Universal Knowledge
Core (UKC)**, then in an external reference ontology, and only if both fail does the
project mint its own identifier.

Source: `https://ukc.datascientia.eu` (queries executed programmatically against the
public UKC concept API, November 2026).

## 1. Entity type terms

| Project term | Label searched in UKC | UKC concept | UKC gloss (English) | Tier |
|---|---|---|---|---|
| `Municipality` | municipality | **UKC-45537** | an urban district having corporate status and powers of self-government | 1 |
| `EducationalInstitution` | educational institution | **UKC-43985** | an institution dedicated to education | 1 |
| `Kindergarten` | kindergarten | **UKC-44658** | a preschool for children age 4 to 6 to prepare them for primary school | 1 |
| `PrimarySchool` | grade school | **UKC-44664** | a school for children where they receive the first stage of basic education | 1 |
| `SecondarySchool` | secondary school | **UKC-44031** | a school for students intermediate between primary school and tertiary school | 1 |
| `HigherEducationInstitution` | university | **UKC-24619** | a high school comprising many faculties, where specialists are trained | 1 |
| `GeoFeature` | geographical area | **UKC-45324** | a demarcated area of the Earth | 1 |
| `NaturalLandmark` | natural landmark, natural monument | *no match* | — | 2 → 3 |

Notes on the two non-trivial cases:

* **`PrimarySchool`** — UKC has no concept labelled `primary school` or
  `elementary school`; the equivalent sense exists under the label **`grade school`**
  (UKC-44664). The project therefore keeps its own term name but points at that concept.
* **`NaturalLandmark`** — UKC has `landmark` only in the anatomical, boundary-mark and
  historical-event senses, and no `natural landmark` / `natural monument` concept.
  Tier 2 (KTLO) provides the *place* branch but no landmark class, so the class stays a
  **custom project term** (tier 3), documented as a known UKC gap.

## 2. Data property terms

| Project term | Label searched in UKC | UKC concept | UKC gloss (English) | Tier |
|---|---|---|---|---|
| `has_name` | name | **UKC-2** | a word by which a person or thing is known | 1 |
| `has_address` | mailing address | **UKC-45005** | the address where a person or organization can be communicated with | 1 |
| `has_ownershipType` | ownership | **UKC-62515** | the state of being an owner of something | 1 |
| `has_schoolLevel` | level | **UKC-19654** | a relative position in a scale or hierarchy | 1 |
| `has_istatCode` | code | **UKC-33626** | a system of symbols used to represent information | 1 |
| `has_area` | area | **UKC-27784** | the extent of a 2-dimensional surface enclosed within a boundary | 1 |
| `has_kindergartenCount` | count | **UKC-3211** | the act of determining the quantity of something | 1 (root) |
| `has_primarySchoolCount` | count | **UKC-3211** | (same root concept) | 1 (root) |
| `has_lowerSecondarySchoolCount` | count | **UKC-3211** | (same root concept) | 1 (root) |
| `has_upperSecondarySchoolCount` | count | **UKC-3211** | (same root concept) | 1 (root) |

The four `*Count` properties are project-specific (tier 3 as *terms*), but they all hang
off the single UKC concept for counting, so they are anchored at tier 1 rather than
invented from nothing.

## 3. Object property terms

UKC is a *lexical* resource: it provides concepts and word senses, not property
relations. Reified relations therefore fall through to tier 2.

| Project term | Handling | Tier |
|---|---|---|
| `has_municipality` | aligned to `schema:containedInPlace` (kept from the inception ontology) | 2 |
| `has_proximity` | no equivalent in UKC or KTLO; declared as a custom relation, intentionally left unpopulated (no coordinate data exists in the sources) | 3 |
| `has_educationalInstitution` | declared as `owl:inverseOf has_municipality`, no independent alignment needed | — |

## 4. What changed in the deliverable set

| Item | Change |
|---|---|
| Ontology namespace | `http://kg2026.unitn.it/ontology/education#` → **`http://knowdive.disi.unitn.it/etype#`** (the base URI used by the course projects, KG-L17 p28) |
| Property names | all data/object properties now carry the `has_` prefix; `locatedIn` → **`has_municipality`**, `near` → **`has_proximity`** (KG-L17 p16 shows the same rename to match target-ontology conventions) |
| Class and property comments | each term now carries its UKC identifier and English gloss inside `rdfs:comment` |
| Entity IRIs | unchanged (`http://kg2026.unitn.it/resource/…`); the namespace convention applies to the *language terms*, not to the instances |

## 5. Verification after the rename

| Check | Value |
|---|---|
| Ontology triples | 92 → **105** (renamed, then annotated; no structural change) |
| Aligned ontology triples | 118 → **117** |
| Knowledge graph triples | **2,347** (identical count) |
| Entities | 176 Municipality / 47 EducationalInstitution / 153 Kindergarten / 1 HigherEducationInstitution |
| `has_municipality` links | **191**, all pointing to declared municipalities (0 dangling) |
| Count value triples | **648** |
| CQ1 (facilities in Trento) | **42** |
| CQ3 (municipalities covered by counts) | **162** |
| CQ4 (municipalities without a university) | **175** |
| Residual `kg2026.unitn.it/ontology` references | **0** |

The rename is purely lexical: not one triple was added or removed, and all four
competency questions return the same answers as before.

