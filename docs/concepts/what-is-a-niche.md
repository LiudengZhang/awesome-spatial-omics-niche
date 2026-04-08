# What Is a Niche?

## The Short Answer

A niche is the local microenvironment around a cell — who is nearby, what they express, what signals they exchange, and how that context shapes cell behavior.

## The Longer Answer

The term "niche" originates from stem cell biology, where it refers to the specific anatomical location and signaling environment that maintains stem cell identity (Schofield, 1978). In spatial omics, the concept has been generalized to mean any recurring local cellular microenvironment — but there is no consensus on how to define it computationally.

This matters because **every computational method implicitly defines what a niche is**, and that definition determines what it can discover.

## The Definition Tree

We organize niche definitions into six families based on what biological feature they use to define the microenvironment:

### 1. Composition-Based

**Niche = cell-type proportions in a spatial neighborhood.**

Methods: [CellCharter](https://github.com/CSOgroup/cellcharter), [UTAG](https://github.com/ElementoLab/utag), [CytoCommunity](https://github.com/huBioinfo/CytoCommunity), SpatialLDA, SOTIP.

- **Requires:** Pre-annotated cell types + spatial coordinates.
- **Discovers:** Recurring cellular communities (e.g., "tumor-immune interface" = high proportion of tumor cells + CD8 T cells + macrophages).
- **Misses:** Two neighborhoods with the same cell-type proportions but different gene expression or signaling states are treated as identical.

### 2. Expression-Based

**Niche = aggregated gene expression over spatial neighbors.**

Methods: [BANKSY](https://github.com/prabhakarlab/Banksy_py), SpaGCN, SpiceMix, SpatialPCA, NichePCA.

- **Requires:** Gene expression + spatial coordinates (no cell-type annotation needed).
- **Discovers:** Expression microenvironments, including gradients and boundaries that composition methods miss.
- **Misses:** Cannot distinguish whether a neighbor's expression influences a cell or merely co-occurs with it. The biological meaning of averaged expression across diverse cell types is ambiguous.

### 3. Covariance-Based

**Niche = gene-gene covariance structure across spatial neighbors.**

Methods: [ENVI/COVET](https://github.com/dpeerlab/ENVI), SIMVI, SPACE.

- **Requires:** Gene expression + spatial coordinates.
- **Discovers:** How gene co-expression patterns change with spatial context — captures niche effects beyond mean expression.
- **Misses:** Computationally expensive; the covariance tensor grows quadratically with genes. Harder to interpret biologically than proportions or means.

### 4. Communication-Based

**Niche = active ligand-receptor signaling in a spatial neighborhood.**

Methods: [NicheCompass](https://github.com/Lotfollahi-lab/nichecompass), [COMMOT](https://github.com/zcang/COMMOT), CellChat, SpaTalk, SpatialDM, NicheNet.

- **Requires:** Gene expression + spatial coordinates + ligand-receptor database.
- **Discovers:** Functionally defined niches based on what cells are actually communicating, not just who is nearby.
- **Misses:** Depends entirely on the completeness and accuracy of L-R databases. "Active" signaling is inferred from co-expression, not measured directly. Novel or poorly annotated pathways are invisible.

### 5. Morphology-Based

**Niche = joint histological and molecular features.**

Methods: [TESLA](https://github.com/jianhuupenn/TESLA), [METI](https://github.com/jianhuupenn/METI), GigaTIME.

- **Requires:** H&E images + gene expression (or protein markers).
- **Discovers:** Morphologically defined microenvironments that integrate tissue architecture visible in histology with molecular profiles.
- **Misses:** H&E features are correlative with molecular states; the mapping is learned from paired data and may not generalize across tissues or conditions.

### 6. Learned (Foundation Models)

**Niche = whatever the model learns from data.**

Methods: [Nicheformer](https://github.com/theislab/nicheformer), scNiche, ONTraC.

- **Requires:** Large-scale pre-training data + spatial coordinates.
- **Discovers:** Potentially any niche structure that is statistically present in training data.
- **Misses:** The niche definition is implicit and opaque — you cannot easily inspect what the model has learned to define as "niche." Interpretability is limited.

## Same Data, Different Niches

Consider a spatial transcriptomics dataset of colorectal cancer tissue. Applied to the same data:

- A **composition method** might find a "tumor-immune border" niche defined by co-localization of tumor cells, CD8 T cells, and M1 macrophages.
- An **expression method** might split that same border into sub-niches based on whether the tumor cells are in a proliferative or hypoxic expression state.
- A **communication method** might reveal that only a subset of those border cells have active PD-L1/PD-1 signaling, defining a functionally distinct "immune checkpoint niche."
- A **covariance method** might detect that interferon-response genes are co-regulated differently at the invasive margin versus the tumor core.

None of these are wrong. They are answering different questions about the same tissue.

## Which Definition Should You Use?

| Your Question | Best Niche Definition |
|--------------|----------------------|
| What cell types co-localize in this tissue? | Composition-based |
| How does local expression change across tissue? | Expression-based |
| How do gene regulatory patterns vary spatially? | Covariance-based |
| What cells are actively signaling to each other? | Communication-based |
| How does tissue architecture relate to molecular state? | Morphology-based |
| I want the model to figure it out | Foundation model |
| Are cells in a specific niche differentially expressed? | Use any niche definition + Niche-DE |

## The Functional Niche Frontier

All current computational definitions are correlative: they describe what is observed in a neighborhood. The next frontier is the **functional niche** — a microenvironment defined by its causal effect on cell behavior. See [Functional Niche](functional-niche.md) for more on this open problem.
