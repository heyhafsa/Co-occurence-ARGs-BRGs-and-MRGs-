Co-occurence-ARGs, BRGs and MRGs
A continuation to Beatrice Cornu Hewitts' analysis on antimicrobial resistance gene co-occurence networks and patterns. Highly recommended to follow this analysis before the following one for better understanding and visualization of the data 
Rscripts can be found here: https://github.com/BeatriceCornuHewitt/VGO_COPDcaco_resistome 

This repository extends the methodology of Cornu Hewitt et al. (2024, JAC) to include metal and biocide resistance gene data, examining co-selection between antimicrobial, metal, and biocide resistance.

1. Overview

The pipeline takes a phyloseq object of gene abundances (FPKM, qPCR-normalised) and produces:
- Pairwise Spearman correlation networks for every gene-type combination (ARG-ARG, MRG-MRG, BRG-BRG, MRG-ARG, MRG-BRG, ARG-BRG)
- A combined "ALL" network across all gene types
- Edge tables and summary statistics for each network
- `.graphml` files ready for visualisation in Gephi

Significant co-occurrence is defined using the strict threshold from Cornu Hewitt et al. (2024): Spearman |ρ| ≥ 0.6 and Benjamini-Hochberg FDR-adjusted p < 0.01.

2. Structure

├── scripts/
│   ├── 01_load_data.R
│   ├── 02_prevalence_filter.R
│   ├── 03_spearman_correlation.R
│   ├── 04_fdr_correction.R
│   ├── 05_cluster90_aggregation.R
│   ├── 06_network_export.R
│   └── 07_all_network_summary.R
├── Output_files/
│   ├── network_figures/      # example rendered Gephi network images
│   └── tables/                # edge tables and summary statistics
└── README.md
```

3. Dependencies

- R (≥ 4.2)
- R packages: `phyloseq`, `Hmisc`, `igraph`, `dplyr`, `scales`, `ggplot2`, `gridExtra`
- [Gephi](https://gephi.org/) (0.10+) for network visualisation

Install R package dependencies:

```r
install.packages(c("Hmisc", "igraph", "dplyr", "scales", "ggplot2", "gridExtra"))

if (!requireNamespace("BiocManager", quietly = TRUE)) install.packages("BiocManager")
BiocManager::install("phyloseq")
```

4. Data availability

This pipeline runs on a phyloseq object derived from the VGO-COPD cohort (patient-level resistome data) and is not included in this repository due to participant privacy. The expected input is:

07a_VGO-COPD-ResCap_phyloseq_ARGMetalBiocide-FPKM(qPCR)_2026-04-21.RDS
containing 1303 taxa (genes) across 72 samples, with a `tax_table()` including `Type` (Antibiotic / Metals / Biocides / Multi-compound), `Class`, and `Cluster90` columns.

To request access to the underlying data, please contact the study's principal investigators at IRAS, Utrecht University.

How to run

Run the scripts in numerical order 
Output `.graphml` files are saved to `~/Desktop/METALBIO/` by default. Graphml files are only opened via Gephi 

5. Visualising networks in Gephi
- Open a `.graphml` file in Gephi
- Apply the Fruchterman-Reingold layout
- Size nodes by degree
- Colour edges by the `direction` attribute (positive/negative correlation) and use RHO for thickness 
- Colour nodes by the `color_by` attribute (Class for within-type networks, GeneType for cross-type networks)

Result previews can be found in Output file 

Citation

This pipeline builds on the methodology described in:

Cornu Hewitt, B., Bossers, A., van Kersen, W., de Rooij, M. M. T., & Smit, L. A. M. (2024). Associations between acquired antimicrobial resistance genes in the upper respiratory tract and livestock farm exposures: a case-control study in COPD and non-COPD individuals. The Journal of antimicrobial chemotherapy, 79(12), 3160–3168. https://doi.org/10.1093/jac/dkae335

Author

Hafsa Benouahi
MSc Biomedical Sciences (International Public Health), Vrije Universiteit Amsterdam
Thesis internship — Institute for Risk Assessment Sciences (IRAS), Utrecht University)
