# Genomic Interaction Analysis

End-to-end analysis of **3D chromatin organization** from Hi-C data, integrated with CTCF motif scanning and 1D epigenomic tracks (ChIP-seq / ATAC-seq).

Course project for **Introduction to Bioinformatics and Computational Genomics** (WUT, 2026).

---

## Overview

Genome architecture is not linear. Chromosomes fold into compartments, **Topologically Associating Domains (TADs)**, and chromatin loops — structures that shape gene regulation. This project explores that hierarchy on real Hi-C data from the human lung fibroblast line **IMR-90** (Rao et al., *Cell* 2014, [GSE63525](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE63525)).

Focus region: **chr17:30–35 Mb** at **25 kb** resolution (with supporting views at 250 kb and 10 kb for loops).

---

## What We Did

| # | Module | What it covers |
|---|--------|----------------|
| 1 | **Hi-C visualization** | Load `.hic` → `.cool` / `.mcool`, Knight–Ruiz balancing, contact heatmaps (whole chr17 + zoomed region) |
| 2 | **TAD detection** | Custom **insulation score** and **directionality index**; compare against Arrowhead reference TADs (precision / recall, size distributions) |
| 3 | **CTCF motifs** | PWM motif scanning; enrichment of CTCF at TAD boundaries vs random genomic positions; strand orientation at loop anchors (convergence rule) |
| 4 | **1D integration** | ENCODE **CTCF ChIP-seq** + **ATAC-seq** for IMR-90; multi-track plots with Hi-C; pile-up enrichment at TAD boundaries (liftOver hg38→hg19 where needed) |
| 5 | **Chromatin loops** | Loop calling with **chromosight** on 10 kb matrices; overlap with official HiCCUPS loops from GSE63525 |

---

## Key Results (short)

- Hierarchical structure is visible: A/B-like checkerboard at coarse resolution, TAD squares along the diagonal at 25 kb.
- **Insulation score** recovers TAD size distributions close to the Arrowhead reference; **directionality index** trades higher precision for lower recall.
- CTCF motifs are enriched at TAD boundaries; loop anchors show the classic **convergent** orientation pattern (+ on the left anchor, − on the right).
- ChIP-seq peaks align visually with TAD boundaries; genome-wide-style enrichment is limited by the small regional sample (13 boundaries in 5 Mb).
- Loop calling: chromosight vs HiCCUPS on chr17 → **F1 ≈ 0.40** (recall ~0.68), consistent with published inter-caller agreement ranges.

Full figures, methods, and discussion are in the notebook and the presentation.

---

## Repository Contents

```
Genomic-Interaction-Analysis/
├── Project3.ipynb                  # Main analysis notebook
├── Assignment3.md                  # Original assignment brief
├── HiC_Analysis_Presentation.pptx  # Project presentation / report
├── requirements.txt
└── README.md
```

Large data files (`.hic`, `.cool`, BigWig, etc.) are downloaded by the notebook into a local `./data/` folder (~1.5 GB) and are **not** stored in git.

---

## Tech Stack

- **Python** + Jupyter
- [cooler](https://cooler.readthedocs.io/) / hic2cool — Hi-C matrices
- NumPy, SciPy, pandas, matplotlib, seaborn — analysis & plots
- biotite — sequence / PWM motif work
- pybigtools, pyliftover — 1D tracks & genome liftOver
- chromosight — chromatin loop detection

---

## Getting Started

### Prerequisites

- Python 3.10+
- Several GB of free disk space for Hi-C downloads
- Stable internet (first run downloads GEO / ENCODE files)

### Setup

```bash
git clone https://github.com/domanskis06/Genomic-Interaction-Analysis.git
cd Genomic-Interaction-Analysis

python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate

pip install -r requirements.txt
jupyter notebook Project3.ipynb
```

Run cells top-to-bottom. The first data-download cell is slow but only needs to complete once.

---

## Data Sources

| Data | Source |
|------|--------|
| In situ Hi-C (IMR-90, MAPQ ≥ 30, hg19) | GEO [GSE63525](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE63525) |
| Arrowhead TAD domains | GSE63525 supplement |
| HiCCUPS loop list | GSE63525 supplement |
| CTCF ChIP-seq / ATAC-seq (IMR-90) | ENCODE |

---

## Authors

- Mateusz Osak  
- Krzysztof Dąbrowski  
- Szymon Domański  

Warsaw University of Technology — *Wstęp do Bioinformatyki i Genomiki Obliczeniowej*, 2026.

---

## License

Shared for educational and portfolio purposes. Upstream data remain under their original GEO / ENCODE terms.
