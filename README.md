# snRNA-seq Analysis of Parkinson's Disease — Human Midbrain

![Pipeline](https://img.shields.io/badge/pipeline-Scanpy%20%7C%20Harmony%20%7C%20Leiden%20%7C%20RandomForest-blue)
![Dataset](https://img.shields.io/badge/dataset-GSE157783%20%7C%201000%20Genomes-green)
![Status](https://img.shields.io/badge/status-complete-brightgreen)
![Platform](https://img.shields.io/badge/platform-Google%20Colab-orange)
![AUC](https://img.shields.io/badge/AUC-0.898%20±%200.005-red)

## Overview

End-to-end single-nucleus RNA sequencing (snRNA-seq) analysis pipeline for Parkinson's disease (PD) classification using post-mortem human midbrain tissue.  
Integrates unsupervised transcriptomic analysis with a supervised Random Forest machine learning classifier.

**Dataset:** GSE157783 — Human substantia nigra midbrain (NCBI GEO)  
**Scope:** 11 donors (5 PD, 6 Control), 39,606 nuclei after QC  
**Technology:** 10x Genomics Chromium snRNA-seq

---

## Clinical Question

> *Can single-nucleus transcriptomic profiles distinguish Parkinson's disease nuclei from healthy controls, and what genes drive this classification?*

---

## Pipeline Summary

```
GSE157783 (NCBI GEO)
      │
      ▼
Data Loading & AnnData Construction
  └── 41,434 raw nuclei → 39,606 after QC
      │
      ▼
Quality Control
  ├── Min genes/nucleus : 200
  ├── Max genes/nucleus : 6,000
  └── Min cells/gene    : 10
      │
      ▼
Normalization + HVG Selection
  └── 2,000 highly variable genes
      │
      ▼
PCA → Harmony Batch Correction
  └── 40 PCs, batch = donor (sample_id)
      │
      ▼
Leiden Clustering (resolution 0.5)
  └── 21 transcriptionally distinct clusters
      │
      ▼
UMAP Visualization
  ├── Cell clusters (21 Leiden)
  └── PD vs Control overlay
      │
      ▼
Cell Type Annotation
  └── 12 major brain cell populations
      │
      ▼
Dopaminergic Neuron Analysis
  └── 47 DaN nuclei (0.119%) — consistent with PD neurodegeneration
      │
      ▼
Random Forest Classifier (5-fold CV)
  └── Mean AUC = 0.898 ± 0.005
```

---

## Key Findings

| Metric | Value |
|--------|-------|
| Nuclei after QC | 39,606 |
| Leiden clusters | 21 |
| Cell types identified | 12 |
| Dopaminergic neurons found | 47 (0.119%) |
| Random Forest AUC | **0.898 ± 0.005** |
| Control accuracy | 3993 / 4000 correct |
| PD accuracy | 3650 / 4000 correct |

### Cell Type Composition

| Cell Type | Nuclei | % |
|-----------|--------|---|
| Oligodendrocytes | 21,192 | 53.5 |
| Astrocytes | 4,687 | 11.8 |
| Microglia | 3,899 | 9.8 |
| OPCs | 2,751 | 6.9 |
| Excitatory Neurons | 2,062 | 5.2 |
| Endothelial Cells | 1,723 | 4.3 |
| Pericytes | 1,228 | 3.1 |
| Inhibitory Neurons | 922 | 2.3 |
| Ependymal Cells | 531 | 1.3 |
| GABA Neurons | 444 | 1.1 |
| CADPS2+ Neurons | 120 | 0.3 |
| **Dopaminergic Neurons** | **47** | **0.12** |

---

## Figures

| Figure | Description |
|--------|-------------|
| `results/figures/qc_violin.png` | QC metrics — gene counts and total counts before filtering |
| `results/figures/umap_clusters.png` | UMAP — 21 Leiden clusters coloured by cluster ID |
| `results/figures/umap_pd_vs_control.png` | UMAP — PD vs Control nuclei overlay |
| `results/figures/dan_proportion_boxplot.png` | Boxplot — dopaminergic neuron proportion per donor |
| `results/figures/roc_curve.png` | ROC curve — Random Forest 5-fold CV (AUC = 0.898) |
| `results/figures/feature_importance.png` | Top 20 genes by Random Forest feature importance |
| `results/figures/confusion_matrix.png` | Confusion matrix — PD vs Control final classifier |

---

## Tools & Versions

| Tool | Version | Purpose |
|------|---------|---------|
| scanpy | 1.10.x | Core snRNA-seq analysis framework |
| anndata | 0.10.x | Annotated data matrix format |
| harmonypy | 0.0.9 | Multi-donor batch correction |
| scikit-learn | 1.4.x | Random Forest classifier and evaluation |
| leidenalg | 0.10.x | Leiden clustering algorithm |
| pandas | 2.x | Data manipulation |
| numpy | 1.26.x | Numerical computation |
| scipy | 1.12.x | Sparse matrix handling |
| matplotlib | 3.8.x | Figure generation |
| seaborn | 0.13.x | Statistical visualisation |
| python-igraph | 0.11.x | Graph construction for clustering |
| Python | 3.11 | Core environment |

---

## Repository Structure

```
snrna-pd-analysis/
├── README.md                          # This file
├── environment.yml                    # Conda environment
├── .gitignore                         # Excludes large data files
│
├── data/
│   └── download_gse157783.sh          # Download script for GSE157783
│
├── workflow/
│   └── miniproject_snrna_pd.py        # Full pipeline script (Colab-ready)
│
├── results/
│   ├── qc/
│   │   └── qc_summary.md              # QC filtering statistics
│   └── figures/
│       ├── qc_violin.png
│       ├── umap_clusters.png
│       ├── umap_pd_vs_control.png
│       ├── dan_proportion_boxplot.png
│       ├── roc_curve.png
│       ├── feature_importance.png
│       └── confusion_matrix.png
│
├── notebooks/
│   └── README.md                      # Colab notebook instructions
│
└── report/
    └── clinical_interpretation.md     # Project report summary
```

---

## How to Reproduce

### Option A — Google Colab (recommended)

1. Open `workflow/miniproject_snrna_pd.py` in Google Colab
2. Download GSE157783 files from NCBI GEO (see `data/download_gse157783.sh`)
3. Run all cells in order
4. Runtime: ~45–60 minutes on Colab free tier

### Option B — Local

```bash
git clone https://github.com/YOUR_USERNAME/snrna-pd-analysis
cd snrna-pd-analysis

conda env create -f environment.yml
conda activate snrna-pd

bash data/download_gse157783.sh
python workflow/miniproject_snrna_pd.py
```

---

## Data Sources

| Resource | URL |
|----------|-----|
| GSE157783 dataset | https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE157783 |
| Original paper (Kamath et al. 2022) | https://doi.org/10.1038/s41593-022-01061-1 |
| Scanpy documentation | https://scanpy.readthedocs.io |
| Harmony paper | https://doi.org/10.1038/s41592-019-0619-0 |

---

## Limitations & Notes

- Dataset restricted to **substantia nigra / midbrain region** only
- Parental genotypes are from **post-mortem tissue** — donor-specific batch effects corrected via Harmony
- Gene identifiers remain as **Ensembl IDs** — direct mapping to gene symbols (SNCA, LRRK2) was limited by dataset format
- Dopaminergic neuron count (47 nuclei) is extremely low — statistics should be interpreted cautiously
- This is a **training/portfolio project** — not validated for clinical use

---

## Skills Demonstrated

- snRNA-seq preprocessing and quality control (Scanpy)
- Multi-donor batch correction (Harmony)
- Dimensionality reduction: PCA + UMAP
- Graph-based clustering: Leiden algorithm
- Cell type annotation using marker genes
- Supervised ML classification (Random Forest, 5-fold CV)
- ROC-AUC evaluation and feature importance analysis
- Reproducible cloud-based bioinformatics (Google Colab)

---

*Dataset: GSE157783, publicly available via NCBI GEO. Not for clinical use.*
