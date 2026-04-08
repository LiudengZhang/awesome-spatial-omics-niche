# Niche-DE

**Verdict:** Answers a question most niche methods ignore: once you define a niche, which genes are actually affected by it? Separates niche-dependent from cell-intrinsic expression.

**Citation:** Jiang R, et al. "Niche-DE: niche-differential gene expression analysis in spatial transcriptomics data identifies context-dependent cell-cell interactions." *Genome Biology* 25:14, 2024. [DOI](https://doi.org/10.1186/s13059-023-03154-x)

## Problem Setup

Standard differential expression compares cell types or conditions. But a cell's expression may depend on its spatial context — the same cell type may express different genes depending on which other cells are nearby. Can we detect genes whose expression depends on the cellular niche?

## Niche Definition

**Meta-definition:** Niche-DE does not define niches itself. It takes any niche definition (from any other method) as input and tests whether gene expression within that niche differs from what would be expected based on cell type alone.

Specifically: niche = the cell-type composition of a cell's spatial neighborhood (composition-based, matching Schurch et al.).

## Architecture

1. For each cell, compute the proportion of each cell type in its spatial neighborhood.
2. For a target cell type, model gene expression as a function of neighborhood composition.
3. Test whether neighbor cell-type proportions significantly predict expression, controlling for cell-intrinsic variation.
4. Genes with significant niche association are "niche-differentially expressed" (niche-DE genes).

Two modes:

- **Niche-DE:** Which genes in cell type A are affected by the presence of cell type B nearby?
- **Niche-LR (Ligand-Receptor):** Extension linking niche-DE genes to specific L-R pairs to identify the communication mechanism.

## Evaluation

- Validated on MERFISH, Slide-seq, and CODEX datasets.
- Demonstrated detection of known niche-dependent genes (e.g., genes induced in macrophages near tumor cells).
- Compared against non-spatial DE methods and CCC tools.

## Honest Assessment

**Strengths:**

- Asks a fundamentally important question that most niche methods skip: given a niche, what does it actually do to gene expression?
- Clean statistical framework with proper null hypothesis testing.
- Niche-LR extension links spatial effects to molecular mechanisms.
- Works downstream of any niche definition method — composable with the entire niche analysis ecosystem.

**Limitations:**

- Still correlative — niche association does not prove causation. A gene may be associated with a niche because of shared developmental origin, not because the niche causes the expression change.
- Composition-based niche definition is simple; could miss niche effects driven by expression states or signaling rather than cell-type proportions.
- Statistical power depends on having enough cells of each type in each niche context.

**Design Decision:** Niche-DE fills a critical gap. The field has many methods to identify niches but few to test their functional consequences. By separating niche-dependent from cell-intrinsic expression, it provides the closest thing to a functional readout without perturbation data. The Niche-LR extension is particularly valuable because it connects spatial patterns to molecular mechanisms.
