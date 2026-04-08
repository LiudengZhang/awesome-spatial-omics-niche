# ENVI / COVET

**Verdict:** Conceptually the most sophisticated niche definition — niche as covariance tensor, not just mean. Captures regulatory structure invisible to simpler methods, at the cost of computational complexity.

**Citation:** Haviv D, et al. "The covariance environment defines cellular niches for spatial inference." *Nature Biotechnology* 42:1830-1842, 2024. [DOI](https://doi.org/10.1038/s41587-024-02193-4)

## Problem Setup

Can we define cell niches using the gene-gene covariance structure of their spatial neighbors, rather than just neighbor composition or mean expression? And can this richer niche representation improve imputation of whole transcriptomes from spatial data?

## Niche Definition

**Covariance-based:** Niche = the gene-gene covariance tensor computed over a cell's spatial neighbors. Two neighborhoods with the same mean expression but different correlation structure represent different niches.

This is a fundamentally richer definition than composition-based (who is nearby) or expression-based (what is expressed nearby). It captures how genes co-vary with each other in the local environment.

## Architecture

**COVET (COVariance Environment Tensor):**

1. For each cell, identify its spatial neighbors.
2. Compute the gene-gene covariance matrix across those neighbors for a selected set of genes.
3. This covariance matrix is the COVET niche representation.

**ENVI (ENVironment Imputation):**

1. Train on paired spatial + scRNA-seq data from the same tissue.
2. Use COVET niche representations as conditioning variables in a conditional VAE.
3. The VAE learns to impute full transcriptomes from spatial data conditioned on the local niche covariance.

## Evaluation

- Demonstrated on mouse brain (MERFISH + scRNA-seq), mouse intestine (Slide-seq + scRNA-seq), and other tissues.
- Compared against imputation methods that use simpler niche definitions (mean expression, composition).
- COVET-conditioned imputation outperforms alternatives, demonstrating that covariance carries biological information beyond means.

## Honest Assessment

**Strengths:**

- Theoretically compelling: two neighborhoods with the same average gene expression but different regulatory structure (different covariances) are biologically different. COVET captures this.
- ENVI's imputation is a practical killer application — enables whole-transcriptome analysis from gene-panel spatial data.
- From Dana Pe'er's lab (MSKCC), with strong theoretical foundations.

**Limitations:**

- The covariance tensor grows quadratically with the number of genes. Practical only with gene subsets (typically ~50-200 genes) or dimensionality reduction.
- Covariance estimation requires enough cells in each neighborhood for stable estimates — sparse regions may produce noisy tensors.
- Harder to interpret biologically than composition or mean expression. What does it mean that genes A and B are correlated in neighborhood X but not Y?
- Computationally more demanding than simpler niche methods.

**Design Decision:** COVET bets that regulatory structure (covariance) is more biologically meaningful than snapshot statistics (means, proportions). This is likely true — gene regulation is fundamentally about co-expression patterns. But the practical cost of computing and interpreting covariance tensors limits adoption.
