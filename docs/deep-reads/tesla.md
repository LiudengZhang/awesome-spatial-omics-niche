# TESLA

**Verdict:** Practical and impactful — H&E-guided super-resolution for spatial transcriptomics addresses a real limitation of spot-level technologies. From Linghua Wang's group, directly relevant to the morphology-niche connection.

**Citation:** Hu J, et al. "TESLA uncovers spatial gene expression patterns at super-resolution in tissue by integrating histological images." *Cell Systems* 14(1):20-32.e6, 2023. [DOI](https://doi.org/10.1016/j.cels.2022.12.006)

## Problem Setup

Visium spatial transcriptomics has ~55 um spot resolution, containing multiple cells per spot. Can we use the paired H&E image — which has single-cell resolution — to annotate tissue at finer granularity than the transcriptomics data?

## Niche Definition

**Morphology-based:** Niche = the joint histological and transcriptomic context. H&E morphology provides the spatial resolution; gene expression provides the molecular characterization. The niche is defined at a resolution finer than either modality alone.

## Architecture

1. Extract image features from H&E at sub-spot resolution using a convolutional neural network.
2. Build a spatial graph connecting spots to their sub-spot image regions.
3. Learn a mapping from image features to gene expression patterns.
4. Use the learned mapping to annotate tissue at image resolution, subdividing each Visium spot into finer regions.
5. Identify tumor regions, cell types, and spatial niches at enhanced resolution.

## Key Applications

- Super-resolution annotation of tumor margins.
- Identification of tumor-immune interfaces at finer resolution than Visium spots.
- Detection of spatial expression patterns within individual spots.

## Honest Assessment

**Strengths:**

- Addresses a genuine limitation — Visium's spot resolution is too coarse for niche analysis. TESLA enhances this using freely available H&E data.
- Clinically relevant: H&E is the standard clinical stain, available for virtually all tissue samples.
- Practical tool with demonstrated utility for tumor microenvironment characterization.
- From Linghua Wang's group at MD Anderson, with deep domain expertise in cancer spatial biology.

**Limitations:**

- The super-resolution is learned from training data — the model predicts expression at sub-spot resolution, but these predictions are estimates, not measurements.
- Accuracy depends on the informativeness of H&E morphology for the genes of interest. Some genes have no morphological correlate.
- Validated primarily on cancer tissues; generalization to other tissue types is less established.
- Requires paired H&E + spatial transcriptomics for training, though inference only needs H&E.

**Design Decision:** TESLA bets that histology encodes enough information to subdivide spatial transcriptomics spots. This is true for tissue architecture features (tumor vs stroma, epithelial vs mesenchymal) but less true for molecular states that do not manifest morphologically (metabolic programs, signaling states). The method is most powerful for cancer pathology where morphological correlates of molecular state are strongest.
