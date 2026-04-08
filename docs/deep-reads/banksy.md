# BANKSY

**Verdict:** The strongest expression-based niche method. The azimuthal gradient feature is a genuine conceptual advance that captures tissue boundaries invisible to other expression methods.

**Citation:** Singhal V, et al. "BANKSY unifies cell typing and tissue domain segmentation for scalable spatial omics data analysis." *Nature Genetics* 56:431-441, 2024. [DOI](https://doi.org/10.1038/s41588-024-01664-3)

## Problem Setup

Can a single framework perform both cell typing and spatial domain/niche identification by incorporating spatial context into cell embeddings?

## Niche Definition

**Expression-based:** Niche = each cell's expression augmented with two spatial features: (1) the mean expression of spatial neighbors, and (2) the azimuthal Gabor gradient of neighbor expression.

The gradient is what makes BANKSY unique. While the mean captures bulk neighborhood effects, the gradient captures where expression changes rapidly — tissue interfaces, boundaries between regions, and transition zones.

## Architecture

1. For each cell, compute its own expression vector.
2. Compute the mean expression of cells within a specified radius.
3. Compute the azimuthal Gabor gradient — a directional derivative that detects expression boundaries.
4. Concatenate: `[cell_expression, lambda * neighbor_mean, lambda * neighbor_gradient]`.
5. Apply standard clustering (Leiden) to the augmented feature matrix.

The `lambda` parameter controls the weight of spatial versus cell-intrinsic features. At `lambda=0`, BANKSY reduces to standard expression-based clustering.

## Evaluation

- Benchmarked on DLPFC (domain detection), mouse brain MERFISH, and mouse embryo seqFISH.
- Compared against spatial methods (SpaGCN, BayesSpace, STAGATE) and non-spatial clustering.
- Scales to millions of cells.

## Honest Assessment

**Strengths:**

- The gradient feature is a genuine advance — it captures tissue interfaces that mean-based methods miss entirely. Biologically, interfaces (tumor margins, tissue boundaries) are often the most important niches.
- Simple, fast, scalable. No deep learning required.
- The `lambda` parameter provides an interpretable knob: more spatial vs more expression-driven.
- Principled mathematical framework rooted in spatial statistics.

**Limitations:**

- The `lambda` parameter must be chosen, and the optimal value varies by dataset and biological question. No principled default.
- On DLPFC (a domain benchmark), BANKSY performs well but this tests domain detection, not niche identification.
- The gradient is sensitive to cell density — sparse regions may have noisy gradient estimates.
- Does not distinguish between different causes of expression gradients (biological boundary vs technical artifact).

**Design Decision:** BANKSY's key insight is that boundaries matter as much as regions. Most spatial methods average over neighbors, which blurs boundaries. The gradient feature preserves them. This makes BANKSY uniquely suited for studying tissue interfaces — exactly where many biologically important niches live.
