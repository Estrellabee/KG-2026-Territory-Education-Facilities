# Phase 4 — Entity Definition (mapping specification and resulting graph)

> Project #6 *Territory and Education Facilities*, KG 2026, University of Trento.
> This document specifies how each cleaned source table is turned into RDF
> triples, and reports the resulting graph together with its validation.

## 4.1 Sources after cleaning

| # | Source (raw) | Cleaned file | Rows | Content |
|---|---|---|---|---|
| 1 | Trentino Commune List | `commune-list.csv` | 176 | municipality name, ISTAT code, area |
| 2 | educational_institutions | `institutions.csv` | 47 | institution name, ownership type, address, municipality |
| 3 | school_list_cleaned_with_id_institute_number | `kindergarten-directory.csv` | 153 | kindergarten name, address, school level |
| 4 | number_scuole_infanzia | `count-kindergarten.csv` | 162 | kindergartens per municipality (2012/13) |
| 5 | number_scuole_elementari | `count-primary.csv` | 162 | primary schools per municipality (2012/13) |
| 6 | number_scuole_medie | `count-lower-secondary.csv` | 162 | lower secondary schools per municipality (2012/13) |
| 7 | numero_scuole_superiori_e_professionali | `count-upper-secondary.csv` | 162 | upper secondary schools per municipality (2012/13) |
| 8 | unitn_institute_final.ttl | — (already RDF) | 1 | University of Trento |

## 4.2 URI policy

All instances live in the namespace `http://kg2026.unitn.it/resource/`, with one
sub-namespace per entity kind so that two different kinds can never share an
identifier:

```
resource/municipality/<name>      e.g. resource/municipality/Trento
resource/institution/<name>       e.g. resource/institution/ISTITUTO%20COMPRENSIVO%20ROVERETO%20EST
resource/kindergarten/<name>      e.g. resource/kindergarten/Scuola%20dell'infanzia%20di%20Vigo%20di%20Fassa
resource/university/<name>
```

Names are percent-encoded so that spaces, apostrophes and accented letters
produce legal IRIs. The mapping tool used during development wrote
`http://localhost:8080/source/…` (the base URI of the running tool session);
a stable namespace is used in the deliverable instead, so the graph does not
depend on the machine or the session in which it was produced.

## 4.3 Mapping specification

### M1 — `commune-list.csv` → `edu:Municipality`

| Column | RDF | Datatype |
|---|---|---|
| `comune-name` | subject URI (`resource/municipality/…`), `rdf:type edu:Municipality`, `edu:has_name` | string |
| `comune-code` | `edu:has_istatCode` | string (identifier, not an integer) |
| `area` | `edu:has_area` | `xsd:decimal` |

### M2..M5 — `count-*.csv` → additional properties of the same municipality

| Source | Column | RDF predicate |
|---|---|---|
| `count-kindergarten.csv` | `count_kindergarten` | `edu:has_kindergartenCount` |
| `count-primary.csv` | `count_primary` | `edu:has_primarySchoolCount` |
| `count-lower-secondary.csv` | `count_lower_secondary` | `edu:has_lowerSecondarySchoolCount` |
| `count-upper-secondary.csv` | `count_upper_secondary` | `edu:has_upperSecondarySchoolCount` |

`municipality` produces the same subject URI as in M1 (same namespace +
identical name string), so the counts are attached to the entity already created
by the commune list — this is the join of the whole project. Values are
`xsd:integer`.

### M6 — `institutions.csv` → `edu:EducationalInstitution`

| Column | RDF |
|---|---|
| `institute_name` | subject URI (`resource/institution/…`), `rdf:type edu:EducationalInstitution`, `edu:has_name` |
| `institute_address` | `edu:has_address` (string) |
| `institute_type` | `edu:has_ownershipType` (string, enumerated `public` / `religious` / `lay`) |
| `institute_comune` | `edu:has_municipality` → URI of the municipality (`resource/municipality/…`) |

`edu:has_municipality` is the object property of the ontology; its object is the
municipality entity produced by M1, which is why the municipality strings had to
be normalised before mapping.

### M7 — `kindergarten-directory.csv` → `edu:Kindergarten`

| Column | RDF |
|---|---|
| `nome_istituto` | subject URI (`resource/kindergarten/…`), `rdf:type edu:Kindergarten`, `edu:has_name` |
| `indirizzo` | `edu:has_address` (string) |
| `tipo_instituto` | `edu:has_schoolLevel` (`kindergarten`) |
| *derived from `indirizzo`* | `edu:has_municipality` → municipality |

The source has no municipality column; each address ends with
`- <CAP> <MUNICIPALITY>`, so the municipality is parsed out of the address
(`… - 38068 ROVERETO` → `Rovereto`). 149 of 153 addresses were resolved; the
four remaining ones name municipalities created in 2020 that do not exist in
the commune list (see 4.6).

### M8 — `unitn_institute_final.ttl` → `edu:HigherEducationInstitution`

The file is already RDF (knowdive vocabulary). Its single institute
(Università degli studi di Trento, Via Calepina 14) is re-expressed in the
project ontology as `rdf:type edu:HigherEducationInstitution`,
`edu:has_name`, `edu:has_address`, `edu:has_schoolLevel "higher"` and
`edu:has_municipality` → `resource/municipality/Trento`.

## 4.4 Resulting graph

| Measure | Value |
|---|---|
| Triples produced by the mapping | 2 347 |
| Municipalities | 176 |
| Educational institutions | 47 |
| Kindergartens | 153 |
| Higher education institutions | 1 |
| Entities with an identifier | 377 |
| `edu:has_istatCode` / `edu:has_area` triples | 176 / 176 |
| count triples (per level) | 162 |

The delivered file adds one `rdfs:label` triple per entity (377 in total, equal to its `has_name`), so the repository holds **2 724** statements for the entity layer.

Example triples:

```turtle
res:municipality/Trento a edu:Municipality ;
    edu:has_name "Trento" ;
    edu:has_istatCode "22205" ;
    edu:has_area "157.9"^^xsd:decimal ;
    edu:has_kindergartenCount 41 ;
    edu:has_primarySchoolCount 32 ;
    edu:has_lowerSecondarySchoolCount 13 ;
    edu:has_upperSecondarySchoolCount 16 .
```

## 4.5 Validation

| Check | Result |
|---|---|
| Turtle parses (rdflib 7.6) | OK |
| `edu:has_municipality` objects that are not municipalities | 0 (no dangling link) |
| Empty `edu:has_name` literals | 0 |
| Municipalities without `edu:has_name` | 0 |
| Institutions without a municipality | 6 (12.8 %, see 4.6) |
| Kindergartens without a municipality | 4 (2.6 %, see 4.6) |

Sample SPARQL (executed on the graph):

```sparql
# CQ1  which institution types exist in Trento
SELECT ?cls (COUNT(?x) AS ?n) WHERE {
  ?x edu:has_municipality <http://kg2026.unitn.it/resource/municipality/Trento> .
  ?x a ?cls .
} GROUP BY ?cls
```
→ `EducationalInstitution 22`, `Kindergarten 19`, `HigherEducationInstitution 1`

```sparql
# CQ3  municipalities with most / fewest kindergartens
SELECT ?name ?n WHERE { ?m edu:has_name ?name ; edu:has_kindergartenCount ?n }
ORDER BY DESC(?n) LIMIT 5
```
→ `Trento 41`, `Rovereto 15`, `Pergine Valsugana 6`, `Predaia 6`, `Ala 5`;
the minimum is `0` in 25 municipalities.

```sparql
# CQ4  municipalities without any higher education institution
SELECT (COUNT(?m) AS ?n) WHERE {
  ?m a edu:Municipality .
  FILTER NOT EXISTS { ?h a edu:HigherEducationInstitution ; edu:has_municipality ?m }
}
```
→ `175` (only Trento hosts a university).

## 4.6 Open issues (to be reported in Evaluation)

1. **Administrative vintage mismatch.** The commune list reflects the
   municipalities after the 2016 mergers but before 2020 (176 entities), while
   the counts and the kindergarten directory reflect the current 166. Ten
   municipalities cannot be reconciled: Strigno, Taio, Transacqua, Tuenno,
   Vezzano, Vigolo Vattaro (merged before 2020) and Borgo d'Anaunia, Novella,
   Terre d'Adige, Ville di Fiemme (created in 2020). Six institutions and four
   kindergartens therefore carry no `has_municipality` link, and the four count rows
   naming 2020 municipalities were dropped (162 rows per level).
2. **Counts are 2012/13, the directory is current.** For Trento the count table
   reports 41 kindergartens while the directory lists 19 entities: the two
   sources describe different school years. Counts are used for ranking (CQ3)
   and existence per level (CQ1/Sup6), the directory for named kindergarten
   entities; the two are not expected to agree row by row.
3. **No coordinates, no landmark data.** `edu:has_proximity` and `edu:NaturalLandmark`
   remain in the ontology but have no instances, so CQ2 is not answerable with
   the collected sources (it stays an acknowledged limitation, as declared in
   Phase 1).
4. **The institutions table carries no school level**, only the ownership type;
   which *levels* a municipality offers is answered through the counts, not
   through the institution names.
