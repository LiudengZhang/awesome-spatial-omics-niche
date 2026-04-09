# H&E Integration: Morphological Niches

H&E staining is the most widely available tissue imaging modality in clinical pathology. Every Xenium run produces a paired H&E image, meaning our hackathon datasets D1 and D4 already have morphological data waiting to be used. This page explores how H&E-derived features could extend the pipeline from two modalities (RNA vs Protein) to three (RNA vs Protein vs Morphology).

!!! info "Connection to the organizer"
    Linghua Wang's group developed **TESLA** (Cell Systems, 2023), **METI** (Nature Communications, 2024), and **SpaGCN** (Nature Methods, 2021) — all of which integrate H&E morphology with spatial gene expression. Adding a morphological niche axis to the pipeline directly aligns with the organizer's research focus.

---

## Pathology Foundation Models

Recent pathology foundation models extract rich embeddings from H&E image patches. These embeddings encode tissue architecture, cell morphology, and spatial organization — features that are invisible in transcriptomic data alone.

| Model | What It Does | Training Scale | Spatial-ST Integration? | GitHub / Access |
|-------|-------------|---------------|------------------------|-----------------|
| **UNI / UNI2** | Patch-level embeddings via DINOv2 | 200M+ patches, 350K WSIs | Used in HEST benchmark for gene prediction | [mahmoodlab/UNI](https://github.com/mahmoodlab/UNI) |
| **CONCH** | Vision-language model (image + text) | Pathology images + reports | Used in HEST benchmark | [mahmoodlab/CONCH](https://github.com/mahmoodlab/CONCH) |
| **Virchow / Virchow2** | ViT-H patch embeddings | 3M+ WSIs (Paige) | Benchmarked in HEST | Gated access via Paige |
| **Phikon-v2** | ViT-L via DINOv2, fully open | 460M tiles, 30+ cancer sites | Benchmarked in HEST | [owkin/HistoSSLscaling](https://github.com/owkin/HistoSSLscaling) |
| **CTransPath** | Contrastive learning on patches | 15M patches from TCGA + PAIP | Widely used baseline | [Xiyue-Wang/TransPath](https://github.com/Xiyue-Wang/TransPath) |
| **CHIEF** | Tile + WSI-level cancer evaluation | 60K WSIs, 19 anatomical sites | Cancer diagnosis, not ST | [hms-dbmi/CHIEF](https://github.com/hms-dbmi/CHIEF) |
| **Prov-GigaPath** | Whole-slide foundation model | 1.3B params, 170K WSIs | Benchmarked in HEST | [prov-gigapath/prov-gigapath](https://github.com/prov-gigapath/prov-gigapath) |

!!! note "HEST-1k benchmark"
    The [HEST-1k dataset](https://github.com/mahmoodlab/HEST) (NeurIPS 2024 Spotlight) provides 1,108 paired H&E + spatial transcriptomics samples and benchmarks 11 foundation models on gene expression prediction from morphology. This is the standard reference for comparing patch embedding quality.

---

## Tools That Bridge H&E and Spatial Transcriptomics for Niche Analysis

These tools go beyond generic patch embeddings — they specifically integrate histology with spatial molecular data to define tissue regions, niches, or cell states.

| Tool | What It Does | Works with Xenium? | Citation |
|------|-------------|-------------------|---------|
| **SpatialFusion** | Multimodal foundation model: aligns H&E + transcriptomics + pathway activity into joint embeddings, then clusters neighborhoods into spatial niches via graph convolution | Designed for single-cell ST; should work | bioRxiv 2026 |
| **H&Enium** | Contrastive alignment of pathology FM embeddings with Xenium transcriptomics; improves cell typing and gene prediction from H&E alone | **Yes** — built on Xenium data | bioRxiv 2025 |
| **STORM** | Multimodal FM integrating H&E patches + ST profiles; spatial domain discovery and gene prediction | **Yes** — tested on Xenium, Visium HD, CosMx | arXiv 2026 |
| **OmiCLIP** | CLIP-style alignment of H&E patches with transcriptomes across 32 organs | Visium-trained; untested on Xenium | Nature Methods 2025 |
| **TESLA** | Super-resolution tissue annotation using H&E to subdivide Visium spots | No — Visium spots only | Cell Systems 2023 |
| **METI** | TME interaction mapping from H&E + spatial expression | No — Visium spots only | Nature Communications 2024 |
| **GigaTIME** | Predicts multiplex IF from H&E alone (virtual staining) | H&E only — no ST needed | Cell 2025 |
| **SpaGCN** | Graph CNN integrating expression + location + histology for spatial domains | Visium only | Nature Methods 2021 |

---

## Morphological Niches: The Concept

The idea is straightforward:

1. **Extract patch embeddings** from the H&E image using a foundation model (UNI, Phikon, etc.) at each cell or spot location.
2. **Cluster the embeddings** to define "morphological niches" — tissue regions with similar histological appearance.
3. **Compare morphological niches with molecular niches** (from RNA or protein) to ask: do tissue regions that look similar also share molecular programs?

This creates a three-way comparison:

```
Molecular niches (RNA)    ←→    Morphological niches (H&E)
        ↕                                ↕
Molecular niches (Protein) ←→   Morphological niches (H&E)
```

!!! tip "Why this matters"
    Concordance between morphological and molecular niches would validate that H&E — the cheapest, most widely available clinical stain — captures biologically meaningful niche structure. Discordance would reveal which niche features are invisible to the microscope and require molecular measurement.

**Practical approach for the hackathon:**

- Use a publicly available FM (Phikon-v2 or UNI) to extract patch embeddings at each Xenium cell coordinate from the paired H&E image.
- Cluster embeddings with the same approach used for molecular niches (e.g., Gaussian mixture model via CellCharter, or simple Leiden clustering).
- Quantify overlap with CellCharter-derived RNA niches and protein niches using ARI/NMI.

---

## Extending the Pipeline: RNA vs Protein vs Morphology

The current pipeline (see [Tool Selection](tool-selection.md)) compares RNA niches and protein niches in Step 4. Adding morphological niches creates a three-way concordance analysis:

| Modality | Source | Niche Method | Datasets |
|----------|--------|-------------|----------|
| RNA | Xenium transcripts | CellCharter | D1, D4 |
| Protein | Protein panel | CellCharter | D1 |
| Morphology | Paired H&E image | FM embeddings + clustering | D1, D4 |

**Three questions become testable:**

1. **RNA vs Protein** (already in pipeline) — do transcriptomic and proteomic niches agree?
2. **RNA vs Morphology** — do molecular programs align with tissue architecture?
3. **Protein vs Morphology** — do surface markers correlate with histological features?

!!! warning "Scope consideration"
    This is an extension, not a replacement. The two-day hackathon timeline is tight. H&E integration could be a stretch goal or a compelling figure if time permits. The core pipeline (Steps 1-4) should be completed first.

---

## Dataset Availability

| Dataset | Xenium | Protein | H&E Paired? | Notes |
|---------|--------|---------|-------------|-------|
| **D1** | Yes | Yes | **Yes** — Xenium produces paired H&E | Three-way comparison possible (RNA + Protein + Morphology) |
| **D4** | Yes | No | **Yes** — Xenium produces paired H&E | Two-way comparison (RNA + Morphology) |
| **V1** | No (CosMx) | No (CODEX) | Unknown | CosMx and CODEX may or may not have paired H&E |

D1 is the strongest candidate for morphological niche analysis because it enables the full three-way comparison.

---

*See also: [Morphology-Based Methods](../methods/morphology-based.md) | [TESLA Deep Read](../deep-reads/tesla.md) | [Tool Selection](tool-selection.md)*
