# Data Compatibility

This page maps our hackathon tools against our specific platforms, documents panel-size constraints, and provides a focused overview of our three selected tools — including how they have been used in published studies.

---

## Platform Compatibility Matrix

How each tool performs on our specific platforms. Verified against source papers and codebases.

Legend: **Yes** = paper-tested or independently validated | **Likely** = platform-agnostic design, should work but no Xenium-specific docs | **No** = incompatible, crashes, or requires unsupported data format

### Niche Recognition Tools

| Tool | Xenium | Protein | CosMx | CODEX | Visium |
|------|--------|---------|-------|-------|--------|
| **CellCharter** | **Yes** | **Yes** | **Yes** | **Yes** | **Yes** |
| **BANKSY** | **Yes** | **Yes** | **Yes** | **Yes** | **Yes** |
| **NicheCompass** | **Yes** | No | **Yes** | No | No |
| **Nicheformer** | **Yes** | No | No | No | **Yes** |
| **ENVI/COVET** | **Yes** | No | No | No | No |
| **SpiceMix** | No | No | No | No | No |
| **CytoCommunity** | Likely | **Yes** | No | **Yes** | No |
| **SpatialLDA** | No | Likely | No | **Yes** | No |
| **Novae** | **Yes** | No | No | No | **Yes** |

### Functional Characterization Tools

| Tool | Xenium | Protein | CosMx | CODEX | Visium |
|------|--------|---------|-------|-------|--------|
| **NicheCompass** | **Yes** | No | **Yes** | No | No |
| **COMMOT** | Likely | No | No | No | **Yes** |
| **CellChat v2** | **Yes** | No | No | No | **Yes** |
| **SpaTalk** | No | No | No | No | No |
| **SpatialDM** | No | No | No | No | **Yes** |
| **Niche-DE** | **Yes** | No | **Yes** | No | **Yes** |
| **NCEM** | No | No | No | No | **Yes** |
| **MISTy** | Likely | **Yes** | No | **Yes** | **Yes** |
| **TESLA** | No | No | No | No | **Yes** |
| **METI** | No | No | No | No | **Yes** |

!!! info "Compatibility is broader than expected"
    After checking each tool's GitHub repo, documentation, and issues: **8/19 tools support Xenium** (paper-tested or with dedicated docs/tutorials), and 4 more are likely compatible. **4/19 tools support protein data** (CODEX/IMC/MIBI-TOF): CellCharter (CODEX, IMC), BANKSY (CODEX), CytoCommunity (MIBI-TOF), MISTy (IMC). Most communication and DE tools (NicheCompass, CellChat, COMMOT, Niche-DE) are RNA-only because they rely on gene-symbol-based L-R databases. Foundation models (Nicheformer, Novae) are transcriptomics-only by design.

---

## Small Panel Constraints

Xenium's ~300-gene targeted panel creates specific limitations:

| Constraint | Affected Tool | Impact | Workaround |
|-----------|--------------|--------|------------|
| Too few genes for L-R matching | NicheCompass | Many L-R gene programs are lost because ligands or receptors are not in the panel | The NicheCompass paper used gene imputation for small-panel seqFISH data; core niche identification still works |
| niche-LR extension fails | Niche-DE | The mechanistic niche-LR extension cannot match L-R pairs with ~300 genes | Core niche-DE (context-dependent gene identification) works fully; only the L-R extension is affected |
| Covariance is less informative | ENVI/COVET | Gene-gene covariance tensors are sparse with 300 genes vs. whole transcriptome | Requires matched scRNA-seq (available in D4) for imputation via ENVI |
| Expression-based methods lose resolution | BANKSY, SpiceMix | Neighbor expression augmentation is less distinctive with small panels | BANKSY still ranked best on Xenium in benchmark (Nat Methods 2025); use lambda=0.2 for cell typing, skip log1p and HVG selection |

CosMx's ~1,000-gene panel partially alleviates these issues for V1 validation.

---

## Our Picks: Tool Overviews

### CellCharter — Niche Identification

**Focus:** Identifies spatial niches by learning cell-type composition patterns across neighborhoods at multiple spatial resolutions.

**How it works:** Builds a spatial graph from cell coordinates, uses a graph neural network (GNN) to learn latent embeddings that encode the cell-type composition of each cell's neighborhood, then clusters these embeddings with a Gaussian mixture model. The multi-scale design lets it capture niches at different spatial resolutions simultaneously.

**Citation:** Varrone M, Tavernari D, Santamaria-Martínez A, Walsh LA, Ciriello G. *CellCharter reveals spatial cell niches associated with tissue remodeling and cell plasticity.* Nature Genetics 56: 74-84 (2024).

**Platform coverage:** Paper-tested on CODEX (CRC, colorectal cancer), CosMx (NSCLC), MERSCOPE (mouse brain), Visium (breast cancer), IMC (breast cancer). Xenium is listed as supported and independently validated (see below).

**Used in published studies:**

| Study | Platform | How CellCharter Was Used | Context |
|-------|----------|------------------------|---------|
| Esophageal adenocarcinoma, *Cell Reports Medicine* 2025 | **Xenium (16 samples)** | Identified shared spatial niches; assigned CellCharter clusters and computed cell-type proportions per cluster | Primary and metastatic esophageal adenocarcinoma across 6 patients. |
| Restrepo et al., *Nature Genetics* 2026 | MERFISH | Ran CellCharter v0.3.4 with scANVI; identified 10 multicellular neighborhoods | Human skin anatomy atlas (~1.2M cells, 114 samples). |
| SACCELERATOR benchmark, 2025 | **Xenium (mouse brain)** | Benchmarked CellCharter among 22 spatial clustering methods; among top performers by ARI | Consensus benchmarking across 15 datasets, 9 technologies. |
| Subcellular platform benchmark, *Nature Communications* 2025 | **Xenium 5K** + CODEX | Used CellCharter for spatial clustering to evaluate cross-platform concordance | Benchmarked Stereo-seq, Visium HD, CosMx 6K, Xenium 5K on cancer tissues. |

**Relevance to our pipeline:** The only niche tool verified on both RNA (CosMx, MERSCOPE) and protein (CODEX, IMC) platforms. This makes it uniquely suited for our Step 4 (RNA vs protein concordance), where we run it independently on each modality of D1 and V1.

---

### NicheCompass — Niche Communication

**Focus:** Jointly identifies niches and quantifies the ligand-receptor signaling that defines them. Unlike tools that treat niche ID and communication as separate steps, NicheCompass defines niches *by* their communication patterns.

**How it works:** Builds a spatial graph and trains a variational graph neural network with a biologically informed prior: known ligand-receptor interaction databases are encoded into the model architecture. The learned latent space captures inter- and intracellular signaling programs, enabling both niche identification and mechanistic interpretation of what drives each niche.

**Citation:** Birk S, Bonafonte-Pardàs I, Grigsby M, Stanojevic S, Bok S, Metzger D, Lotfollahi M. *Quantitative characterization of cell niches in spatially resolved omics data.* Nature Genetics (2025).

**Platform coverage:** Paper-tested on Xenium (breast cancer, ~286K cells) and CosMx (NSCLC, ~800K cells across 8 samples). Also tested on seqFISH (mouse embryo, with gene imputation for small panel). RNA-only — no protein modality support.

**Used in published studies:**

| Study | Platform | How NicheCompass Was Used | Context |
|-------|----------|-------------------------|---------|
| Oligodendroglia vulnerability, *Acta Neuropathologica* 2025 | **Xenium (366 genes)** | Identified cellular domains by integrating spatial transcriptomics across samples | Parkinson's disease. Human dorsal striatum, small panel similar to our ~300-gene Xenium. |
| Skin fibroblasts atlas, *Nature Immunology* 2025 | **Xenium 5K** + Visium | Ran NicheCompass with 1,024 spatially variable genes, 8 neighbors per cell | Atopic dermatitis. Human skin fibroblast subtypes across tissues. |
| Cardiac-immune microniches, *bioRxiv* 2026 | Visium + MERFISH | Segmented injury area into 7 distinct microniches with distinct cell-cell communication programs | Cardiac regeneration. Zebrafish heart. |

!!! note "Adoption status"
    NicheCompass was published in March 2025 and adoption is still early. Most spatial CCC studies currently use CellChat (Wang/Liu et al. 2025), stLearn (Prakrithi et al. 2025), or custom L-R analysis (Peng/Kadara et al. 2026) rather than NicheCompass.

**Relevance to our pipeline:** The only communication tool paper-tested on both Xenium and CosMx. Provides mechanistic interpretation of *why* niches exist (L-R programs), not just *where* they are. Applied to D1 + D4 Xenium for discovery, V1 CosMx for validation. CODEX skipped (protein-only, incompatible).

---

### Niche-DE — Niche Gene Programs

**Focus:** Tests whether a gene's expression depends on the cell's niche context, beyond what cell type alone would predict. Separates niche-driven expression from cell-intrinsic programs.

**How it works:** Takes niche assignments (from any niche-calling tool like CellCharter) and tests each gene for differential expression that is explained by niche membership after controlling for cell type. This identifies genes that are "turned on" or "turned off" specifically because of where a cell sits in the tissue, not because of what type of cell it is. An optional niche-LR extension traces these niche-dependent genes back to specific ligand-receptor interactions.

**Citation:** Bai Z, Zhang N, Mason KA. *Niche-DE: niche-differential gene expression analysis in spatial transcriptomics data identifies context-dependent cell-cell interactions.* Genome Biology 25: 1-29 (2024).

**Platform coverage:** Paper-tested on Visium (DLPFC), Slide-seq (mouse cerebellum), CosMx (NSCLC), and Xenium (breast cancer). The niche-LR extension requires sufficient L-R gene pairs in the panel — it cannot run on Xenium's ~300-gene panel but works on CosMx's ~1,000-gene panel and whole-transcriptome platforms.

**Adoption in published studies:**

!!! note "Adoption status"
    Niche-DE was published in 2024 and no independent studies using it as a pipeline tool have been confirmed yet. The concept of niche-dependent gene analysis is widely practiced — papers like Peng/Kadara et al. (Cancer Cell 2026), Meyer/Bodenmiller et al. (Cancer Cell 2025), and Klughammer et al. (Nature Medicine 2024) all test differential expression across spatial niches — but they use custom DE approaches or standard tools (edgeR, DESeq2) rather than Niche-DE specifically. Niche-DE's advantage over these approaches is that it explicitly controls for spatial autocorrelation, which standard DE methods do not.

**Relevance to our pipeline:** The only spatial-aware DE tool paper-tested on both Xenium and CosMx. Standard DE methods have 86-95% false positive rates on spatial data due to spatial autocorrelation (smiDE, Genome Biology 2025). Niche-DE controls for this. Core niche-DE works on Xenium; the niche-LR mechanistic extension is available on CosMx (V1) where the panel is larger.

---

## Summary

| Tool | Task | Works on Xenium? | Works on CosMx? | Works on CODEX? | Small Panel Limitation |
|------|------|-----------------|----------------|----------------|----------------------|
| **CellCharter** | Niche ID | **Yes** (independently validated) | **Yes** | **Yes** | None — composition-based, not gene-dependent |
| **NicheCompass** | Communication | **Yes** | **Yes** | No | Loses L-R programs on ~300-gene panels |
| **Niche-DE** | Gene programs | **Yes** | **Yes** | No | Core works; niche-LR extension unavailable on small panels |

These three tools were selected because they are the **best-validated options for our specific platforms** (Xenium, CosMx, CODEX). Most competing tools were developed on Visium or MERFISH and have not been tested on the platforms in our datasets.

---

*See also: [Tool Selection](tool-selection.md) | [Decision Guide](../concepts/decision-guide.md) | [Literature Review](literature-review.md)*
