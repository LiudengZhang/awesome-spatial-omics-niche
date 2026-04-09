# Composition-Based Methods

**Niche definition: cell-type proportions in a spatial neighborhood.**

These methods answer the question: what cell types co-localize in this tissue? They require pre-annotated cell types and spatial coordinates, then cluster cells based on the composition of their local neighborhoods.

## Key Methods

### CellCharter

- **Paper:** [Nature Genetics, 2024](https://doi.org/10.1038/s41588-023-01588-4)
- **Code:** [github.com/CSOgroup/cellcharter](https://github.com/CSOgroup/cellcharter)
- **Niche definition:** GNN-learned embedding of cell-type composition at multiple spatial resolutions.
- **Key innovation:** Handles multiple spatial scales simultaneously and works across both imaging-based and sequencing-based spatial platforms.
- **Strengths:** Multi-resolution analysis, cross-platform compatibility, principled clustering via Gaussian mixture models.
- **Limitations:** Requires cell-type annotations as input — niche quality is bounded by annotation quality.

### UTAG

- **Paper:** [Nature Methods, 2022](https://doi.org/10.1038/s41592-022-01657-2)
- **Code:** [github.com/ElementoLab/utag](https://github.com/ElementoLab/utag)
- **Niche definition:** Message-passing on cell graphs combines each cell's expression with its spatial neighbors' expression.
- **Key innovation:** Unsupervised — does not require cell-type annotations, though it uses expression rather than pure composition.
- **Note:** Sits between composition-based and expression-based; listed here because it is commonly used for neighborhood detection.

### CytoCommunity

- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-023-02124-2)
- **Code:** [github.com/huBioinfo/CytoCommunity](https://github.com/huBioinfo/CytoCommunity)
- **Niche definition:** Supervised graph partitioning that uses condition labels (e.g., responder vs non-responder) to find disease-relevant cellular neighborhoods.
- **Key innovation:** The only niche method that directly incorporates clinical outcome labels into niche discovery.
- **Strengths:** Finds niches that differ between conditions, not just niches that exist.
- **Limitations:** Requires condition labels — cannot discover niches in an unsupervised setting.

### SpatialLDA

- **Paper:** [Genome Biology, 2022](https://doi.org/10.1186/s13059-022-02721-y)
- **Code:** [github.com/calico/spatial_lda](https://github.com/calico/spatial_lda)
- **Niche definition:** Topic modeling treats spatial regions as documents and cell types as words; niches are latent topics.
- **Key innovation:** Probabilistic framework that allows cells to belong to multiple niche topics with different weights.
- **Strengths:** Interpretable topic structure, handles mixed niches naturally.
- **Limitations:** Sensitive to hyperparameter choices (number of topics, neighborhood size).

### SOTIP

- **Paper:** [Nature Communications, 2023](https://doi.org/10.1038/s41467-023-39608-w)
- **Code:** [github.com/TencentAILabHealthcare/SOTIP](https://github.com/TencentAILabHealthcare/SOTIP)
- **Niche definition:** Optimal transport distances between spatial neighborhoods enable cross-sample niche comparison.
- **Key innovation:** Designed for multi-sample, multi-condition analysis — compares niches across patients.
- **Strengths:** Principled statistical framework for inter-sample niche comparison.

### CNTools

- **Paper:** [Bioinformatics, 2024](https://doi.org/10.1093/bioinformatics/btae095)
- **Code:** [github.com/liu-bioinfo-lab/CNTools](https://github.com/liu-bioinfo-lab/CNTools)
- **Niche definition:** Kernel density estimation of cell-type distributions in spatial neighborhoods.
- **Strengths:** Lightweight, interpretable, does not require graph construction.

## When to Use Composition-Based Methods

**Best for:**

- Tissues with well-characterized cell types and reliable annotations.
- Questions about which cell types co-localize and how those patterns change across conditions.
- Studies where the biological question is about cellular communities (e.g., tertiary lymphoid structures, tumor-immune interfaces).

**Not ideal for:**

- Datasets without cell-type annotations (use expression-based methods instead).
- Questions about what cells are doing rather than who is nearby (use communication-based methods).
- Detecting sub-states within a cell type that depend on spatial context (use covariance or expression methods).
