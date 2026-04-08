# Nicheformer

**Verdict:** The first foundation model designed specifically for spatial single-cell data. Proves that architecture (incorporating spatial context) matters more than scale (parameter count).

**Citation:** Schaar AC, et al. "Nicheformer: a foundation model for single-cell and spatial omics." *Nature Methods* 22:688-697, 2025. [DOI](https://doi.org/10.1038/s41592-025-02610-7)

## Problem Setup

Can a single foundation model learn useful cell representations from both dissociated and spatial single-cell data, and does spatial-aware pre-training improve downstream spatial tasks?

## Niche Definition

**Learned:** Niche = whatever the transformer learns from spatial neighborhood context during pre-training. The niche definition is implicit in the model's attention patterns and is not directly inspectable.

## Architecture

1. Transformer architecture pre-trained on both dissociated scRNA-seq and spatial transcriptomics data.
2. During pre-training on spatial data, the model receives spatial neighborhood context — tokens from neighboring cells are included in the attention window.
3. 49M parameters — deliberately smaller than models like TranscriptFormer (444M).
4. Produces cell embeddings that can be used for clustering, annotation, integration, and spatial tasks.

## Key Result

The 49M-parameter Nicheformer outperforms the 444M TranscriptFormer on spatial tasks (spatial domain detection, niche identification). This is the paper's most important finding: for spatial biology, incorporating the right data type during training matters more than scaling parameters.

## Evaluation

- Compared against TranscriptFormer, scGPT, and other foundation models on both dissociated and spatial tasks.
- Spatial tasks: domain detection, niche identification, spatial gene imputation.
- Non-spatial tasks: cell annotation, batch integration, cross-tissue transfer.

## Honest Assessment

**Strengths:**

- Proves the architecture-over-scale hypothesis for spatial biology. This is an important result for the field.
- Pre-trained on diverse tissues with spatial context, making it genuinely spatial-native rather than adapted from non-spatial models.
- From the Theis lab, well-integrated with the scverse ecosystem.
- Practical: 49M parameters means it can be fine-tuned on standard GPUs.

**Limitations:**

- The niche definition is opaque — you cannot inspect what the model has learned to call a "niche." The attention patterns provide some interpretability but are not a substitute for an explicit niche definition.
- Pre-training data composition determines what niches the model can recognize. Tissues or conditions not in the training set may produce poor embeddings.
- The comparison against TranscriptFormer, while favorable, compares different pre-training data compositions, not just architecture. The improvement may partly reflect data curation, not just spatial context.
- Does not address the functional niche question — like all current methods, it captures correlative structure.

**Design Decision:** Nicheformer's key bet is that spatial context during pre-training is worth more than more parameters. The 49M vs 444M comparison is striking, but the real lesson is that foundation models for spatial biology should be trained on spatial data — an obvious point that TranscriptFormer did not follow.
