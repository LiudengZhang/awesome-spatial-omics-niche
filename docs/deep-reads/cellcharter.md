# CellCharter

**Verdict:** The most complete composition-based niche method; multi-resolution, cross-platform, and principled clustering, though bounded by the quality of input cell-type annotations.

**Citation:** Varrone M, et al. "CellCharter reveals spatial cell niches associated with tissue remodeling and cell plasticity." *Nature Genetics* 56:1718-1727, 2024. [DOI](https://doi.org/10.1038/s41588-024-01835-0)

## Problem Setup

How can we identify spatial cell niches that are robust across different spatial resolutions, platforms (imaging vs sequencing), and tissues?

## Niche Definition

**Composition-based:** Niche = cell-type proportions in a graph neighborhood, learned through GNN message passing at multiple spatial scales, then clustered via Gaussian mixture models.

## Architecture

1. Build a spatial graph from cell coordinates (k-NN or radius-based).
2. Use graph neural network (GNN) message passing to aggregate cell-type composition at multiple neighborhood scales.
3. Encode aggregated compositions into a latent space.
4. Cluster the latent embeddings using a Gaussian mixture model with automatic model selection (BIC/AIC for choosing k).

## Evaluation

- Tested on CODEX (protein imaging), MERFISH (RNA imaging), Visium (sequencing), and Slide-seq (sequencing) datasets.
- Benchmarked against Schurch-style k-NN + k-means and other niche methods.
- Demonstrated cross-platform consistency — similar niches detected from different technologies on comparable tissues.

## Honest Assessment

**Strengths:**

- Multi-resolution: captures niches at different spatial scales rather than committing to a single neighborhood size.
- Cross-platform: validated on both imaging and sequencing data.
- Principled clustering: GMM with model selection avoids the arbitrary k-choice problem of Schurch et al.
- Well-engineered software with good documentation.

**Limitations:**

- Requires cell-type annotations as input — niche quality is bounded by annotation quality. If cell types are wrong or too coarse, niches will be wrong.
- The GNN architecture adds complexity over simpler approaches (k-NN + k-means) but the marginal improvement varies by dataset.
- Does not capture expression states or signaling — two neighborhoods with the same cell-type proportions but different activation states are treated identically.
