# Niche vs Domain

## The Distinction

**Domain** and **niche** are frequently conflated in the spatial omics literature, but they are fundamentally different concepts:

| Property | Domain | Niche |
|----------|--------|-------|
| **Definition** | A contiguous spatial region of similar expression or cell composition | A recurring local microenvironment pattern |
| **Spatial constraint** | Must be spatially contiguous | Can appear at many disconnected locations |
| **Analogy** | A neighborhood on a city map | A type of neighborhood (e.g., "downtown district") that exists in every city |
| **Example** | Cortical layer 3 in the DLPFC | The perivascular niche found around blood vessels throughout the brain |
| **Output** | Each cell gets one domain label; the labels form a spatial partition | Each cell gets a niche label; identical labels can appear far apart |

## Why This Matters

The most commonly used benchmark dataset in spatial transcriptomics — the human dorsolateral prefrontal cortex (DLPFC) from Maynard et al. (2021) — has manual layer annotations that define **domains** (cortical layers 1-6 plus white matter). When a method is evaluated on DLPFC, it is being tested on domain detection, not niche identification.

This creates a systematic bias: methods optimized for DLPFC may excel at finding contiguous regions but fail to identify recurring niches that are distributed throughout the tissue. A perivascular niche, an immune-tumor interface, or a hypoxic pocket would each appear at multiple disconnected locations — exactly the pattern that domain-centric evaluation misses.

## When Domain Methods Find Niches (and When They Don't)

Domain methods can identify niches when niches happen to be spatially concentrated. In the DLPFC, each cortical layer is both a domain and arguably a niche. In a tumor, the invasive margin is roughly contiguous and detectable as a domain.

But many biologically important niches are distributed:

- **Perivascular niches** surround blood vessels throughout tissue.
- **Immune niches** cluster around antigen-presenting cells at many locations.
- **Stem cell niches** are scattered through tissue at specific anatomical positions.
- **Hypoxic niches** form wherever oxygen gradients create pockets.

A domain method will fragment these into separate domains or merge them with surrounding tissue. A niche method will group them together as instances of the same microenvironment type.

## The DLPFC Problem

The DLPFC dataset dominates benchmarking because it has high-quality manual annotations and is publicly available. But optimizing for DLPFC means optimizing for domain detection. The field needs dedicated niche benchmarks with:

1. Ground-truth niche annotations at multiple disconnected locations.
2. Evaluation metrics that reward recognizing the same niche type across spatial gaps.
3. Datasets where biologically important structure is distributed, not contiguous.

The CRC CODEX dataset from Schurch et al. (2020) is closer to a true niche benchmark — its 9 cellular neighborhoods were defined by recurring cell-type compositions regardless of spatial location.

## Methods in the Grey Zone

Some methods blur the line:

- **BANKSY** can find both domains and niches depending on parameterization — its neighborhood aggregation can emphasize local context (niche-like) or regional consistency (domain-like).
- **MENDER** explicitly models multiple spatial scales, potentially capturing both domain and niche structure.
- **SpiceMix** uses spatial priors but does not enforce contiguity, making it more niche-like than domain-like.

Understanding whether a method enforces spatial contiguity is key to knowing whether it finds domains or niches.
