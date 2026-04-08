# The Functional Niche

## Beyond Correlation

Every computational niche method described on this site is fundamentally correlative: it identifies microenvironments based on what is observed — which cell types are nearby, what genes are expressed, what ligand-receptor pairs co-occur. But none of them answer the causal question: **does this niche actually influence cell behavior?**

The functional niche is a microenvironment defined not by what is present, but by what it does — its causal effect on the cells within it.

## The Gap

Consider an immune niche identified by the co-localization of CD8 T cells, macrophages, and dendritic cells around a tumor. A correlative method can detect this pattern. But:

- Are the T cells there because the niche recruited them, or did they arrive independently?
- Is the macrophage polarization state caused by the niche context, or would it be the same elsewhere?
- Does removing the dendritic cells change the T cell behavior?

These are causal questions that observational spatial data alone cannot answer.

## Approaches Toward Functional Niches

### Perturbation + Spatial Readout

The most direct approach: perturb a component of the niche and measure the spatial response. Technologies like Perturb-FISH (spatial transcriptomics after CRISPR perturbation) and spatial CRISPR screens are beginning to enable this, but data is extremely scarce.

### Niche-DE as a Proxy

[Niche-DE](https://github.com/ccb-hms/NicheDE) tests whether gene expression in a cell depends on its niche context. While still correlative (it measures association, not causation), it is a step toward functional characterization because it identifies genes whose expression is niche-dependent rather than cell-intrinsic.

### Agent-Based Modeling

Tools like [PhysiCell](https://github.com/MathCancer/PhysiCell) simulate cells with explicit rules for cell-cell interactions, allowing in silico experiments where niche components can be removed or modified. The challenge is specifying accurate interaction rules.

### Communication-Based Methods as Approximation

Communication-based niche methods (NicheCompass, COMMOT, CellChat) approximate functional niches by focusing on signaling rather than co-localization. Active ligand-receptor pairs are closer to causal mechanisms than cell-type proportions. But co-expression of a ligand and receptor does not guarantee active signaling.

## What Would It Take?

A true functional niche definition would require:

1. **Causal inference frameworks** adapted for spatial data — methods that can distinguish niche effects from confounders like shared developmental origin or spatial gradients of nutrients/oxygen.
2. **Spatial perturbation data** at scale — datasets where niche components have been experimentally manipulated with spatial readouts.
3. **Temporal resolution** — observing how niches form and dissolve over time to distinguish cause from consequence.

This is the frontier. Current methods define niches by what they look like. The field needs methods that define niches by what they do.
