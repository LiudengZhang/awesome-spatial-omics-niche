# NicheCompass

**Verdict:** The most complete niche method currently available — integrates cell identity, spatial context, and ligand-receptor signaling into a single framework with cross-sample atlas building.

**Citation:** Lim J, et al. "NicheCompass: a graph neural network approach to niche identification using cell-cell communication." *Nature Genetics* 57:697-708, 2025. [DOI](https://doi.org/10.1038/s41588-025-02120-6)

## Problem Setup

Can we build a unified niche atlas across multiple samples and conditions by incorporating intercellular communication into niche embeddings?

## Niche Definition

**Communication-based (hybrid):** Niche = GNN-learned embedding that explicitly encodes ligand-receptor interaction features on spatial graph edges, combined with cell expression features on nodes.

This is a hybrid definition — it uses composition (who is nearby via graph structure), expression (node features), and communication (L-R edge features) simultaneously.

## Architecture

1. Build a spatial graph from cell coordinates.
2. Annotate graph edges with ligand-receptor interaction scores from curated databases.
3. Train a graph neural network that propagates both node (expression) and edge (L-R) features.
4. Learn a latent niche embedding per cell that captures cell identity, spatial context, and active signaling.
5. Enable cross-sample integration by aligning latent spaces across datasets.

## Evaluation

- Tested on multiple tissues including liver, intestine, and tumor samples.
- Cross-sample niche atlas construction demonstrated on datasets from different labs and platforms.
- Compared against composition-only and expression-only niche methods.

## Honest Assessment

**Strengths:**

- The most information-rich niche definition — incorporates who, what, and how cells interact.
- Cross-sample atlas building is a practical capability missing from most niche methods.
- L-R edge features add biological interpretability — you can inspect which signaling pathways define each niche.
- Well-engineered software from the Lotfollahi lab (same group as scArches, CellFlow).

**Limitations:**

- Niche definitions are constrained by the L-R database used. Novel or poorly annotated signaling pathways are invisible.
- L-R "activity" is inferred from co-expression, not measured directly. High expression of a ligand and receptor does not guarantee active signaling.
- The GNN + L-R combination adds computational complexity compared to simpler niche methods.
- Cross-sample integration assumes that niches are comparable across samples — a strong assumption that may not hold for highly variable tissues.

**Design Decision:** NicheCompass bets that intercellular communication is the most biologically meaningful niche feature. This is a defensible bet — signaling defines function, and function defines the niche. But it makes the method dependent on L-R databases, which are incomplete for many tissues and species.
