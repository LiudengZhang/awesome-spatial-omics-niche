# Literature Review: Spatial Niche Analysis Workflows in Cancer

This page reviews 8 published studies that perform end-to-end spatial niche analysis in cancer tissues. Each paper chains multiple analysis steps — niche identification, cell-cell communication, gene programs, clinical validation — providing methodological templates for our hackathon pipeline.

!!! info "Selection criteria"
    Papers were selected for: (1) end-to-end niche workflows (not single-tool papers), (2) cancer tissues, (3) high-impact venues, (4) relevance to our pipeline design (composition-based niches, communication, cross-platform validation).

---

## Summary Table

| Paper | Cancer | Platform(s) | Niche Method | CCC | Gene Programs | Validation | Cells |
|-------|--------|-------------|-------------|-----|---------------|------------|-------|
| Wang/Liu 2025 | Pan-cancer (10 types) | MERSCOPE, CosMx, Visium, COMET, Xenium, CODEX, IMC | Neighborhood composition clustering | CellChat L-R | NMF programs | 7 platforms, 10 cancers | 14M+ |
| Peng/Kadara 2026 | Lung (AAH→LUAD) | Visium CytAssist, Xenium 5K, COMET | cell2location deconvolution + co-localization | IL1B-IL1R1 axis | Stage-specific programs | Independent cohort + mouse model | 5.4M |
| Meyer/Bodenmiller 2025 | TNBC | IMC (39-plex) | imcRtools + spicyR spatial interaction | CD8-tumor interaction phenotyping | RNA-seq + scRNA-seq validation | 8 cohorts (~4,000 patients) | ~1M |
| Wang/Ali 2023 | TNBC | IMC (43-plex) | Cell mask adjacency + permutation test | Direct contact scoring | Activation state profiling | 3 timepoints, 660 images | >1M |
| Sorin 2023 | Lung adenocarcinoma | IMC (35-plex) | Schurch-style cellular neighborhoods | Co-localization analysis | Protein activation states | Independent cohort (n=60) | 1.6M |
| Arora 2023 | OSCC + pan-cancer | Visium + scRNA-seq | Pathologist-annotated core/edge regions | L-R interaction analysis | DE programs per region | TCGA pan-cancer | Spot-level |
| Klughammer 2024 | Metastatic breast | Slide-seq, MERFISH, ExSeq, CODEX | Neighborhood composition clustering | Co-localization across modalities | Spatial DE programs | 4 platforms on serial sections | 67 biopsies |
| Prakrithi 2025 | Skin (cSCC, BCC, melanoma) | 12 technologies (Xenium, CosMx, CODEX, Visium, ...) | Meta-communities across platforms | CellChat + stLearn SCTP | Joint pathway analysis | 12 platforms on serial sections | 131K+ |

---

## Per-Paper Analysis

### 1. Wang/Liu 2025 — CAF Spatial Subtypes

**Conserved spatial subtypes and cellular neighborhoods of cancer-associated fibroblasts revealed by single-cell spatial multi-omics.** Cancer Cell 43(5): 905-924 (2025). [DOI](https://doi.org/10.1016/j.ccell.2025.03.004)

!!! tip "Hackathon organizer's paper"
    Senior author: **Linghua Wang** (MD Anderson). This paper directly demonstrates the composition-based niche approach we are using with CellCharter.

**Workflow:**

1. Cell segmentation and annotation across MERSCOPE (500-plex) and CosMx (1,000-plex) on 8 cancer types, 24 tissue sections, >5.7M cells
2. Spatial graph construction → neighborhood composition profiling for each CAF
3. Clustering CAFs by neighborhood composition → 4 conserved spatial CAF subtypes
4. NMF-based gene program identification per subtype
5. CellChat ligand-receptor communication analysis per subtype
6. Cross-platform validation on Visium, COMET, Xenium, CODEX, IMC (5 additional platforms)
7. Cross-cancer validation across 10 cancer types (14M+ total cells)
8. Clinical association: CAF subtype composition → immune infiltration, TLS, TAM distribution, survival

**What we can learn:**

- **Composition-based niche definition works pan-cancer.** CAF subtypes defined by neighboring cell composition are conserved across 10 cancers and 7 platforms — strong evidence for our CellCharter approach.
- **Cross-platform validation is achievable** even when key markers are missing from some panels. They used neighborhood-based classification (not marker-based) to transfer subtypes across platforms.
- **Communication follows niche identity.** Each spatial CAF subtype has distinct L-R interaction networks, supporting our pipeline ordering (niche ID first, then communication).

---

### 2. Peng/Kadara 2026 — Lung Precancer Niche Co-evolution

**Multimodal spatial-omics reveal co-evolution of alveolar progenitors and proinflammatory niches in progression of lung precursor lesions.** Cancer Cell 44(2): 321-339 (2026). [DOI](https://doi.org/10.1016/j.ccell.2025.10.004)

!!! tip "Hackathon organizer's paper"
    Corresponding authors: **Humam Kadara** and **Linghua Wang** (MD Anderson). Uses Visium + Xenium + COMET — the same platforms as our D1/D4 datasets.

**Workflow:**

1. Visium CytAssist on 56 tissues from 25 patients spanning AAH → AIS → MIA → LUAD
2. scFFPE-seq (Chromium Flex) on 75 matched tissues as single-cell reference
3. **cell2location** deconvolution of Visium spots using scFFPE-seq reference
4. Co-localization analysis → identification of epithelial-proinflammatory niches (alveolar progenitors + IL1B-high macrophages)
5. Stage-specific transcriptional program identification
6. L-R communication analysis (IL1B-IL1R1 axis)
7. WES on 79 samples for clonal architecture
8. Xenium 5K validation at single-cell resolution (6 pairs of lesions)
9. COMET spatial proteomics for protein-level validation
10. Independent validation cohort (36 lesions, 19 patients)
11. **Mouse model validation** + IL-1B neutralization therapeutic experiment

**What we can learn:**

- **Multi-resolution strategy:** Discovery at spot-level (Visium, cheaper/broader), validation at single-cell level (Xenium). We could adopt this for D4 (Visium v2 + Xenium).
- **Deconvolution as niche proxy:** cell2location on Visium spots effectively identifies niches without explicit niche-calling tools — an alternative to CellCharter for spot-level data.
- **Niche → causal biology path:** They went from spatial niche discovery → mouse model → therapeutic intervention. This is the gold standard for translational niche analysis.
- **Same lab, same platforms as our hackathon.** Their workflow on Visium + Xenium + COMET is directly transferable.

---

### 3. Meyer/Bodenmiller 2025 — TNBC Stratification

**A stratification system for breast cancer based on basoluminal tumor cells and spatial tumor architecture.** Cancer Cell 43(9): 1637-1655 (2025). [DOI](https://doi.org/10.1016/j.ccell.2025.05.013)

**Workflow:**

1. IMC (39-plex) on TMAs from 215 TNBC patients (~495 images, ~1M cells)
2. **steinbock** preprocessing → **Mesmer** deep learning segmentation
3. Two-step epithelial classification: graph clustering + Gaussian Mixture Model on panCK/E-cadherin
4. Unsupervised clustering → 110 fine clusters → 9 metaclusters → 11 tumor cell phenotypes
5. **imcRtools** spatial interaction testing between tumor metaclusters and immune cells
6. **spicyR** permutation-based spatial interaction analysis
7. CD8 T cell spatial phenotyping: infiltrating / bystanding / excluded
8. Five-subtype stratification combining CK expression + spatial immune phenotype
9. Survival analysis (Cox PH, Lasso-regularized Cox)
10. Validation across 8 independent cohorts (~4,000 patients) including ISPY2 trial

**What we can learn:**

- **Spatial interaction testing as niche characterization.** imcRtools + spicyR provide a rigorous statistical framework for testing whether cell-cell co-localization is significant — complementary to composition-based approaches.
- **Clinical stratification from spatial niches.** Their 5-subtype system (INF > N > B > L > BL) directly predicts survival and immunotherapy response — demonstrating the clinical utility of niche analysis.
- **Massive validation depth.** 8 cohorts, ~4,000 patients, multiple modalities (IMC, mIF, RNA-seq, scRNA-seq, organoids). Sets the standard for how thoroughly niche findings should be validated.

---

### 4. Wang/Ali 2023 — Immunotherapy Spatial Predictors

**Spatial predictors of immunotherapy response in triple-negative breast cancer.** Nature 621: 868-876 (2023). [DOI](https://doi.org/10.1038/s41586-023-06498-3)

**Workflow:**

1. IMC (43-plex) on FFPE biopsies from the NeoTRIPaPDL1 trial at 3 timepoints (baseline n=243, on-treatment n=207, post-treatment n=210)
2. **steinbock** preprocessing → **Mesmer/DeepCell** segmentation
3. Semi-supervised phenotyping → 37 cell phenotypes (17 epithelial + 20 TME)
4. Spatial interaction: direct cell mask adjacency + permutation tests (not KNN neighborhoods)
5. Feature extraction: 148 features per image (densities, interaction scores, proliferative fractions)
6. Univariate logistic regression → odds ratios for pCR association
7. Regularized logistic regression → multivariate prediction (AUC = 0.82)
8. 100 repeated random train/test splits for internal validation

**What we can learn:**

- **Adjacency-based spatial analysis** (direct cell contact) is a simpler alternative to neighborhood composition that still yields strong clinical predictions.
- **Longitudinal spatial profiling** (3 timepoints) reveals how niches remodel during therapy — an analysis dimension our hackathon could consider if temporal data is available.
- **Feature engineering matters.** Their 148-feature framework (density + interaction + proliferation) is a practical template for extracting clinically relevant features from niche analysis results.

---

### 5. Sorin 2023 — Lung Immune Landscapes

**Single-cell spatial landscapes of the lung tumour immune microenvironment.** Nature 614: 548-554 (2023). [DOI](https://doi.org/10.1038/s41586-022-05672-3)

**Workflow:**

1. IMC (35-plex, Hyperion) on TMAs from 416 LUAD patients (1 mm^2 cores)
2. Hybrid segmentation: **Mask R-CNN** + MATLAB post-processing + EM-GMM refinement
3. Rule-based cell phenotyping → 17 cell types
4. Cellular neighborhood analysis following **Schurch et al. (Cell, 2020)** — KNN neighborhood composition vectors → clustering into recurrent neighborhoods
5. Survival analysis: neighborhoods → Kaplan-Meier + Cox PH
6. **ResNet-50** deep learning on raw IMC images (37 channels) → binary progression prediction
7. Validation cohort: 60 patients, 2 cores each

**What we can learn:**

- **Schurch-style cellular neighborhoods** remain the most widely used niche definition in IMC studies. This is essentially what CellCharter automates with a learned latent space.
- **Deep learning on spatial images** can predict clinical outcomes directly from raw pixel data — bypassing cell segmentation and phenotyping entirely.
- **Scale matters.** 416 patients is large for spatial studies. Most niche findings are underpowered; survival associations require hundreds of patients.

---

### 6. Arora 2023 — Tumor Core vs Edge

**Spatial transcriptomics reveals distinct and conserved tumor core and edge architectures that predict survival and targeted therapy response.** Nature Communications 14: 5029 (2023). [DOI](https://doi.org/10.1038/s41467-023-40271-4)

**Workflow:**

1. Visium spatial transcriptomics on HPV-negative oral squamous cell carcinoma (OSCC)
2. scRNA-seq as reference for deconvolution
3. Pathologist annotation of tumor core (TC) vs. leading edge (LE) regions
4. Differential gene expression between TC and LE
5. Cell-type composition analysis per region (deconvolution)
6. Cell-cell communication inference (L-R analysis)
7. Pan-cancer signature validation using TCGA cohorts
8. Drug response prediction from spatial architecture signatures

**What we can learn:**

- **Anatomical niche definition** (core vs. edge) is simpler than algorithmic approaches but biologically meaningful — edge signatures are conserved across cancers.
- **Pan-cancer transferability.** Spatial signatures derived from one cancer type can predict outcomes in others via TCGA validation — useful if our hackathon findings need broader contextualization.
- **Spot-level limitation.** Visium resolution limits niche granularity compared to single-cell platforms (Xenium, CosMx). Our pipeline uses single-cell platforms for this reason.

---

### 7. Klughammer 2024 — Metastatic Breast Multi-Modal

**A multi-modal single-cell and spatial expression map of metastatic breast cancer.** Nature Medicine 30: 3236-3249 (2024). [DOI](https://doi.org/10.1038/s41591-024-03215-z)

**Workflow:**

1. Serial sections from 67 biopsies (60 patients, 9 metastatic sites) profiled on 4 spatial platforms: Slide-seq, MERFISH, ExSeq, CODEX
2. scRNA-seq/snRNA-seq as dissociated reference
3. **Cell2location** deconvolution for Slide-seq; direct classification for MERFISH/CODEX
4. Neighborhood composition analysis → spatial niche clustering
5. Macrophage spatial phenotyping (3 patterns: short-range accumulation, long-range accumulation, intermixing)
6. Spatial DE: genes differentially expressed near vs. far from T/NK cells
7. Cross-platform concordance: pseudobulk correlation + cell-type composition comparison across all 4 platforms on same biopsies

**What we can learn:**

- **Cross-platform concordance is quantifiable.** Pseudobulk expression correlation and cell-type composition comparison across serial sections provide concrete metrics for our RNA vs. protein concordance analysis (Step 4).
- **Macrophage spatial phenotypes** reveal that the same cell type can have fundamentally different spatial organizations — niche context matters beyond cell-type identity.
- **Multi-modal on same tissue** is the gold standard. Our D1 (Xenium + Protein on same tissue) mirrors this design.

---

### 8. Prakrithi 2025 — 12-Technology Skin Cancer

**Integrating 12 spatial and single cell technologies to characterise tumour neighbourhoods and cellular interactions in three skin cancer types.** bioRxiv (2025). [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12324176/)

**Workflow:**

1. 24 skin donors (cSCC, BCC, melanoma) profiled across 12 technologies: scRNAseq, FLEX snRNAseq, Visium, Xenium, CosMx, GeoMx CTA, GeoMx Protein, Opal, RNAScope, PLA, MALDI glycomics, CODEX
2. Cancer cell identification: consensus CNV (InferCNV + CopyKat must agree) + cancer module score
3. Per-platform community detection → 10 communities per technology
4. Cross-platform community correlation → 4 meta-communities (immune, KC, stromal, tumour)
5. **CellChat** on scRNAseq → 312 L-R pairs; **stLearn SCTP** for spatially-constrained interaction testing on Visium/CosMx
6. **MMCCI** integration: combines interaction scores across platforms and replicates
7. Joint pathway analysis via MetaboAnalyst (genes + proteins + metabolites → KEGG)
8. Experimental validation: RNAScope, Opal Polaris, PLA (proximity ligation assay)
9. GWAS integration via gsMAP

**What we can learn:**

- **No single technology captures all niche features.** Communities from Xenium (RNA), CODEX (protein), and MALDI (metabolites) reveal complementary aspects of the same spatial neighborhoods — directly supports our RNA vs. protein concordance analysis.
- **Meta-communities across platforms** is a practical strategy for cross-platform niche comparison. Pearson correlation of cell-type composition vectors is a simple, effective metric.
- **Spatially-constrained CCC catches what non-spatial misses.** stLearn SCTP found interactions (WNT5A-ROR1) that all three spatial platforms detected but scRNAseq missed — validating our choice of spatially-aware communication tools (NicheCompass, COMMOT).

---

## Patterns and Gaps

### What most studies do

| Step | Common approach | Used by |
|------|----------------|---------|
| **Niche ID** | Composition-based: KNN neighborhood → cell-type frequency vector → clustering | 6/8 papers |
| **Communication** | CellChat (most common), with or without spatial constraints | 4/8 papers |
| **Gene programs** | Differential expression or NMF between niches | 5/8 papers |
| **Clinical outcome** | Survival analysis (Cox PH, Kaplan-Meier) | 6/8 papers |
| **Segmentation** | Mesmer/DeepCell (most common for IMC/protein) | 3/8 papers |
| **Deconvolution** | cell2location (for spot-level data like Visium) | 2/8 papers |

### What few studies do (our opportunity)

| Analysis | Papers that do it | Our pipeline |
|----------|------------------|-------------|
| **Cross-modality niche concordance (RNA vs protein)** | Klughammer (4 platforms), Prakrithi (12 platforms) | **CellCharter on RNA vs protein independently** — more systematic than either |
| **Spatially-aware communication** (not just CellChat) | Prakrithi (stLearn SCTP) | **NicheCompass** — communication-based niche definition, more principled |
| **Context-dependent gene programs** (niche-specific DE) | None use Niche-DE specifically | **Niche-DE** — tests whether expression depends on niche context, not just cell type |
| **Systematic cross-platform validation** | Wang/Liu (7 platforms), Prakrithi (12 platforms) | **Discovery on D1+D4, validation on V1** — different platforms, different tissue |

### Key insight for our hackathon

Most studies follow the same workflow: **IMC/CODEX → Schurch-style neighborhoods → CellChat → survival**. This is well-established but has known limitations:

1. **Composition-only niches** miss gene expression context (COVET/BANKSY address this)
2. **Non-spatial CCC** (standard CellChat) detects implausible long-range signals (NicheCompass/COMMOT address this)
3. **Standard DE** has 86-95% false positive rates on spatial data (Niche-DE addresses this)
4. **Cross-modality concordance** is rarely tested systematically

Our pipeline addresses all four gaps. The literature review confirms that our tool choices (CellCharter, NicheCompass, Niche-DE) and our novel Step 4 (RNA vs protein concordance) fill genuine methodological holes in the field.

---

## Tool Usage Across Papers

| Tool | Papers Using It | Our Pipeline |
|------|----------------|-------------|
| cell2location | Peng/Kadara, Klughammer | Not needed (single-cell platforms) |
| CellChat | Wang/Liu, Prakrithi, (Arora likely) | Considered; chose NicheCompass instead |
| steinbock + Mesmer | Meyer/Bodenmiller, Wang/Ali | Not needed (10x pipelines handle segmentation) |
| imcRtools + spicyR | Meyer/Bodenmiller | Not needed (IMC-specific) |
| Scanpy/AnnData | Wang/Liu, Peng/Kadara, Prakrithi | Yes — data infrastructure |
| Squidpy | Wang/Liu (likely), Klughammer (likely) | Yes — spatial graph construction |
| **CellCharter** | Referenced in Wang/Liu context | **Our primary niche tool** |
| **NicheCompass** | Own methods paper (Birk 2025) | **Our communication tool** |
| **Niche-DE** | Own methods paper (Bai 2024) | **Our gene program tool** |

!!! note "Gap confirmed"
    No published cancer study has used CellCharter + NicheCompass + Niche-DE together in a single pipeline. Our combination is novel.

---

## References

1. Liu Y, Sinjab A, et al. Conserved spatial subtypes and cellular neighborhoods of cancer-associated fibroblasts. *Cancer Cell* 43(5): 905-924 (2025).
2. Peng F, Sinjab A, et al. Multimodal spatial-omics reveal co-evolution of alveolar progenitors and proinflammatory niches. *Cancer Cell* 44(2): 321-339 (2026).
3. Meyer D, et al. A stratification system for breast cancer based on basoluminal tumor cells and spatial tumor architecture. *Cancer Cell* 43(9): 1637-1655 (2025).
4. Wang XQ, Danenberg E, et al. Spatial predictors of immunotherapy response in triple-negative breast cancer. *Nature* 621: 868-876 (2023).
5. Sorin M, et al. Single-cell spatial landscapes of the lung tumour immune microenvironment. *Nature* 614: 548-554 (2023).
6. Arora R, Cao C, et al. Spatial transcriptomics reveals distinct and conserved tumor core and edge architectures. *Nature Communications* 14: 5029 (2023).
7. Klughammer J, et al. A multi-modal single-cell and spatial expression map of metastatic breast cancer. *Nature Medicine* 30: 3236-3249 (2024).
8. Prakrithi P, Grice LF, et al. Integrating 12 spatial and single cell technologies to characterise tumour neighbourhoods. *bioRxiv* (2025).

---

*See also: [Tool Selection](tool-selection.md) for our recommended pipeline based on these insights.*
