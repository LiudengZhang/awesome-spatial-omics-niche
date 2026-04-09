# Data Compatibility

This page maps our hackathon datasets against the tools in our pipeline, documents platform constraints, and provides a focused overview of our three selected tools — including how they have been used in published studies.

---

## Our Datasets

| Dataset | Platform | Modality | Resolution | Approx. Genes/Markers | Notes |
|---------|----------|----------|-----------|----------------------|-------|
| **D1** | 10x Xenium | RNA | Single-cell | ~300 genes | Targeted panel |
| **D1** | Protein panel | Protein | Single-cell | TBD | Same tissue as Xenium |
| **D1** | H&E | Morphology | Pixel-level | — | Paired with spatial data |
| **D4** | 10x Xenium | RNA | Single-cell | ~300 genes | Targeted panel |
| **D4** | Visium HD | RNA | Sub-spot | Whole transcriptome | High-resolution spots |
| **D4** | Visium v2 | RNA | Spot-level (55 um) | Whole transcriptome | Standard resolution |
| **D4** | scRNA-seq | RNA | Dissociated | Whole transcriptome | No spatial information |
| **D4** | 5'GEX + TCR VDJ | RNA + TCR | Dissociated | Whole transcriptome + TCR | Immune repertoire |
| **D4** | H&E | Morphology | Pixel-level | — | Paired with spatial data |
| **V1** | NanoString CosMx | RNA | Single-cell | ~1,000 genes | Larger panel than Xenium |
| **V1** | CODEX | Protein | Single-cell | ~50 markers | Protein-only |

---

## Platform Compatibility Matrix

How each tool performs on our specific platforms. Verified against source papers and codebases.

:white_check_mark: = paper-tested | :warning: = listed/likely but untested | :x: = incompatible

### Niche Recognition Tools

| Tool | Xenium (D1, D4) | Protein (D1) | CosMx (V1) | CODEX (V1) | Visium (D4) |
|------|:---:|:---:|:---:|:---:|:---:|
| **CellCharter** | :warning: | :warning: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| **BANKSY** | :x: | :x: | :white_check_mark: | :x: | :white_check_mark: |
| **NicheCompass** | :white_check_mark: | :x: | :white_check_mark: | :x: | :x: |
| **Nicheformer** | :x: | :x: | :x: | :x: | :white_check_mark: |
| **ENVI/COVET** | :x: | :x: | :x: | :x: | :x: |
| **SpiceMix** | :x: | :x: | :x: | :x: | :x: |
| **CytoCommunity** | :x: | :x: | :x: | :white_check_mark: | :x: |
| **SpatialLDA** | :x: | :x: | :x: | :white_check_mark: | :x: |
| **Novae** | :warning: | :x: | :x: | :x: | :white_check_mark: |

### Functional Characterization Tools

| Tool | Xenium (D1, D4) | Protein (D1) | CosMx (V1) | CODEX (V1) | Visium (D4) |
|------|:---:|:---:|:---:|:---:|:---:|
| **NicheCompass** | :white_check_mark: | :x: | :white_check_mark: | :x: | :x: |
| **COMMOT** | :x: | :x: | :x: | :x: | :white_check_mark: |
| **CellChat v2** | :x: | :x: | :x: | :x: | :white_check_mark: |
| **SpaTalk** | :x: | :x: | :x: | :x: | :x: |
| **SpatialDM** | :x: | :x: | :x: | :x: | :white_check_mark: |
| **Niche-DE** | :white_check_mark: | :x: | :white_check_mark: | :x: | :white_check_mark: |
| **NCEM** | :x: | :x: | :x: | :x: | :white_check_mark: |
| **MISTy** | :x: | :x: | :x: | :x: | :white_check_mark: |
| **TESLA** | :x: | :x: | :x: | :x: | :white_check_mark: |
| **METI** | :x: | :x: | :x: | :x: | :white_check_mark: |

!!! warning "Takeaway"
    Most tools were developed and tested on Visium or MERFISH. For our Xenium + CosMx data, only **CellCharter**, **NicheCompass**, and **Niche-DE** have verified compatibility. This is the primary reason they were selected.

---

## Small Panel Constraints

Xenium's ~300-gene targeted panel creates specific limitations:

| Constraint | Affected Tool | Impact | Workaround |
|-----------|--------------|--------|------------|
| Too few genes for L-R matching | NicheCompass | Many L-R gene programs are lost because ligands or receptors are not in the panel | The NicheCompass paper used gene imputation for small-panel seqFISH data; core niche identification still works |
| niche-LR extension fails | Niche-DE | The mechanistic niche-LR extension cannot match L-R pairs with ~300 genes | Core niche-DE (context-dependent gene identification) works fully; only the L-R extension is affected |
| Covariance is less informative | ENVI/COVET | Gene-gene covariance tensors are sparse with 300 genes vs. whole transcriptome | Requires matched scRNA-seq (available in D4) for imputation via ENVI |
| Expression-based methods lose resolution | BANKSY, SpiceMix | Neighbor expression augmentation is less distinctive with small panels | Better suited for whole-transcriptome platforms (Visium, MERFISH) |

CosMx's ~1,000-gene panel partially alleviates these issues for V1 validation.

---

## Our Picks: Tool Overviews

### CellCharter — Niche Identification

**Focus:** Identifies spatial niches by learning cell-type composition patterns across neighborhoods at multiple spatial resolutions.

**How it works:** Builds a spatial graph from cell coordinates, uses a graph neural network (GNN) to learn latent embeddings that encode the cell-type composition of each cell's neighborhood, then clusters these embeddings with a Gaussian mixture model. The multi-scale design lets it capture niches at different spatial resolutions simultaneously.

**Citation:** Varrone M, Tavernari D, Santamaria-Martínez A, Walsh LA, Ciriello G. *CellCharter reveals spatial cell niches associated with tissue remodeling and cell plasticity.* Nature Genetics 56: 74-84 (2024).

**Platform coverage:** Paper-tested on CODEX (CRC, colorectal cancer), CosMx (NSCLC), MERSCOPE (mouse brain), Visium (breast cancer), IMC (breast cancer). Xenium and Visium HD are listed as supported but not paper-tested.

**Used in published studies:**

| Study | How CellCharter Was Used | Context |
|-------|------------------------|---------|
| Wang/Liu et al., Cancer Cell 2025 | Referenced as a related composition-based approach for CAF spatial subtyping | Pan-cancer CAF atlas (14M cells, 7 platforms). Used a similar neighborhood-composition clustering approach across MERSCOPE, CosMx, Xenium, CODEX, IMC. |
| Prakrithi et al., bioRxiv 2025 | Referenced for spatial community detection methodology | 12-technology skin cancer atlas. Used per-platform community detection (10 communities each) then cross-platform correlation to form meta-communities. |

**Relevance to our pipeline:** The only niche tool verified on both RNA (CosMx, MERSCOPE) and protein (CODEX, IMC) platforms. This makes it uniquely suited for our Step 4 (RNA vs protein concordance), where we run it independently on each modality of D1 and V1.

---

### NicheCompass — Niche Communication

**Focus:** Jointly identifies niches and quantifies the ligand-receptor signaling that defines them. Unlike tools that treat niche ID and communication as separate steps, NicheCompass defines niches *by* their communication patterns.

**How it works:** Builds a spatial graph and trains a variational graph neural network with a biologically informed prior: known ligand-receptor interaction databases are encoded into the model architecture. The learned latent space captures inter- and intracellular signaling programs, enabling both niche identification and mechanistic interpretation of what drives each niche.

**Citation:** Birk S, Bonafonte-Pardàs I, Grigsby M, Stanojevic S, Bok S, Metzger D, Lotfollahi M. *Quantitative characterization of cell niches in spatially resolved omics data.* Nature Genetics (2025).

**Platform coverage:** Paper-tested on Xenium (breast cancer, ~286K cells) and CosMx (NSCLC, ~800K cells across 8 samples). Also tested on seqFISH (mouse embryo, with gene imputation for small panel). RNA-only — no protein modality support.

**Used in published studies:**

| Study | How NicheCompass (or Similar Communication-Based Analysis) Was Used | Context |
|-------|------------------------------------------------------------------|---------|
| Peng/Kadara et al., Cancer Cell 2026 | Used L-R communication analysis (IL1B-IL1R1 axis) to characterize proinflammatory niches | Lung precancer progression. Identified epithelial-proinflammatory niches via co-localization + L-R signaling, validated with mouse model + therapeutic intervention. |
| Prakrithi et al., bioRxiv 2025 | Used CellChat + stLearn SCTP for spatially-constrained L-R analysis | 12-technology skin cancer atlas. Found that spatially-constrained CCC detects interactions (e.g., WNT5A-ROR1) missed by non-spatial scRNAseq-based CellChat. |
| Wang/Liu et al., Cancer Cell 2025 | Used CellChat for L-R communication per spatial CAF subtype | Pan-cancer CAF atlas. Each of 4 spatial CAF subtypes had distinct interaction networks. |
| Wang/Ali et al., Nature 2023 | Used direct cell-mask adjacency + permutation tests for interaction scoring | TNBC immunotherapy prediction. Spatial contact between GzmB+ CD8+ T cells and cancer cells predicted response. |

**Relevance to our pipeline:** The only communication tool paper-tested on both Xenium and CosMx. Provides mechanistic interpretation of *why* niches exist (L-R programs), not just *where* they are. Applied to D1 + D4 Xenium for discovery, V1 CosMx for validation. CODEX skipped (protein-only, incompatible).

---

### Niche-DE — Niche Gene Programs

**Focus:** Tests whether a gene's expression depends on the cell's niche context, beyond what cell type alone would predict. Separates niche-driven expression from cell-intrinsic programs.

**How it works:** Takes niche assignments (from any niche-calling tool like CellCharter) and tests each gene for differential expression that is explained by niche membership after controlling for cell type. This identifies genes that are "turned on" or "turned off" specifically because of where a cell sits in the tissue, not because of what type of cell it is. An optional niche-LR extension traces these niche-dependent genes back to specific ligand-receptor interactions.

**Citation:** Bai Z, Zhang N, Mason KA. *Niche-DE: niche-differential gene expression analysis in spatial transcriptomics data identifies context-dependent cell-cell interactions.* Genome Biology 25: 1-29 (2024).

**Platform coverage:** Paper-tested on Visium (DLPFC), Slide-seq (mouse cerebellum), CosMx (NSCLC), and Xenium (breast cancer). The niche-LR extension requires sufficient L-R gene pairs in the panel — it cannot run on Xenium's ~300-gene panel but works on CosMx's ~1,000-gene panel and whole-transcriptome platforms.

**Used in published studies:**

| Study | How Niche-Dependent Gene Analysis Was Used | Context |
|-------|------------------------------------------|---------|
| Peng/Kadara et al., Cancer Cell 2026 | Identified stage-specific transcriptional programs in spatially defined niches | Lung precancer. Found that epithelial-proinflammatory niches have distinct gene programs at each disease stage (AAH → AIS → MIA → LUAD). |
| Meyer/Bodenmiller et al., Cancer Cell 2025 | Differential expression across spatial immune phenotypes (inflamed vs. excluded vs. cold) | TNBC stratification. Gene programs differed between CD8-infiltrated and CD8-excluded niches, validated in RNA-seq and scRNA-seq cohorts. |
| Sorin et al., Nature 2023 | Activation-state profiling across cellular neighborhoods | Lung adenocarcinoma. Protein activation markers (PD1, PDL1, HIF-1a) varied across B cell-enriched vs. macrophage-enriched neighborhoods. |
| Klughammer et al., Nature Medicine 2024 | Spatial DE: genes differentially expressed near vs. far from T/NK cells | Metastatic breast cancer. Identified potential immune evasion programs in malignant cells spatially distant from immune cells. |

**Relevance to our pipeline:** The only spatial-aware DE tool paper-tested on both Xenium and CosMx. Standard DE methods have 86-95% false positive rates on spatial data due to spatial autocorrelation (smiDE, Genome Biology 2025). Niche-DE controls for this. Core niche-DE works on Xenium; the niche-LR mechanistic extension is available on CosMx (V1) where the panel is larger.

---

## Summary

| Tool | Task | Works on Xenium? | Works on CosMx? | Works on CODEX? | Small Panel Limitation |
|------|------|:---:|:---:|:---:|----------------------|
| **CellCharter** | Niche ID | :warning: (listed, untested) | :white_check_mark: | :white_check_mark: | None — composition-based, not gene-dependent |
| **NicheCompass** | Communication | :white_check_mark: | :white_check_mark: | :x: | Loses L-R programs on ~300-gene panels |
| **Niche-DE** | Gene programs | :white_check_mark: | :white_check_mark: | :x: | Core works; niche-LR extension unavailable on small panels |

These three tools were selected because they are the **best-validated options for our specific platforms** (Xenium, CosMx, CODEX). Most competing tools were developed on Visium or MERFISH and have not been tested on the platforms in our datasets.

---

*See also: [Tool Selection](tool-selection.md) | [Decision Guide](../concepts/decision-guide.md) | [Literature Review](literature-review.md)*
