# Phase 4 — Municipality Name Normalisation Note

> Purpose: make the municipality name in every cleaned table identical to the
> `comune-name` value in `commune-list.csv`, so that the `has_municipality` object
> property can be materialised in Karma (object URI = municipality URI).

## 1. Why this step was needed

Karma builds the object URI of `has_municipality` from the literal value of the source
column. The link therefore only exists if the two strings are byte-identical.
The raw sources used different conventions:

| Table | Convention found |
|---|---|
| `institutions.csv` | ALL CAPITALS (`ROVERETO`, `TRENTO`) |
| `count-*.csv` | ASCII apostrophe instead of accented letters (`Male'`), bilingual compound names (`Canazei-Cianacei`) |
| `commune-list.csv` | official Italian spelling, but the capital was truncated to `Trent` by the source export |

## 2. Rules applied

1. **Capital correction** — `commune-list.csv` ISTAT `22205` (`Trent`, 157.9 km²)
   is the city of Trento; renamed to `Trento` so that the 22 Trento institutions
   and the Trento counts can be linked.
2. **Case** — `ALL CAPITALS` → official mixed case (`ROVERETO` → `Rovereto`).
3. **Accents** — `'` after a vowel restored to the accented letter
   (`Male'` → `Malè`, `Fiave'` → `Fiavè`, `Baselga di Pine'` → `Baselga di Pinè`,
   `Panchia'` → `Panchià`, `Rovere' della Luna` → `Roverè della Luna`,
   `Ruffre'-Mendola` → `Ruffrè-Mendola`).
4. **Bilingual compound names** — only the Italian part is kept
   (`Canazei-Cianacei` → `Canazei`, `Moena-Moena` → `Moena`,
   `San Giovanni di Fassa-Sèn Jan` → `San Giovanni di Fassa`,
   `Campitello di Fassa-Ciampedel` → `Campitello di Fassa`,
   `Fierozzo-Vlarötz` → `Fierozzo`, `Frassilongo-Garait` → `Frassilongo`,
   `Luserna-Lusérn` → `Luserna`, `Mazzin-Mazin` → `Mazzin`,
   `Palù del Fersina-Palai en Bersntol` → `Palù del Fersina`,
   `Soraga di Fassa-Soraga` → `Soraga di Fassa`).
5. **No match** — the value is left **empty** instead of being linked to a
   non-existing entity, so no dangling URI is created.

## 3. Rows affected

| File | Rows | Linkable after normalisation | Left empty / dropped |
|---|---|---|---|
| `commune-list.csv` | 176 | 176 (1 name corrected) | 0 |
| `institutions.csv` | 47 | 41 | 6 left without `has_municipality` |
| `count-kindergarten.csv` | 162 | 162 | 4 rows dropped (formerly 166) |
| `count-primary.csv` | 162 | 162 | 4 rows dropped (formerly 166) |
| `count-lower-secondary.csv` | 162 | 162 | 4 rows dropped (formerly 166) |
| `count-upper-secondary.csv` | 162 | 162 | 4 rows dropped (formerly 166) |
| `kindergarten-directory.csv` | 153 | 153 | 0 |

Rationale for the two different treatments: in `institutions.csv` the row itself
is an institution, so it is kept and only the link is missing; in the
`count-*.csv` a row is nothing but "municipality + number", so a row without a
municipality would create an empty subject and is dropped instead.

Additional value normalisation done in the same pass:

- `institutions.csv` / `institute_type`: `Pubblico` / `Religioso` / `Laico` →
  `public` / `religious` / `lay`, matching the enumeration declared in the
  ontology;
- `kindergarten-directory.csv` / `tipo_instituto`: the single source value
  `Scuola dell'infanzia` → `kindergarten`, matching the `schoolLevel`
  enumeration.

All other columns and row counts are unchanged (verified against `.bak` copies
stored in `backup-before-name-normalization/`).

## 4. Open issue — different administrative vintage

`commune-list.csv` (176 municipalities) reflects the situation **after the 2016
mergers but before 2020**, while `count-*.csv` (166 municipalities) reflects the
**current** situation after the 2020 mergers. Ten municipalities cannot be
reconciled and are therefore not linked:

- present in the institutions list but no longer existing (merged before 2020):
  `Strigno` (→ Castel Ivano), `Taio` (→ Predaia), `Transacqua`
  (→ Primiero San Martino di Castrozza), `Tuenno` (→ Ville d'Anaunia),
  `Vezzano` (→ Vallelaghi), `Vigolo Vattaro` (→ Altopiano della Vigolana);
- present in the counts but created in 2020: `Borgo d'Anaunia`, `Novella`,
  `Terre d'Adige`, `Ville di Fiemme`.

Consequence to be reported in the Evaluation chapter: 6 institutions (12.8 %)
carry no `has_municipality` link, and 4 count rows per level (2.5 %) were dropped.
The remaining 162 municipalities of each count table are the ones present in
`commune-list.csv`; the ranking required by CQ3 is computed on those, and the
four 2020 municipalities (Borgo d'Anaunia, Novella, Terre d'Adige, Ville di
Fiemme) are explicitly out of scope because the municipality table does not
contain them.
