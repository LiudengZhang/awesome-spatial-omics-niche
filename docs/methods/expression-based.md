# Expression-Based Methods

**Niche definition: gene expression patterns aggregated over spatial neighbors.**

These methods answer the question: how does the transcriptomic landscape change across tissue? They work directly with expression data and spatial coordinates without requiring cell-type annotations.

## Key Methods

### BANKSY

- **Paper:** [Nature Genetics, 2024](https://doi.org/10.1038/s41588-024-01664-3)
- **Code:** [github.com/prabhakarlab/Banksy_py](https://github.com/prabhakarlab/Banksy_py)
- **Niche definition:** Each cell's feature vector is augmented with the mean and azimuthal Gabor gradient of its spatial neighborhood's expression.
- **Key innovation:** The gradient feature captures expression boundaries and tissue interfaces — regions where expression changes rapidly — which pure averaging methods miss entirely.
- **Strengths:** Principled mathematical framework, captures both bulk neighborhood effects and boundary effects, works at multiple scales.
- **Limitations:** The `lambda` parameter controls the weight of spatial versus expression features; choosing it requires domain knowledge or systematic tuning.
- **Verdict:** The strongest expression-based niche method. The gradient feature is a genuine conceptual advance.

### SpaGCN

- **Paper:** [Nature Methods, 2021](https://doi.org/10.1038/s41592-021-01255-8)
- **Code:** [github.com/jianhuupenn/SpaGCN](https://github.com/jianhuupenn/SpaGCN)
- **Niche definition:** Graph convolutional network propagates expression through spatial graphs, optionally incorporating H&E histology features.
- **Key innovation:** Early method to integrate histological image features with spatial transcriptomics via graph convolutions.
- **Strengths:** Simple, effective for Visium-scale data, incorporates histology.
- **Limitations:** Finds domains more than niches — the GCN tends to produce spatially contiguous clusters.

### SpiceMix

- **Paper:** [Nature Genetics, 2023](https://doi.org/10.1038/s41588-022-01256-z)
- **Code:** [github.com/ma-compbio/SpiceMix](https://github.com/ma-compbio/SpiceMix)
- **Niche definition:** Non-negative matrix factorization with spatial priors decomposes expression into metagenes with spatially varying loadings.
- **Key innovation:** Handles subcellular-resolution data (seqFISH, MERFISH) and produces interpretable metagene programs.
- **Strengths:** Interpretable decomposition, spatial priors without enforcing contiguity, handles multiple spatial scales.
- **Limitations:** Computationally intensive for large datasets.

### SpatialPCA

- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-022-34879-1)
- **Code:** [github.com/shangll123/SpatialPCA](https://github.com/shangll123/SpatialPCA)
- **Niche definition:** PCA with a spatial kernel that models expression correlations as a function of physical distance.
- **Key innovation:** Extends PCA to account for spatial autocorrelation, producing spatially smooth low-dimensional embeddings.
- **Strengths:** Principled statistical framework, interpretable principal components.
- **Limitations:** Gaussian process kernel can be computationally expensive; assumes smooth spatial variation.

### NichePCA

- **Paper:** [Bioinformatics, 2025](https://doi.org/10.1093/bioinformatics/btae708)
- **Code:** [github.com/imsb-uke/nichepca](https://github.com/imsb-uke/nichepca)
- **Niche definition:** PCA applied to spatial neighborhood expression profiles, producing interpretable niche axes.
- **Key innovation:** Explicitly designed for niche analysis rather than adapted from domain detection; niche axes correspond to biological gradients.
- **Strengths:** Simple, fast, interpretable axes. Newest method in this category.

## When to Use Expression-Based Methods

**Best for:**

- Datasets without reliable cell-type annotations.
- Discovering expression gradients and tissue boundaries.
- Exploratory analysis where you want the data to define niche structure without imposing cell-type categories.

**Not ideal for:**

- Questions specifically about cell-type co-localization (use composition-based methods).
- Understanding intercellular signaling (use communication-based methods).
- When you need the niche definition to be biologically interpretable at the gene level (averaged expression across diverse cells is hard to interpret).
