# CODEX Niche Analysis

This page documents how spatial niche analysis is performed on protein-based spatial data (CODEX, PhenoCycler, MIBI-TOF, IMC), based on a review of 10 representative papers. Since CODEX measures proteins (~30-60 markers) rather than transcripts, the niche analysis workflow differs fundamentally from RNA-based platforms.

---

## The CODEX Workflow

The standard CODEX niche analysis pipeline has 5 steps:

1. **Cell Segmentation** — identify individual cells from multiplexed images
2. **Cell Typing** — classify cells by protein marker expression
3. **Niche Identification** — define spatial neighborhoods by cell-type composition
4. **Niche Characterization** — functional analysis via marker co-expression and spatial enrichment
5. **Clinical Correlation** — link niches to patient outcomes or treatment response

!!! warning "No cell-cell communication step"
    Unlike RNA-based workflows, CODEX analysis does **not** include a ligand-receptor communication step. CCC tools (NicheCompass, CellChat, COMMOT) rely on gene-symbol L-R databases that do not exist for protein panels. Functional characterization on protein data uses marker intensity analysis, cell-type interaction frequencies, and spatial enrichment instead.

---

## Tools at Each Step

| Step | Common Tools | Notes |
|------|-------------|-------|
| **Segmentation** | Mesmer/DeepCell, CellProfiler, CODEX toolkit, Steinbock | Deep learning pipelines (Mesmer) now standard; older papers used CellProfiler |
| **Cell Typing** | FlowSOM, X-shift/VorteX, Leiden (scanpy) | FlowSOM and X-shift are cytometry-native; Leiden increasingly used |
| **Niche ID** | k-NN frequency vectors + K-means (Schurch-style), CellCharter, CytoCommunity, ColonyMap | k=10 KNN with cell-type frequency vectors is the dominant approach |
| **Characterization** | Marker co-expression, spatial enrichment, cell-cell interaction scoring, MISTy | No L-R tools; MISTy is the only computational niche characterization tool verified on protein data |
| **Clinical Correlation** | Kaplan-Meier, Cox regression, linear mixed models | Standard survival analysis |

---

## The Dominant Niche Method: k-NN Cellular Neighborhoods

Most CODEX niche papers (6/10 reviewed) use a variant of the approach introduced by Schurch et al. (2020):

1. Build a k-nearest-neighbor graph (typically k=10) from cell coordinates
2. For each cell, compute a frequency vector: the proportion of each cell type among its k neighbors
3. Cluster these frequency vectors (K-means or Leiden) into cellular neighborhoods (CNs)

This is a **composition-based** niche definition — niches are defined by the mix of cell types present, not by gene expression or signaling.

**Computational tools that automate this:**

| Tool | What It Adds Beyond k-NN + K-means | Paper-Tested On |
|------|-------------------------------------|-----------------|
| **CellCharter** | Multi-scale GNN embeddings + GMM with automatic K selection | CODEX (CRC), IMC (breast), CosMx, MERSCOPE, Visium |
| **CytoCommunity** | Supervised GNN that uses condition labels to find disease-relevant niches | CODEX (CRC), MIBI-TOF (TNBC) |
| **ColonyMap** | Detects spatially assembled "colonies" (densely co-localized communities) | CODEX/PhenoCycler (SCLC) |

!!! tip "CellCharter is the recommended starting point"
    CellCharter is the most versatile option: it works on both RNA and protein platforms, handles multi-sample integration, and automatically selects the number of niches. It is also the tool used by other hackathon team members on Xenium, enabling direct cross-modality comparison.

---

## Literature Review: 10 Representative Papers

### Summary Table

| Paper | Platform | Cancer | Markers | Niche Method | Clinical Linkage | Journal |
|-------|----------|--------|---------|-------------|-----------------|---------|
| Goltsev et al. 2018 | CODEX | Mouse spleen | 30 | i-niche (Delaunay + K-means) | No (mouse) | Cell |
| Schurch et al. 2020 | CODEX | Colorectal | 56 | k=10 KNN -> 9 CNs | Yes (survival) | Cell |
| Risom et al. 2022 | MIBI-TOF | Breast (DCIS->IBC) | 37 | TME state mapping | Yes (recurrence) | Cell |
| Danenberg et al. 2022 | IMC | Breast (n=693) | 37 | Graph community detection | Yes (survival) | Nature Genetics |
| Hoch et al. 2022 | IMC | Melanoma | ~40 | Chemokine patch + TLS | Yes (immunotherapy) | Science Immunology |
| Wang et al. 2023 | IMC | TNBC (n=243) | 43 | Cell interaction scoring | Yes (immunotherapy) | Nature |
| CellCharter 2024 | CODEX/IMC | Lung, CRC | 30+ | GNN + GMM | Yes (prognosis) | Nature Genetics |
| CytoCommunity 2024 | CODEX + MIBI-TOF | CRC, TNBC | 56 | GNN graph partitioning | Yes (risk) | Nature Methods |
| Matusiak et al. 2024 | CODEX | Colon + breast | 36 | k=10 KNN -> 30 clusters | Yes (survival) | Cancer Discovery |
| Chen et al. 2025 | CODEX/PhenoCycler | SCLC | 35 | ColonyMap | Yes (immunotherapy) | Cancer Cell |

---

### Paper Details

#### Goltsev et al. 2018 — The CODEX Origin Paper

**Citation:** Goltsev Y, Samusik N, Kennedy-Darling J, et al. *Deep profiling of mouse splenic architecture with CODEX multiplexed imaging.* Cell 174: 968-981 (2018).

**Platform:** CODEX, 30 markers on mouse spleen (not cancer — included because it introduced the i-niche concept and computational framework that all subsequent CODEX papers build on).

**Workflow:**
- Segmentation: CellProfiler + custom pipeline
- Cell typing: X-shift/VorteX -> 27 phenotypic clusters
- Niche ID: Delaunay triangulation -> i-niche frequency vectors -> K-means (K=100)
- Characterization: Mapping i-niches to anatomical compartments; healthy vs. lupus comparison

**Key finding:** A cell's spatial neighborhood strongly influences its protein expression levels, independent of cell identity. This established that spatial context is a determinant of cell state.

---

#### Schurch et al. 2020 — Cellular Neighborhoods in CRC

**Citation:** Schurch CM, Bhate SS, Barlow GL, et al. *Coordinated cellular neighborhoods orchestrate antitumoral immunity at the colorectal cancer invasive front.* Cell 182: 1341-1359 (2020).

**Platform:** CODEX, 56 markers on colorectal cancer TMAs (140 regions, 35 patients).

**Workflow:**
- Segmentation: CODEX toolkit
- Cell typing: X-shift/VorteX -> 26 cell types
- Niche ID: k=10 KNN -> cell-type frequency vectors -> K-means -> 9 cellular neighborhoods
- Characterization: Tucker tensor decomposition, CN coupling/fragmentation metrics
- Clinical: Cox regression, Kaplan-Meier survival

**Key finding:** Nine conserved cellular neighborhoods identified. Two patient archetypes: Crohn's-like reaction (TLS-rich, better prognosis) vs. diffuse inflammatory infiltration (no TLS, worse). PD-1+CD4+ T cells in granulocyte neighborhoods predicted survival in high-risk patients.

!!! info "The origin of CODEX niche analysis"
    This paper established the k=10 KNN -> frequency vector -> K-means pipeline that most subsequent CODEX papers follow. It remains the most cited spatial niche paper in the protein spatial field.

---

#### Risom et al. 2022 — DCIS-to-Invasive Breast Cancer Transition

**Citation:** Risom T, Glass DR, Averbukh I, et al. *Transition to invasive breast cancer is associated with progressive changes in the structure and composition of tumor stroma.* Cell 185: 299-310 (2022).

**Platform:** MIBI-TOF, 37 markers on breast cancer surgical resections (79 cases).

**Workflow:**
- Segmentation: Mesmer/DeepCell
- Cell typing: FlowSOM -> 16 cell classes
- Niche ID: Four TME states via spatial compartment mapping (myoepithelial, stromal, immune)
- Characterization: Myoepithelial disruption quantification, stromal activation markers

**Key finding:** Myoepithelial disruption was paradoxically more advanced in DCIS that did *not* progress to invasive cancer, suggesting protective remodeling. Spatial proteomic signature predicted invasive recurrence from DCIS.

---

#### Danenberg et al. 2022 — Breast Cancer TME Structures (METABRIC)

**Citation:** Danenberg E, Bardwell H, Zanotelli VRT, et al. *Breast tumor microenvironment structures are associated with genomic features and clinical outcome.* Nature Genetics 54: 660-669 (2022).

**Platform:** IMC, 37 markers on 693 breast tumors from the METABRIC cohort.

**Workflow:**
- Segmentation: ilastik + CellProfiler
- Cell typing: FlowSOM -> epithelial, stromal, leukocyte lineages
- Niche ID: Graph community detection -> 10 recurrent multicellular structures
- Characterization: Enrichment for proliferative, regulatory, and exhausted immune markers

**Key finding:** Ten recurrent TME structures identified. A "suppressed expansion" structure (FoxP3+ Tregs + PD-1+ exhausted T cells + proliferating cells) predicted poor outcome in ER+ breast cancer, independently of genomic classifiers. Spatial architecture adds prognostic signal beyond PAM50 subtypes.

---

#### Hoch et al. 2022 — Chemokine Niches in Melanoma

**Citation:** Hoch T, Schulz D, Eling N, et al. *Multiplexed imaging mass cytometry of the chemokine milieus in melanoma characterizes features of the response to immunotherapy.* Science Immunology 7: eabk1692 (2022).

**Platform:** IMC with protein + chemokine RNA co-detection (~40 markers), 69 metastatic melanoma patients.

**Workflow:**
- Segmentation: Steinbock
- Cell typing: Protein marker clustering -> CD8+ T cells, macrophages, B cells, tumor cells, etc.
- Niche ID: Chemokine "patch" spatial enrichment + TLS niche co-localization
- Characterization: RNA-protein co-detection attributed chemokine sources to specific cell types within niches

**Key finding:** TLS-like chemokine niches enriched for TCF7+ naive-like T cells predicted checkpoint blockade response. CXCL13+ exhausted T cells co-localized with CXCL9/10 patches in infiltrated tumors.

---

#### Wang et al. 2023 — Spatial Predictors of Immunotherapy Response in TNBC

**Citation:** Wang XQ, Danenberg E, Huang CS, et al. *Spatial predictors of immunotherapy response in triple-negative breast cancer.* Nature 621: 868-876 (2023).

**Platform:** IMC, 43 markers on TNBC (243 patients from a randomized neoadjuvant ICB trial, with baseline + on-treatment + post-treatment biopsies).

**Workflow:**
- Segmentation: Semi-supervised epithelial/TME separation
- Cell typing: Expression clustering -> 17 epithelial + 20 TME phenotypes
- Niche ID: Cell density + phenotype-specific interaction scoring
- Characterization: Longitudinal spatial remodeling across 3 biopsy timepoints

**Key finding:** Proliferating CD8+TCF1+ T cells and MHCII+ cancer cells were the dominant baseline predictors of ICB response. Combining baseline and on-treatment spatial features improved response prediction. This is the most clinically direct protein spatial niche paper — it links spatial organization to treatment response in a randomized trial.

---

#### CellCharter 2024 — Multi-Platform Niche Identification

**Citation:** Varrone M, Tavernari D, Santamaria-Martinez A, Walsh LA, Ciriello G. *CellCharter reveals spatial cell niches associated with tissue remodeling and cell plasticity.* Nature Genetics 56: 74-84 (2024).

**Platform:** Paper-tested on CODEX (CRC), IMC (breast cancer), CosMx, MERSCOPE, Visium.

**Workflow:**
- Niche ID: GNN embeddings of spatial neighborhoods -> GMM clustering with stability-based model selection
- Multi-scale: captures niches at different spatial resolutions simultaneously
- Cross-platform: same method on both RNA and protein data

**Key finding (CODEX/IMC):** Identified tumor-associated neutrophil niches co-localizing with hypoxia markers, associated with poor prognosis in lung cancer.

---

#### CytoCommunity 2024 — Supervised Niche Discovery

**Citation:** Hu Y, Rong J, Xu Y, et al. *Unsupervised and supervised discovery of tissue cellular neighborhoods from cell phenotypes.* Nature Methods 21: 267-278 (2024).

**Platform:** Paper-tested on CODEX (CRC, 56 markers) and MIBI-TOF (TNBC).

**Workflow:**
- Niche ID: GNN-based graph partitioning; supervised mode uses sample-level condition labels to discover disease-relevant tissue cellular neighborhoods (TCNs)
- Characterization: Hypergeometric cell-type enrichment per TCN

**Key finding:** In high-risk CRC, identified granulocyte-enriched and CAF-enriched TCNs absent in low-risk tumors. TCN composition correlated with risk stratification and survival.

---

#### Matusiak et al. 2024 — Macrophage Niches in Colon and Breast Cancer

**Citation:** Matusiak M, Hickey JW, van IJzendoorn DGP, et al. *Spatially segregated macrophage populations predict distinct outcomes in colon cancer.* Cancer Discovery 14: 1418-1439 (2024).

**Platform:** CODEX, 36 markers on colon (32 cases) and breast (36 cases) cancer TMAs.

**Workflow:**
- Segmentation: CODEX toolkit
- Cell typing: Leiden clustering (scanpy) + manual annotation
- Niche ID: k=10 KNN -> 30 neighborhood clusters (Schurch-style)
- Characterization: Linear mixed-effects models; IHC and IF validation

**Key finding:** Five spatially distinct macrophage populations occupied conserved niche architectures: FOLR2+ in plasma cell niches, LYVE1+ in perivascular niches, SPP1+ in hypoxic zones, NLRP3+ with neutrophils. IL4I1+ macrophages predicted good outcomes; SPP1+ predicted poor outcomes in colon cancer.

---

#### Chen et al. 2025 — Immune Colony Niches in SCLC

**Citation:** Chen H, Deng C, Gao J, et al. *Integrative spatial analysis reveals tumor heterogeneity and immune colony niche related to clinical outcomes in small cell lung cancer.* Cancer Cell 43: 519-536 (2025).

**Platform:** CODEX/PhenoCycler, 35 markers on SCLC (165 patients, 267 images, >9.3M cells).

**Workflow:**
- Segmentation: Deep learning pipeline
- Cell typing: Unsupervised clustering -> tumor subtypes, macrophages, T cells, NK/NKT, B cells, stroma
- Niche ID: ColonyMap — novel algorithm detecting densely co-localized cellular communities
- Characterization: Multi-omics integration (genomics + CODEX)

**Key finding:** An antitumoral immune colony niche (MT2: macrophages + CD8+ T cells + NKT cells) predicted immunotherapy response and superior survival. ColonyMap-derived spatial features outperformed standard cell-type abundance metrics.

---

## Patterns Across Papers

### What's consistent
- **Composition-based niche ID dominates**: 8/10 papers define niches by cell-type composition of neighborhoods
- **k=10 is standard**: Most papers use k=10 nearest neighbors for neighborhood construction
- **Clinical linkage is expected**: 9/10 papers connect niches to patient outcomes or treatment response
- **Segmentation is solved**: Deep learning (Mesmer) or platform-native tools; not a bottleneck
- **No CCC tools used**: Zero papers apply ligand-receptor communication analysis to protein data

### What varies
- **Niche granularity**: From 4 TME states (Risom) to 100 i-niches (Goltsev) — no consensus on optimal resolution
- **Cell typing method**: X-shift (older), FlowSOM (cytometry standard), Leiden (newer, scanpy-based)
- **Characterization approach**: Ranges from simple enrichment tests to tensor decomposition to longitudinal spatial remodeling

### What's missing
- **No standardized functional characterization**: Unlike RNA platforms (where NicheCompass/Niche-DE provide structured analysis), protein niche characterization is ad hoc
- **No cross-modality validation**: Only CellCharter has been applied to both RNA and protein data from the same tissue
- **Limited panel overlap**: Each study uses a different antibody panel, making cross-study comparison difficult

---

## Recommended CODEX Workflow for the Hackathon

Based on the literature review, the recommended workflow for CODEX data:

| Step | Tool | Rationale |
|------|------|-----------|
| **Segmentation** | Mesmer/DeepCell or platform-provided | Standard; deep learning preferred |
| **Cell Typing** | Leiden clustering (scanpy) | Consistent with team's RNA workflow; scanpy ecosystem |
| **Niche ID** | CellCharter | Paper-tested on CODEX; same tool as RNA team -> enables cross-modality comparison |
| **Characterization** | Marker intensity analysis + spatial enrichment + MISTy | MISTy is the only computational tool verified on protein spatial data for niche characterization |
| **Clinical Correlation** | Kaplan-Meier + Cox regression | Standard |

!!! tip "Cross-modality concordance"
    Using CellCharter on both CODEX and Xenium (from the same tissue in D1) enables the key hackathon question: **do RNA niches and protein niches agree?** This is directly modeled on CellCharter's own paper, which demonstrated cross-platform niche analysis.

---

*See also: [Data Compatibility](data-compatibility.md) | [Tool Selection](tool-selection.md) | [Literature Review](literature-review.md) | [CellCharter Deep Read](../deep-reads/cellcharter.md)*
