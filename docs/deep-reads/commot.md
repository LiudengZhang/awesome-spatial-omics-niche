# COMMOT

**Verdict:** The most mathematically principled approach to spatial cell-cell communication; optimal transport elegantly handles spot-level data where other methods require single-cell resolution.

**Citation:** Cang Z, et al. "Screening cell-cell communication in spatial transcriptomics via collective optimal transport." *Nature Methods* 20:218-228, 2023. [DOI](https://doi.org/10.1038/s41592-022-01728-4)

## Problem Setup

How can we infer spatially-constrained cell-cell communication from spatial transcriptomics data, especially at spot-level resolution (Visium) where each measurement contains multiple cells?

## Niche Definition

**Communication-based:** Niche = the set of active ligand-receptor signaling pathways between a cell/spot and its spatial neighbors, inferred via optimal transport.

## Architecture

1. For each L-R pair, define a spatial communication cost matrix based on physical distances between spots.
2. Use optimal transport to find the most likely communication pattern — the assignment of ligand-expressing spots to receptor-expressing spots that minimizes total transport cost.
3. The transport plan reveals directionality: which spots send signals and which receive them.
4. Aggregate across L-R pairs to build a communication-defined niche profile per spot.

## Key Innovation

Optimal transport provides a principled way to handle the mixed-cell-type problem at spot level. Rather than assuming each spot is one cell type, OT distributes communication proportionally across the cell types present in each spot.

## Evaluation

- Validated on mouse embryo (seqFISH), mouse brain (MERFISH), and Visium datasets.
- Compared against CellChat and other CCC methods.
- Demonstrated discovery of spatially-restricted signaling pathways invisible to non-spatial methods.

## Honest Assessment

**Strengths:**

- Mathematically principled — optimal transport is a clean framework for spatial communication inference.
- Handles spot-level data natively, which most CCC methods cannot.
- Provides directionality — not just which cells communicate, but who sends and who receives.
- Reveals spatially-restricted signaling that non-spatial CCC methods miss.

**Limitations:**

- Computational cost scales with dataset size — OT can be expensive for large spatial datasets.
- Like all CCC methods, depends on L-R databases for interaction candidates.
- "Optimal" transport is a mathematical construction — real biological signaling may not follow the transport-cost-minimizing pattern.
- Does not account for signaling kinetics or pathway activation state.

**Design Decision:** COMMOT bets that optimal transport provides the right mathematical language for spatial communication. This is elegant: communication has a source, a target, and a cost (distance). OT naturally encodes all three. The weakness is that real biology is messier than any transport plan.
