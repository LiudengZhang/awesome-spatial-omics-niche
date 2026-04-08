# Datasets

Datasets organized by what niche definitions they support.

## The Standard Benchmark

### Human DLPFC

- **Citation:** Maynard KR, et al. *Nature Neuroscience* 24:425-436, 2021. [DOI](https://doi.org/10.1038/s41593-020-00787-0)
- **Platform:** 10x Visium
- **Scale:** 12 tissue sections, ~3,500 spots each
- **Annotations:** Manual cortical layer annotations (layers 1-6 + white matter)
- **Niche support:** Domains, not niches. Tests spatial domain detection. Not suitable for evaluating methods that find distributed cellular microenvironments.
- **Why it dominates:** High-quality manual annotations, publicly available, well-characterized tissue.
- **Caveat:** Optimizing for DLPFC means optimizing for domain detection. See [State of Benchmarking](../benchmarks/state-of-benchmarking.md).

## True Niche Datasets

### CRC CODEX (Schurch et al.)

- **Citation:** Schurch CM, et al. *Cell* 182(5):1341-1359, 2020. [DOI](https://doi.org/10.1016/j.cell.2020.07.005)
- **Platform:** CODEX multiplexed protein imaging (56 markers)
- **Scale:** 140 tissue regions from 35 CRC patients
- **Annotations:** 9 cellular neighborhoods defined by composition-based clustering
- **Niche support:** The closest thing to a true niche benchmark. Neighborhoods are distributed, recurring, and clinically validated.
- **Best for:** Composition-based niche methods, clinical niche association studies.

### Hepatic Niches (Guilliams et al.)

- **Citation:** Guilliams M, et al. *Cell* 185(2):379-397, 2022. [DOI](https://doi.org/10.1016/j.cell.2021.12.018)
- **Platform:** Spatial proteomics + transcriptomics
- **Scale:** Multi-donor human liver atlas
- **Annotations:** Zonated hepatic niches with known spatial organization (periportal to pericentral gradient)
- **Niche support:** Expression gradients, communication niches. The liver's known zonation provides biological ground truth.
- **Best for:** Expression-based, covariance-based, and communication-based methods.

### Cardiac Niches (Kanemaru et al.)

- **Citation:** Kanemaru K, et al. *Nature* 619:801-810, 2023. [DOI](https://doi.org/10.1038/s41586-023-06311-1)
- **Platform:** Spatial multi-omic (Visium + CITE-seq)
- **Scale:** Human heart atlas across developmental stages and disease
- **Annotations:** Cardiac cellular neighborhoods and disease-associated niche changes
- **Niche support:** Multi-omic niche definition with temporal progression.

## Brain Datasets

### Mouse Brain MERFISH (Moffitt et al.)

- **Citation:** Moffitt JR, et al. *Science* 362(6416):eaau5324, 2018. [DOI](https://doi.org/10.1126/science.aau5324)
- **Platform:** MERFISH
- **Scale:** Mouse hypothalamus, ~1 million molecules, ~70 genes
- **Annotations:** Cell-type annotations from clustering
- **Niche support:** High spatial resolution, suitable for expression-based and composition-based niche analysis.

### Mouse Embryo seqFISH (Lohoff et al.)

- **Citation:** Lohoff T, et al. *Nature Biotechnology* 40:74-85, 2022. [DOI](https://doi.org/10.1038/s41587-021-01006-2)
- **Platform:** seqFISH
- **Scale:** Mouse embryo with subcellular resolution
- **Annotations:** Cell-type and tissue region annotations
- **Niche support:** Rich developmental niche structure with known tissue organization.
- **Best for:** SpiceMix, expression-based methods that handle subcellular resolution.

## Choosing a Dataset for Your Evaluation

| Your Niche Definition | Best Datasets |
|----------------------|---------------|
| Composition-based | CRC CODEX, any dataset with reliable cell-type annotations |
| Expression-based | MERFISH mouse brain, seqFISH mouse embryo, DLPFC (but tests domains) |
| Covariance-based | Hepatic niches (known zonation provides ground truth for regulatory gradients) |
| Communication-based | Hepatic niches, cardiac niches (known signaling gradients) |
| Morphology-based | Any Visium dataset with paired H&E |
| Foundation model | CRC CODEX + MERFISH + Visium (cross-platform evaluation) |
