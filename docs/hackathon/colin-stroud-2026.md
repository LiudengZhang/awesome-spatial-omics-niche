# Colin Stroud Hackathon 2026

## Overview

- **Event:** Colin Stroud Hackathon
- **Dates:** April 30 - May 1, 2026
- **Location:** MD Anderson Cancer Center
- **Organizer:** Linghua Wang
- **Theme:** "Decoding Functional Niches in Cancer by Leveraging Spatial Omics and AI"

## The Organizer

Linghua Wang is a professor at MD Anderson Cancer Center whose research focuses on integrating computational pathology with spatial transcriptomics to characterize tumor microenvironments. Her group developed:

- **TESLA** (Cell Systems, 2023) — H&E-guided super-resolution annotation for spatial transcriptomics.
- **METI** (Nature Communications, 2024) — Tumor microenvironment interaction mapping combining morphology and spatial gene expression.
- **SpaGCN** (Nature Methods, 2021) — Graph convolutional network for spatial domain identification integrating expression, spatial location, and histology.

All three methods share a common thread: integrating H&E morphology with molecular spatial data. This reflects Wang's core insight that tissue architecture visible in routine clinical stains encodes information about the molecular microenvironment.

## Theme Breakdown

"Decoding **Functional Niches** in Cancer by Leveraging **Spatial Omics** and **AI**"

- **Functional Niches:** Not just identifying niches, but understanding what they do — how niches influence cell behavior, treatment response, and patient outcome. See [Functional Niche](../concepts/functional-niche.md).
- **Spatial Omics:** Technologies that measure molecular features (RNA, protein) with spatial resolution in tissue.
- **AI:** Computational methods from deep learning, foundation models, and statistical learning.

The theme directly aligns with the core tension in the niche field: we have many methods to identify niches (correlative) but few to understand their function (causal). The hackathon is positioned at the frontier between niche identification and functional niche characterization.

## Relevant Background

### Key Papers to Read

1. **Schurch et al. (Cell, 2020)** — The origin of computational cellular neighborhoods. [Deep read](../deep-reads/schurch-2020.md).
2. **TESLA (Cell Systems, 2023)** — Wang's method for H&E-guided spatial annotation. [Deep read](../deep-reads/tesla.md).
3. **METI (Nature Communications, 2024)** — Wang's method for TME interaction mapping.
4. **Niche-DE (Genome Biology, 2024)** — Testing whether gene expression depends on niche context. [Deep read](../deep-reads/niche-de.md).
5. **NicheCompass (Nature Genetics, 2025)** — Communication-aware niche atlas. [Deep read](../deep-reads/nichecompass.md).

### Key Concepts

- [What Is a Niche?](../concepts/what-is-a-niche.md) — The definition taxonomy.
- [Niche vs Domain](../concepts/niche-vs-domain.md) — Why this distinction matters.
- [Functional Niche](../concepts/functional-niche.md) — The causation frontier.
- [Definition Shapes Discovery](../synthesis/definition-shapes-discovery.md) — How your niche definition determines your findings.

### Relevant Datasets

- **CRC CODEX** — The standard niche dataset (Schurch et al., 2020).
- **TISCH2** — Curated tumor microenvironment atlas with 190 datasets covering 50+ cancer types.
- **CZ CELLxGENE Census** — 100M+ cells with standardized APIs.
- Any cancer spatial transcriptomics dataset with paired H&E (relevant for Wang's morphology-integration approach).
