# Domain Detection Methods

**These are not niche methods.** We include them because they are frequently confused with niche methods in the literature and in benchmarks.

See [Niche vs Domain](../concepts/niche-vs-domain.md) for a detailed explanation of why this distinction matters.

## The Key Difference

- **Domain:** A contiguous spatial region of similar expression. Cortical layer 3 is a domain.
- **Niche:** A recurring local microenvironment pattern. The perivascular niche is a niche — it appears around blood vessels throughout the brain.

Domain methods enforce or strongly favor spatial contiguity. Niche methods group similar local microenvironments regardless of where they appear in the tissue.

## Key Methods

### GraphST

- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-023-36796-3)
- **Code:** [github.com/JinmiaoChenLab/GraphST](https://github.com/JinmiaoChenLab/GraphST)
- Self-supervised contrastive learning on spatial graphs. Strong benchmark performance on DLPFC but finds contiguous regions, not distributed niches.

### STAGATE

- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-022-29439-6)
- **Code:** [github.com/zhanglabtools/STAGATE](https://github.com/zhanglabtools/STAGATE)
- Graph attention autoencoder with adaptive graph construction. Attention mechanism weights neighbors, producing smooth spatial embeddings that favor contiguous domains.

### BayesSpace

- **Paper:** [Nature Biotechnology, 2021](https://doi.org/10.1038/s41587-021-00935-2)
- **Code:** [github.com/edward130603/BayesSpace](https://github.com/edward130603/BayesSpace)
- Bayesian spatial clustering with neighbor-informed priors. Explicitly encodes spatial smoothness, which is the definition of domain detection.

### BASS

- **Paper:** [Nature Methods, 2022](https://doi.org/10.1038/s41592-022-01576-2)
- **Code:** [github.com/zhengli09/BASS](https://github.com/zhengli09/BASS)
- Multi-scale Bayesian framework for simultaneous cell-type and spatial domain identification. The domain component enforces spatial contiguity.

### SEDR

- **Paper:** [Bioinformatics, 2022](https://doi.org/10.1093/bioinformatics/btac634)
- **Code:** [github.com/JinmiaoChenLab/SEDR](https://github.com/JinmiaoChenLab/SEDR)
- Variational autoencoder with deep graph embedding. Produces spatially smooth embeddings suited for domain segmentation.

### SpaceFlow

- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-022-31878-0)
- **Code:** [github.com/hongleir/SpaceFlow](https://github.com/hongleir/SpaceFlow)
- Spatially-regularized deep graph network. Adds pseudo-spatiotemporal trajectory inference on top of domain segmentation.

### MENDER

- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02219-w)
- **Code:** [github.com/yuanzhiyuan/MENDER](https://github.com/yuanzhiyuan/MENDER)
- Multi-range cell context decoding. Works across different spatial resolutions but primarily designed for domain identification. The multi-range aspect could capture niche-like patterns at finer scales.

### STACI

- **Paper:** [Bioinformatics, 2023](https://doi.org/10.1093/bioinformatics/btad634)
- **Code:** [github.com/uhlerlab/STACI](https://github.com/uhlerlab/STACI)
- Statistical approach using spatial autocorrelation for domain identification.

## When Domain Methods Are Appropriate for Niche Questions

Domain methods can serve as niche methods when:

1. **Niches are spatially concentrated** — e.g., a tumor core niche that occupies a contiguous region.
2. **You want to segment tissue into broad regions** before applying niche methods within each region.
3. **The DLPFC-style question is your actual question** — identifying cortical layers is a valid task, just not a niche task.

But if your biological question involves distributed microenvironments, domain methods will either miss them or fragment them into separate clusters.
