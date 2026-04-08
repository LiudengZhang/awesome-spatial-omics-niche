# From Correlation to Causation

## The Gap

Every computational niche method on this site is correlative. They identify spatial patterns — which cells are nearby, what is expressed, what co-varies, what signaling is active. None of them prove that the niche causes any of the observed biology.

This matters because the implicit promise of niche analysis is causal: understanding the niche should tell us how to change cell behavior by changing the microenvironment. Drug development, immunotherapy optimization, and regenerative medicine all require causal understanding.

## What "Correlative" Means in Practice

A composition-based method finds that CD8 T cells near dendritic cells express higher levels of activation markers. This could mean:

1. **The niche causes activation:** Dendritic cells present antigen to T cells, activating them. The niche is functionally relevant.
2. **Activation causes the niche:** Activated T cells migrate toward dendritic cells. The niche is a consequence, not a cause.
3. **Confounding:** Both activated T cells and dendritic cells are recruited by the same chemokine gradient. Neither causes the other.
4. **Developmental origin:** Both cell types differentiate from a common precursor in the same location. The co-localization reflects shared origin, not interaction.

Current niche methods cannot distinguish between these scenarios from observational spatial data alone.

## Bridging Strategies

### Perturbation + Spatial Readout

The gold standard: experimentally remove or modify a niche component and measure the spatial response.

- **Perturb-FISH / spatial CRISPR screens:** Combine genetic perturbation with spatial transcriptomics readout.
- **Drug perturbation + spatial readout:** Treat tissue with targeted therapies and observe niche remodeling.
- **Status:** Data is extremely scarce. Most perturbation studies use dissociated readouts, losing spatial context.

### Temporal Spatial Data

If we observe niche formation over time, we can establish temporal precedence (necessary but not sufficient for causation).

- **Live imaging + spatial transcriptomics:** Observe niche assembly in real-time.
- **Developmental time series:** Multiple time points showing niche formation.
- **Status:** Technically challenging. Live imaging has limited molecular readout; spatial transcriptomics is destructive.

### Computational Causal Inference

Apply causal inference frameworks to spatial data:

- **Instrumental variables:** Use natural spatial variation (distance from vasculature, tissue boundaries) as instruments.
- **Mendelian randomization:** Use genetic variation to infer causal relationships between niche composition and cell state.
- **Granger causality on spatial pseudotime:** If spatial gradients can be interpreted as pseudotime, test for temporal (spatial) precedence.
- **Status:** Early and largely unvalidated for spatial omics.

### Agent-Based Modeling

Simulate tissues with explicit interaction rules and test whether removing niche components changes cell behavior.

- **PhysiCell, CompuCell3D:** Mechanistic simulation frameworks.
- **Advantage:** Can test counterfactuals ("what if we removed macrophages from this niche?").
- **Limitation:** Requires specifying interaction rules, which are often unknown.

## The Niche-DE Contribution

[Niche-DE](../deep-reads/niche-de.md) does not solve the causation problem, but it takes a meaningful step: by testing whether gene expression statistically depends on niche context (controlling for cell-intrinsic variation), it identifies genes that are at least niche-associated. This is stronger than simple differential expression but weaker than causal proof.

## What the Field Needs

1. **Spatial perturbation datasets at scale.** The equivalent of Perturb-seq but with spatial readouts.
2. **Causal inference methods adapted for spatial data.** Existing causal frameworks assume independence; spatial data has inherent autocorrelation.
3. **Temporal spatial atlases.** Time-resolved spatial data showing niche formation, maintenance, and dissolution.
4. **Integration frameworks.** Methods that combine observational spatial data with small-scale perturbation data to extend causal inference across tissues.

The transition from "these cells are co-localized" to "this microenvironment causes this cell behavior" is the single most important open problem in spatial niche analysis.
