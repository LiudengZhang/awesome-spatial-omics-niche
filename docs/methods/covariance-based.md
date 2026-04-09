# Covariance-Based Methods

**Niche definition: gene-gene covariance structure across spatial neighbors.**

These methods go beyond averaging neighbor expression to capture how gene co-expression patterns change with spatial context. Two neighborhoods with the same mean expression but different correlation structures represent different niches.

## Key Methods

### ENVI / COVET

- **Paper:** [Nature Biotechnology, 2024](https://doi.org/10.1038/s41587-024-02193-4)
- **Code:** [github.com/dpeerlab/ENVI](https://github.com/dpeerlab/ENVI)
- **Niche definition:** COVET computes a gene-gene covariance tensor over each cell's spatial neighbors. ENVI then uses this niche representation to train a conditional VAE that imputes missing genes.
- **Key innovation:** Formalizes niche as covariance rather than mean — captures regulatory relationships, not just expression levels.
- **Strengths:** Rich niche representation that distinguishes neighborhoods with identical mean expression but different co-regulation patterns. ENVI's imputation enables whole-transcriptome analysis from spatial data.
- **Limitations:** Covariance tensor grows quadratically with the number of genes — practical only with gene subsets or dimensionality reduction. Requires careful gene selection.
- **Verdict:** Conceptually the most sophisticated niche definition. The covariance tensor captures biology that simpler methods miss, but at a computational cost.

### SIMVI

- **Paper:** [Nature Communications, 2025](https://doi.org/10.1038/s41467-025-56090-0)
- **Code:** [github.com/KlugerLab/SIMVI](https://github.com/KlugerLab/SIMVI)
- **Niche definition:** Separates intrinsic cell state from extrinsic niche effects via a structured variational autoencoder.
- **Key innovation:** Jointly models cell-intrinsic and niche-extrinsic factors to prevent confounding — a cell's expression in a niche is decomposed into what the cell would express anywhere plus what the niche adds.
- **Strengths:** Principled deconfounding of cell identity from niche effects. Integrated into the scvi-tools ecosystem.
- **Limitations:** Model identifiability — separating intrinsic from extrinsic requires assumptions that may not hold in all tissues.

### SPACE

- **Paper:** [Cell Systems, 2023](https://doi.org/10.1016/j.cels.2023.04.006)
- **Code:** [github.com/zhangqf-lab/SPACE](https://github.com/zhangqf-lab/SPACE)
- **Niche definition:** Identifies spatial gene expression patterns through spatially-aware clustering of local covariance.
- **Key innovation:** From the same lab as ENVI/COVET; focuses on discovering coherent spatial expression programs.
- **Strengths:** Reveals spatially structured gene programs that may correspond to niche-specific regulatory states.

## When to Use Covariance-Based Methods

**Best for:**

- Questions about how gene regulatory relationships change across tissue.
- Distinguishing niches that look similar in composition or mean expression but have different regulatory states.
- Imputing full transcriptomes from spatial data (ENVI).
- Separating cell-intrinsic from niche-extrinsic effects (SIMVI).

**Not ideal for:**

- Simple niche identification where cell-type proportions suffice.
- Datasets with few genes (covariance estimation requires enough genes for stable estimates).
- Quick exploratory analysis (computationally more demanding than composition or expression methods).
