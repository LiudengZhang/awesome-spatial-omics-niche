# Decision Guide: Choosing Your Niche Tools

Spatial niche analysis tools fall into two stages: **recognizing niches** (what are the spatial neighborhoods?) and **characterizing their function** (what biology drives each niche?). This guide helps you pick the right tool for each stage.

!!! tip "Core insight"
    The niche definition you choose determines what you can find. A composition-based tool finds cell-type mixtures; a communication-based tool finds signaling hubs. Neither is wrong — they answer different questions.

---

## Table 1: Niche Recognition

**Goal:** Identify and label spatial neighborhoods in your tissue.

| Tool | Approach | How It Defines a Niche | Platforms Tested | Key Strength | Key Limitation | Reference |
|------|----------|----------------------|-----------------|-------------|----------------|-----------|
| **[CellCharter](../deep-reads/cellcharter.md)** | Composition | Cell-type proportions in spatial neighborhoods, learned via GNN at multiple resolutions | CODEX, CosMx, MERSCOPE, Visium, IMC | Works on both RNA and protein platforms; multi-scale | Requires cell-type annotations as input | Varrone et al., *Nature Genetics* 2024 |
| **[CytoCommunity](../deep-reads/cytocommunity.md)** | Composition | Supervised graph partitioning using condition labels to find disease-relevant neighborhoods | CODEX, IMC | Finds condition-specific niches (e.g., responder vs. non-responder) | Requires condition labels; supervised | Hu et al., *Nature Methods* 2024 |
| **SpatialLDA** | Composition | Topic model: spatial regions are "documents," cell types are "words," niches are latent topics | CODEX, MERFISH | Probabilistic; soft niche assignments | Sensitive to hyperparameters (number of topics) | Chen et al., *Genome Biology* 2022 |
| **[BANKSY](../deep-reads/banksy.md)** | Expression | Gene expression augmented with mean and azimuthal gradient of neighbor expression | Visium, MERFISH, STARmap, Slide-seq | Fast; captures expression boundaries; top DLPFC performer | Defines domains, not niches in most configurations | Singhal et al., *Nature Genetics* 2024 |
| **SpiceMix** | Expression | NMF with spatial priors: metagenes with spatially varying loadings | seqFISH, MERFISH, STARmap | Learns spatial gene programs directly | Computationally intensive; limited platform testing | Chidester et al., *Nature Genetics* 2023 |
| **[ENVI/COVET](../deep-reads/envi-covet.md)** | Covariance | Gene-gene covariance tensor across spatial neighbors; ENVI imputes via conditional VAE | Slide-seq, MERFISH, seqFISH | Captures niche effects beyond cell-type composition; imputes whole transcriptome | Conceptually complex; requires matched scRNA-seq for ENVI | Haviv et al., *Nature Biotechnology* 2024 |
| **[NicheCompass](../deep-reads/nichecompass.md)** | Communication | GNN with ligand-receptor prior on spatial graph; niches defined by signaling patterns | Xenium, CosMx | Niches are mechanistically interpretable (L-R programs); cross-sample atlas | RNA-only (no protein); small panels lose L-R programs | Birk et al., *Nature Genetics* 2025 |
| **[Nicheformer](../deep-reads/nicheformer.md)** | Foundation model | 49M-param transformer pre-trained on dissociated + spatial data | Visium, MERFISH, Slide-seq | Transfer learning; flexible downstream tasks | Black box; needs fine-tuning per tissue | Fischer et al., *Nature Methods* 2025 |
| **Novae** | Foundation model | Graph-based foundation model with zero-shot niche inference across technologies | Visium, MERFISH, Xenium, Slide-seq | Zero-shot transfer; no retraining needed | Very new (2025); limited independent validation | Music et al., *Nature Methods* 2025 |

!!! warning "Domain detection ≠ Niche identification"
    The tools below find **spatially contiguous regions** (domains), not **distributed cellular neighborhoods** (niches). They are frequently confused with niche tools but answer a different question. See [Niche vs Domain](niche-vs-domain.md).

| Tool | What It Finds | Why It's Not a Niche Tool | When to Use It Instead | Reference |
|------|-------------|-------------------------|----------------------|-----------|
| **GraphST** | Contiguous spatial domains via self-supervised contrastive learning | Enforces spatial smoothness — scattered niches are merged into surrounding tissue | Tissue region segmentation (e.g., cortical layers, tumor vs. stroma) | Long et al., *Nature Communications* 2023 |
| **STAGATE** | Spatial domains via graph attention autoencoder | Adaptive graph construction biases toward contiguous regions | When spatial domains align with anatomical structures | Dong & Zhang, *Nature Communications* 2022 |
| **BayesSpace** | Spatial clusters via Bayesian model with neighbor priors | Explicit spatial smoothness prior penalizes non-contiguous clusters | Low-resolution data (Visium) where domains are large | Zhao et al., *Nature Biotechnology* 2021 |

---

## Table 2: Functional Characterization

**Goal:** Understand the biology driving each niche — signaling, gene programs, morphology.

### Cell-Cell Communication

| Tool | Approach | Spatial-Aware? | Platforms Tested | Key Strength | Key Limitation | Reference |
|------|----------|---------------|-----------------|-------------|----------------|-----------|
| **[NicheCompass](../deep-reads/nichecompass.md)** | GNN with L-R prior on spatial graph | Yes — interactions constrained to spatial neighbors | Xenium, CosMx | Joint niche + communication model; mechanistically interpretable | RNA-only; small panels lose L-R programs | Birk et al., *Nature Genetics* 2025 |
| **[COMMOT](../deep-reads/commot.md)** | Optimal transport with spatial distance constraint | Yes — rejects implausible long-range signals | Visium, MERFISH, seqFISH, STARmap, Slide-seq | Principled spatial filtering; scalable | Xenium/CosMx untested; CCC only (no niche output) | Cang et al., *Nature Methods* 2023 |
| **CellChat v2** | Statistical L-R inference + spatial mode | Partial — spatial mode is an add-on to non-spatial core | Visium, seqFISH, MERFISH, Slide-seq, STARmap | Largest L-R database; most widely used; rich visualization | Originally non-spatial; spatial mode less validated | Jin et al., *Nature Communications* 2024 |
| **SpaTalk** | Graph network modeling of spatially resolved L-R interactions | Yes — models interactions at single-cell resolution | SpaTalk-tested on ST, Slide-seq, MERFISH | Single-cell resolution L-R; cell-type deconvolution built in | Smaller L-R database than CellChat | Shao et al., *Nature Communications* 2022 |
| **SpatialDM** | Bivariate Moran's I for spatially co-expressed L-R pairs | Yes — tests spatial co-expression statistically | Visium, Slide-seq, seqFISH | Simple, statistically rigorous | Tests co-expression, not interaction; spot-level | Li et al., *Nature Communications* 2023 |

### Niche Gene Programs

| Tool | What It Tests | Spatial-Aware? | Platforms Tested | Key Strength | Key Limitation | Reference |
|------|-------------|---------------|-----------------|-------------|----------------|-----------|
| **[Niche-DE](../deep-reads/niche-de.md)** | Whether gene expression depends on niche context, not cell type alone | Yes — conditions on niche assignment | Visium, Slide-seq, CosMx, Xenium | Separates niche effect from cell-intrinsic expression; niche-LR extension for mechanistic insight | niche-LR fails on small gene panels (~300 genes) | Bai et al., *Genome Biology* 2024 |
| **NCEM** | How neighborhood composition influences gene expression via GNN | Yes — models neighborhood as graph context | Visium, MERFISH | Learns niche→expression mapping; interpretable node-centric model | Requires cell-type annotations | Fischer et al., *Nature Biotechnology* 2022 |
| **MISTy** | Intra- and intercellular spatial relationships via explainable ML | Yes — multiview model with spatial ranges | Visium, IMC, seqFISH | Explainable (feature importance per spatial range); modular views | R-only; computationally heavy on large datasets | Tanevski et al., *Genome Biology* 2022 |

### Morphology Integration

| Tool | What It Does | Spatial-Aware? | Platforms Tested | Key Strength | Key Limitation | Reference |
|------|-------------|---------------|-----------------|-------------|----------------|-----------|
| **[TESLA](../deep-reads/tesla.md)** | Super-resolution annotation combining H&E with spatial expression | Yes — pixel-level integration | Visium (spot-level only) | Subdivides spots below Visium resolution using histology | Visium spot-level only — cannot process Xenium/CosMx | Hu et al., *Cell Systems* 2023 |
| **METI** | Maps TME interactions by combining H&E morphology with spatial expression | Yes — morphological feature extraction | Visium (spot-level only) | Integrates tissue morphology into niche characterization | Visium spot-level only; calls `sc.read_visium()` exclusively | Hu et al., *Nature Communications* 2024 |

---

## How to Pick

**Start with your question:**

| Your Question | Stage | Recommended Tool(s) |
|--------------|-------|---------------------|
| What are the spatial neighborhoods in my tissue? | Recognition | CellCharter (composition) or BANKSY (expression) |
| What signaling drives each neighborhood? | Characterization | NicheCompass (spatial L-R) or COMMOT (optimal transport) |
| Which genes are niche-dependent, not just cell-type markers? | Characterization | Niche-DE |
| Do RNA niches agree with protein niches? | Recognition ×2 | CellCharter on each modality independently |
| What are the tissue regions (not niches)? | Domain detection | GraphST or STAGATE (but know these are not niches) |
| Can I get niche analysis without cell-type annotations? | Recognition | BANKSY (expression-based) or Novae (zero-shot) |
| I need to interpret niches mechanistically | Both | NicheCompass (L-R programs define niches) |

---

*See also: [What Is a Niche?](what-is-a-niche.md) | [Niche vs Domain](niche-vs-domain.md) | [Tool Selection for Hackathon](../hackathon/tool-selection.md)*
