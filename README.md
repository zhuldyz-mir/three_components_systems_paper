# Modified Three-Component Concrete for Irrigation Systems: A Hybrid Bibliometric and Scoping Review

> Reproducible pipeline for a two-stage literature review combining quantitative bibliometric mapping (VOSViewer) with qualitative scoping analysis (QualCoder).

## Research Context

This review supports a doctoral dissertation on modified concrete for irrigation canal troughs based on a ternary binder system (Portland cement + Ekibastuz TPP fly ash + MKU-95 microsilica). The goal is to map the global research landscape on ternary cementitious systems and identify the gap at the intersection with irrigation/hydraulic applications.

## Methodology Overview

```
Stage 1: Bibliometric Scan                 Stage 2: Qualitative Scoping Review
┌─────────────────────────┐                ┌─────────────────────────────────┐
│ Scopus queries (Q1–Q4)  │                │ Candidate selection (top-N by   │
│         ↓               │                │ relevance scoring)              │
│ Raw CSV/RIS export      │                │         ↓                       │
│         ↓               │                │ Full-text PDF download          │
│ Cleaning & dedup        │                │         ↓                       │
│         ↓               │                │ QualCoder coding (deductive +   │
│ Percentile filtering    │   ────────►    │ inductive)                      │
│         ↓               │                │         ↓                       │
│ VOSViewer mapping       │                │ Thematic synthesis              │
│ (co-occurrence,         │                │         ↓                       │
│  co-citation,           │                │ Integration with bibliometric   │
│  biblio coupling)       │                │ findings                        │
└─────────────────────────┘                └─────────────────────────────────┘
```

## Project Structure

```
ternary-concrete-irrigation-review/
│
├── README.md
├── requirements.txt
├── LICENSE
│
├── 01_raw_data/
│   ├── scopus_Q1_ternary_binders.csv       # Query 1: ternary systems
│   ├── scopus_Q2_irrigation_concrete.csv    # Query 2: irrigation/hydraulic
│   ├── scopus_Q3_doe_optimization.csv       # Query 3: DOE/RSM in concrete
│   ├── scopus_Q4_particle_packing.csv       # Query 4: particle packing
│   └── scopus_citescore_2024.xlsx           # Scopus source list (CiteScore)
│
├── 02_cleaned_data/
│   ├── merged_deduplicated.csv              # All queries merged, deduped
│   ├── filtered_percentile35.csv            # After journal percentile filter
│   ├── cleaning_log.json                    # Dedup & filter statistics
│   └── query_overlap_matrix.csv             # Cross-query overlap analysis
│
├── 03_thesaurus/
│   ├── keyword_thesaurus.txt                # VOSViewer keyword normalization
│   ├── source_thesaurus.txt                 # Journal name normalization
│   └── cited_refs_thesaurus.txt             # Cited reference normalization
│
├── 04_vosviewer/
│   ├── maps/
│   │   ├── cooccurrence_keywords.png
│   │   ├── cooccurrence_overlay_year.png
│   │   ├── bibliographic_coupling_sources.png
│   │   ├── cocitation_references.png
│   │   └── *.vosviewer                      # Native VOSViewer project files
│   └── exports/
│       ├── co_occurrence/
│       │   ├── network.csv
│       │   ├── clusters.csv
│       │   └── overlay.csv
│       ├── co_citation/
│       └── bibliographic_coupling/
│
├── 05_candidate_selection/
│   ├── candidate_articles.csv               # Scored & ranked articles
│   ├── scoring_criteria.md                  # Scoring rubric documentation
│   ├── selected_articles.csv                # Final subset (15–25 papers)
│   └── prisma_flow.md                       # PRISMA 2020 flow diagram data
│
├── 06_qualcoder/
│   ├── pdfs/                                # Full-text PDFs of selected articles
│   ├── codebook.md                          # Codebook with code hierarchy
│   ├── qualcoder_project.qda               # QualCoder project file
│   └── exports/
│       ├── code_frequencies.csv
│       ├── code_relations_matrix.csv
│       └── coded_segments.csv
│
├── 07_results/
│   ├── figures/
│   │   ├── prisma_flowchart.png
│   │   ├── publication_trend.png
│   │   ├── top_journals.png
│   │   ├── top_countries.png
│   │   ├── keyword_evolution.png
│   │   └── thematic_map.png
│   ├── tables/
│   │   ├── corpus_statistics.csv
│   │   ├── cluster_summary.csv
│   │   └── qualitative_themes.csv
│   └── draft_sections/
│       ├── methodology.md
│       ├── bibliometric_results.md
│       └── thematic_synthesis.md
│
├── scripts/
│   ├── config.py                            # Paths, parameters, constants
│   ├── 01_merge_and_clean.py                # Merge queries, dedup, normalize
│   ├── 02_filter_percentile.py              # Filter by journal CiteScore percentile
│   ├── 03_corpus_statistics.py              # Descriptive stats & trend plots
│   ├── 04_prepare_vosviewer.py              # Generate thesaurus, prep files
│   ├── 05_score_candidates.py               # Score & rank for scoping review
│   ├── 06_generate_figures.py               # Publication plots & summary tables
│   └── utils.py                             # Shared helpers
│
└── docs/
    ├── scopus_search_strings.md             # Exact search queries used
    └── methodology_notes.md                 # Methodological decisions log
```

## Scopus Search Queries

All queries limited to `PUBYEAR > 2020 AND PUBYEAR < 2026`.

### Q1 — Ternary Binder Systems (fly ash + silica fume + cement)
```
TITLE-ABS-KEY(
  ("fly ash" OR "coal ash" OR "pulverized fuel ash")
  AND ("silica fume" OR "microsilica" OR "micro-silica" OR "condensed silica fume")
  AND (concrete OR "cementitious" OR "cement composite")
  AND ("ternary" OR "three-component" OR "ternary binder" OR "ternary blend"
       OR "supplementary cementitious materials" OR "SCM")
)
```

### Q2 — Irrigation / Hydraulic Concrete Durability
```
TITLE-ABS-KEY(
  (concrete OR "cementitious composite")
  AND ("irrigation" OR "canal" OR "hydraulic structure" OR "water conveyance"
       OR "canal lining" OR "flume" OR "aqueduct")
  AND ("durability" OR "frost resistance" OR "freeze-thaw" OR "impermeability"
       OR "water resistance" OR "sulfate resistance")
)
```

### Q3 — DOE / RSM Optimization in Cementitious Systems
```
TITLE-ABS-KEY(
  (concrete OR "cementitious")
  AND ("response surface" OR "Box-Behnken" OR "central composite design"
       OR "design of experiments" OR "mixture design")
  AND ("fly ash" OR "silica fume" OR "supplementary cementitious")
)
```

### Q4 — Particle Packing in Blended Concrete
```
TITLE-ABS-KEY(
  ("particle packing" OR "packing density" OR "particle size distribution")
  AND (concrete OR "cementitious" OR "cement")
  AND ("fly ash" OR "silica fume" OR "ternary" OR "binary")
)
```

## Scopus Export Settings

When exporting from Scopus, use:
- **Format:** CSV
- **Fields:** Citation information, Bibliographical information, Abstract & keywords, Include references
- Export each query separately into `01_raw_data/`

## Pipeline Execution

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Place Scopus CSV exports into 01_raw_data/
# 3. Place Scopus CiteScore spreadsheet as 01_raw_data/scopus_citescore_2024.xlsx

# 4. Run pipeline steps
python scripts/01_merge_and_clean.py       # → 02_cleaned_data/
python scripts/02_filter_percentile.py     # → 02_cleaned_data/filtered_percentile35.csv
python scripts/03_corpus_statistics.py     # → 07_results/figures/ & tables/
python scripts/04_prepare_vosviewer.py     # → 03_thesaurus/
python scripts/05_score_candidates.py      # → 05_candidate_selection/
python scripts/06_generate_figures.py      # → 07_results/figures/

# 5. Open filtered CSV in VOSViewer for bibliometric mapping
# 6. Import selected PDFs into QualCoder for qualitative coding
```

## Article Scoring Rubric

Each candidate article is scored on 5 criteria (0–5 scale):

| Criterion | Weight | Description |
|-----------|--------|-------------|
| Keyword overlap | ×3 | Number of dissertation keywords matched |
| Methodological similarity | ×2 | Uses DOE, RSM, optimization |
| Hydraulic/irrigation application | ×3 | Direct mention of canal, flume, hydraulic |
| Ternary composition | ×2 | Contains cement + fly ash + silica fume |
| Journal quality | ×1 | CiteScore percentile ≥ 50 |

Maximum score: 55. Target subset: top 15–25 articles.

## QualCoder Codebook

Hierarchical code structure for qualitative analysis:

```
A. Composition & Materials
   A1: Fly ash type (Class F / C / regional)
   A2: Silica fume type (densified / undensified)
   A3: Cement replacement level (%)
   A4: W/C ratio
   A5: Chemical admixtures

B. Research Methodology
   B1: Statistical design (Box-Behnken, CCD, factorial)
   B2: Optimization method (RSM, multi-objective)
   B3: Microstructural analysis (SEM, XRD, TGA, MIP)
   B4: Particle packing modeling

C. Concrete Properties
   C1: Compressive strength
   C2: Frost resistance
   C3: Water impermeability
   C4: Sulfate resistance
   C5: Shrinkage / creep

D. Application Domain
   D1: Irrigation / canals
   D2: Hydraulic structures (general)
   D3: Road / pavement
   D4: General construction

E. Research Gaps (stated by authors)
```

## Requirements

- Python ≥ 3.10
- VOSViewer ≥ 1.6.20
- QualCoder ≥ 3.5
- See `requirements.txt` for Python packages

## Citation

If you use this pipeline or dataset, please cite:

```bibtex
@misc{baikhodzhayeva2026ternary,
  title={Modified Three-Component Concrete for Irrigation Systems:
         A Hybrid Bibliometric and Scoping Review},
  author={Baikhodzhayeva, Zh.I.},
  year={2026},
  url={https://github.com/USERNAME/ternary-concrete-irrigation-review}
}
```

## License

This repository is licensed under MIT. The Scopus data is subject to Elsevier's terms of use.
