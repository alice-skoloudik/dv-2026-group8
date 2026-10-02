# Exploration notes — Alice

Level of analysis: One row = one primary school (location) so **no pupil-level data**.

## Data files

- **Reference levels (`referentieniveaus_2024-2025.csv`)** — DUO: number of pupils per school reaching each standardised skill level in maths, reading and writing; the only test outcome comparable across the six test providers.
- **School advice (`schooladviezen_2024-2025.csv`)** — DUO: number of pupils per school receiving each secondary-school track advice (PRO … VWO).
- **Average test scores (`eindscores_2024-2025.csv`)** — DUO: average raw test score per school for the provider(s) it used; six non-comparable scales, only useful to identify the provider.
- **School weighting (`schoolweging_2022-2025.ods`)** — Education Inspectorate (computed by CBS): disadvantage score per school; a separate file because it is published by a different organisation (sheet `2024-2025` is used).

DUO publishes reference levels, advice and test scores as separate open datasets per topic; they share the same school IDs and can be joined directly.

## Variables

**Needed for RQ3**

| Name | Variable | Unit | Type | What it measures | File |
|---|---|---|---|---|---|
| School ID | `INSTELLINGSCODE` | — | chr | School code; join key | all DUO files |
| Location ID | `VESTIGINGSCODE` | — | chr | Location (building) of the school | all DUO files |
| School ID (weighting file) | `OVT` | — | chr, `"00AP\|C1"` | School + location code; part before `\|` = school ID | school weighting |
| School type | `SOORT_PO` | — | chr: `Bo` / `Sbo` | Regular vs. special primary school | all DUO files |
| Maths below basic level | `REKENEN_LAGER1F` | number of pupils | chr (contains `"<5"`) | Pupils below basic level (1F) in maths | reference levels |
| Maths basic level | `REKENEN_1F` | number of pupils | chr (contains `"<5"`) | Pupils at basic level (1F) in maths | reference levels |
| Maths target level | `REKENEN_1S` | number of pupils | chr (contains `"<5"`) | Pupils at target level (1S) in maths | reference levels |
| Reading below basic level | `LV_LAGER1F` | number of pupils | chr (contains `"<5"`) | Pupils below basic level (1F) in reading | reference levels |
| Reading basic level | `LV_1F` | number of pupils | chr (contains `"<5"`) | Pupils at basic level (1F) in reading | reference levels |
| Reading target level | `LV_2F` | number of pupils | chr (contains `"<5"`) | Pupils at target level (2F) in reading | reference levels |
| Advice per track | `PRO`, `VMBO_B`, `VMBO_B_K`, `VMBO_K`, `VMBO_K_GT`, `VMBO_GT`, `VMBO_GT_HAVO`, `HAVO`, `HAVO_VWO`, `VWO` | number of pupils | chr (contains `"<5"`) | Pupils per secondary-school advice, from lowest (PRO) to highest (VWO); `_`-joined = split advice | school advice |
| School weighting | `schoolweging` | index score ≈ 20–41 (CBS scale, no natural unit) | num | Disadvantage of the school's pupil population; higher = more disadvantaged | school weighting |
| Share maths target level | `pct_maths_1s` | proportion of groep 8 pupils, 0–1 | num | Share of pupils reaching the maths target level | derived |
| Share reading target level | `pct_reading_2f` | proportion of groep 8 pupils, 0–1 | num | Share of pupils reaching the reading target level | derived |
| Share HAVO+ advice | `pct_havo_plus` | proportion of groep 8 pupils, 0–1 | num | Share of pupils advised HAVO, HAVO/VWO or VWO | derived |
| Groep 8 size | `n_groep8` | number of pupils | num | Pupils with a track advice; for filtering/weighting | derived |

**Optional**

| Name | Variable | Unit | Type | What it measures | File |
|---|---|---|---|---|---|
| Mean advice | `mean_advice` | average track level, 1 (PRO) – 6 (VWO) | num | Average advice, PRO = 1 … VWO = 6 (split advices at midpoints) | derived |
| Writing levels | `TV_LAGER1F`, `TV_1F`, `TV_2F` | number of groep 8 pupils (children) | chr (contains `"<5"`) | Pupils below basic / at basic / at target level in writing | reference levels |
| Spread of disadvantage | `spreiding` | SD of pupils' individual weighting scores within the school (school weighting points; range ≈ 2.6–9.1) | num | How mixed the school's pupil population is in disadvantage; higher = more mixed | school weighting |
| Test provider | `provider` (from `*_AANTAL`) | category (IEP, Route 8, DIA, AMN, DOE, LIB) | factor, 6 levels | Which transfer test the school used | derived from average test scores |
| Province | `PROVINCIE` | category (12 provinces) | chr, 12 levels | Province of the school | all DUO files |
| Special-secondary advice | `VSO` | number of groep 8 pupils (children) | chr (contains `"<5"`) | Pupils advised special secondary education | school advice |
| No advice possible | `ADVIES_NIET_MOGELIJK` | number of groep 8 pupils (children) | chr (contains `"<5"`) | Pupils for whom no advice could be given | school advice |

## Terminology

**School system**
- **Groep 8** — final year of Dutch primary school (children aged ~12); all data refer to this year group only.
- **Regular vs. special primary school (`Bo` / `Sbo`)** — `Bo` = regular primary school; `Sbo` = school for children who need extra support.
- **School vs. location (`INSTELLING` / `VESTIGING`)** — one school can have several buildings (locations); each row is one location.

**Test**
- **Transfer test (*doorstroomtoets*)** — national test taken by all groep 8 pupils in February; it informs the advice for secondary school.
- **Test provider** — schools choose one of six approved tests (IEP, Route 8, DIA, AMN, DOE, LIB); their raw scores are on different scales and cannot be compared.
- **Reference levels** — standardised skill levels, defined the same way for every test, so they *can* be compared across providers:
  - **Below 1F** — below the basic level.
  - **1F** — basic level; almost every pupil should reach it.
  - **Target level** — higher level: **1S** in maths, **2F** in reading and writing.
- **Subjects** — maths (*rekenen*), reading (*leesvaardigheid*, `LV`), writing/language conventions (*taalverzorging*, `TV`).

**Secondary-school advice**
- **Advice** — the secondary-school track the primary school recommends for each pupil (final advice, after the test).
- **Tracks, lowest to highest:**
  - **PRO** — practical education, for pupils who need the most support.
  - **VMBO** — pre-vocational education, in three sub-tracks from practical to theoretical: **B** (basic), **K** (framework), **GT** (mixed/theoretical).
  - **HAVO** — senior general secondary education (prepares for applied universities).
  - **VWO** — pre-university education (most academic).
- **Split advice** — e.g. `HAVO_VWO`: the school could not choose between two adjacent tracks.
- **HAVO+** — HAVO, HAVO/VWO or VWO advice; the usual cut-off for an "academic" advice.
- **VSO** — special secondary education; outside the PRO–VWO ranking.

**Disadvantage**
- **School weighting (*schoolweging*)** — score for how disadvantaged a school's pupil population is, based on parents' education, origin, length of stay in the Netherlands and debt restructuring; higher = more disadvantaged.
- **Spread (*spreiding*)** — how much pupils within one school differ in disadvantage.

**Sources and data conventions**
- **DUO** — Dutch government agency for education data; publishes the test and advice files.
- **CBS / Education Inspectorate** — Statistics Netherlands computes the school weighting; the Inspectorate publishes it.
- **`"<5"`** — a count of 1–4 pupils, hidden by DUO for privacy.

## Data issues

- **Hidden counts (`"<5"`)** — DUO suppresses counts of 1–4 in all pupil counts; ~91% of schools have at least one in the reference levels, ~99% in the advice. → Replace with 2.5; rerun with 1 and 4 as a robustness check (required in report).
- **Counts stored as text** — because of `"<5"`, all pupil counts (reference levels, advice per track) are read as character. → Convert to numeric after replacing `"<5"`.
- **Special primary schools (school type, `SOORT_PO` = `Sbo`)** — they have no school weighting (`schoolweging`) and no reference levels. → Keep regular schools (`Bo`) only.
- **Different school ID in the weighting file (`OVT`)** — its location suffix (`C1`, `C2`) does not match the location ID (`VESTIGINGSCODE`). → Split `OVT` on `|` and join on school ID (`INSTELLINGSCODE`).
- **Schools with multiple locations** — ~450 schools cannot be matched unambiguously to one school weighting (`schoolweging`) value. → Keep only schools with one location in both files.
- **Missing school weighting (`schoolweging`)** — 119 schools have `NA`. → Drop.
- **No reference-level data (`REKENEN_*`, `LV_*`)** — 161 schools have 0 pupils in all reference-level columns (very small groep 8). → Drop.
- **Small schools (groep 8 size, `n_groep8`)** — 771 schools have < 15 pupils, so their shares are noisy and heavily affected by `"<5"`. → Filter (e.g. n ≥ 15) or weight by n; decide together.
- **Split advices (`VMBO_GT_HAVO`, `HAVO_VWO`, …)** — these sit between two tracks, so it is unclear which side of the HAVO+ cut-off they belong to. → Decide classification; currently HAVO/VWO counts as HAVO+, VMBO-GT/HAVO does not.
- **Maths 2F level (`REKENEN_2F`)** — always 0 (the maths target level is 1S). → Ignore; share maths target level (`pct_maths_1s`) = maths target level (`REKENEN_1S`) / all maths pupils.
- **Final advice only (advice per track)** — advice includes upward revisions after the test; the original teacher advice is not available. → Mention as limitation.
- **School-level shares (`pct_*`)** — the same share at a level does not mean the same pupils (an advantaged school may have more pupils far above the threshold). → Interpret as school-level patterns; mention in robustness section.

## Ideas for exploratory plots

*(to be added after testing)*

## Conclusions

**Purpose of Part 1:** explore and understand the data first; answering RQ3 comes later (Part 2).

**Plots selected for Part 1** (from `exploration-alice.ipynb`)

- **Overview of single variables (Step 1)** — histograms of groep 8 size, school weighting, maths target level, reading target level and HAVO+ advice, plus the bar chart of advice per track. They give a quick overview of the data: many small schools, reading scores much higher than maths, HAVO+ around 44%, and 44% of pupils with a split advice.
- **Correlation heatmap (Step 2)** — summarises how all school variables relate: HAVO+ and mean advice are almost interchangeable (r = 0.90), and school weighting is related to advice at least as strongly as test results are.
- **Advice levels by test results and school weighting (Step 3)** — at similar test results, more disadvantaged schools give more PRO to VMBO-K advice and less VWO advice, and the gap is largest among schools with the highest test results.

**Not used:** scatterplots (hard to read with ~5,500 overlapping points).
