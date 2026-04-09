# Tool Selection for the Hackathon

This page documents tool selection for the Colin Stroud Hackathon, organized by analysis task. For each task, we compare candidate tools based on recent benchmarks and recommend one for our pipeline using **D1 + D4 (discovery) and V1 (validation)**.

---

## Niche Identification

**Question:** What are the spatial neighborhoods in the tissue?

!!! note "Key insight"
    Domain detection ≠ niche identification. Most spatial domain methods (GraphST, STAGATE, BayesSpace) fail at true niche identification in default configurations (bioRxiv 2026 niche benchmark). The tools below are designed for niches specifically.

| Tool | Niche Definition | Strengths | Limitations |
|------|-----------------|-----------|-------------|
| **[CellCharter](../deep-reads/cellcharter.md)** (Nature Genetics, 2024) | Composition-based: cell-type proportions in spatial neighborhoods | Cross-platform stable; strong for continuous tissues; intuitive interpretation | Requires good cell typing first; misses expression-level niches |
| **[BANKSY](../deep-reads/banksy.md)** (Nature Genetics, 2024) | Expression + spatial lag: gene expression weighted by neighbor expression | Scales to large datasets; fast on CosMx; top DLPFC benchmark performer | DLPFC tests domains, not niches; less biologically interpretable |
| **[NicheCompass](../deep-reads/nichecompass.md)** (Nature Genetics, 2025) | Communication-based: ligand-receptor signals across spatial graph | Finds niches others miss (lymphoid structures, tumor-stroma boundaries); cross-sample atlas | Heavier computation; newer, less community validation |
| **[Nicheformer](../deep-reads/nicheformer.md)** (Nature Methods, 2025) | Foundation model: pre-trained on spatial and dissociated data | Transfer learning; flexible downstream tasks; lightweight (49M params) | Black box; needs fine-tuning per tissue type |

**Benchmark references:**

- Chen et al., iMeta 2025 — 14 methods across ~600 datasets, 10 technologies, 8 organs
- Yuan et al., Nature Methods 2024 — 13 spatial clustering methods on 34 datasets
- Kang & Zhang, Nucleic Acids Research 2025 — 19 methods across 30 datasets
- Li et al., Genome Biology 2024 — 16 clustering + 5 alignment + 5 integration methods

**Our pick: [CellCharter](../deep-reads/cellcharter.md)** — proven on Xenium and protein panels (matching D1 + D4), cross-platform stable for V1 validation, and composition-based niches align with the hackathon's goal of characterizing functional tissue neighborhoods.

---

## Niche Communication

**Question:** What signaling drives each niche?

!!! note "Key insight"
    Non-spatial cell-cell communication methods detect biologically implausible tissue-spanning signals. Spatial-aware methods (COMMOT, NicheCompass) far outperform them by constraining interactions to local neighborhoods.

| Tool | Approach | Strengths | Limitations |
|------|----------|-----------|-------------|
| **[NicheCompass](../deep-reads/nichecompass.md)** (Nature Genetics, 2025) | Variational model with L-R prior on spatial graph | Niche + communication jointly modeled; scalable to millions of cells; best in internal benchmark | Heavier than CCC-only tools; complex setup |
| **[COMMOT](../deep-reads/commot.md)** (Nature Methods, 2023) | Optimal transport with spatial distance constraint | Principled spatial CCC; rejects implausible long-range interactions; well-validated | No niche output (CCC only); L-R database dependent |
| **CellNEST** (Nature Methods, 2025) | Attention mechanism for relay signaling | Captures A→B→C relay chains; best CCL19-CCR7 localization in benchmark | Very new (2025); limited validation datasets |
| **CellChat v2** (Nature Communications, 2024) | Statistical L-R inference + spatial mode | Largest L-R database; most widely used; rich visualization | Originally non-spatial; spatial mode is an add-on |

**Benchmark references:**

- CellNEST, Nature Methods 2025 — 8 CCC tools compared on spatial localization
- COMMOT, Nature Methods 2023 — showed non-spatial CCC detects implausible signals
- Briefings in Bioinformatics 2025 — comprehensive review of ~100 CCC tools

**Our pick: [NicheCompass](../deep-reads/nichecompass.md)** — jointly models niches and communication, best benchmark performance, and enables cross-sample atlas construction across D1 + D4.

---

## Niche-Aware Downstream Analysis

**Question:** Which genes are expressed because of niche context, not cell type alone?

!!! warning "False positive alert"
    Standard differential expression methods have severely inflated type I error rates (86-95%) when applied to spatial data because they ignore spatial autocorrelation (smiDE, Genome Biology 2025). Spatial-aware DE methods are essential.

| Tool | What It Does | Strengths | Limitations |
|------|-------------|-----------|-------------|
| **[Niche-DE](../deep-reads/niche-de.md)** (Genome Biology, 2024) | Tests whether gene expression depends on niche context | Robust type I error control; niche-LR extension identifies ligand-receptor mechanisms | Clusters with non-spatial methods in concordance analysis |
| **smiDE** (Genome Biology, 2025) | Spatial correlation-aware differential expression | Proper modeling of spatial autocorrelation; lightweight memory footprint | Newer; less tested on niche-specific questions |

**Benchmark reference:**

- smiDE, Genome Biology 2025 — spatial DE benchmark showing non-spatial methods have 86-95% false positive rates

**Our pick: [Niche-DE](../deep-reads/niche-de.md)** — directly answers the hackathon question (what genes are niche-context-dependent?), with robust statistical control and the niche-LR extension for mechanistic interpretation.

---

## Morphology Mapping

**Question:** Do molecular niches correspond to visible tissue architecture?

!!! tip "Organizer connection"
    TESLA and METI are developed by Linghua Wang's group — the hackathon organizer. These tools directly reflect the hackathon's philosophy of integrating computational methods with tissue morphology.

| Tool | What It Does | Strengths | Limitations |
|------|-------------|-----------|-------------|
| **[TESLA](../deep-reads/tesla.md)** (Cell Systems, 2023) | Pixel-level niche annotation from H&E + expression | Detects tertiary lymphoid structures that expression-only methods miss; super-resolution | Requires paired H&E (available in D1 + D4) |
| **METI** (Nature Communications, 2024) | TME profiling integrating morphology + transcriptomics | Compensates for low-quality modality; knowledge-driven gene signatures | Curated signatures may miss novel programs |

No independent multi-method benchmark exists for morphology-integrated spatial methods — this is a gap in the field.

**Our pick: TESLA + METI** — leverages the H&E data available in both D1 and D4, and connects our molecular niche findings to tissue architecture visible in clinical stains.

---

## Recommended Pipeline

```
D1 + D4 (Xenium + H&E)                    V1 (CosMx + CODEX)
         │                                          │
    ┌────┴────┐                                     │
    │ Step 1  │  CellCharter ──────────────────► CellCharter
    │ Niche ID│  (composition-based niches)      (cross-platform
    └────┬────┘                                  validation)
         │
    ┌────┴────┐
    │ Step 2  │  NicheCompass
    │ Comms   │  (L-R signaling per niche)
    └────┬────┘
         │
    ┌────┴────┐
    │ Step 3  │  Niche-DE
    │ Programs│  (context-dependent genes)
    └────┬────┘
         │
    ┌────┴────┐
    │ Step 4  │  TESLA / METI
    │ Morpho  │  (H&E ↔ niche mapping)
    └─────────┘
```

| Step | Task | Tool | Why This Tool |
|------|------|------|---------------|
| 1 | Niche identification | CellCharter | Proven on Xenium + protein panels; cross-platform stable for V1 validation |
| 2 | Niche communication | NicheCompass | Joint niche + L-R model; best benchmark performance; cross-sample atlas |
| 3 | Niche gene programs | Niche-DE | Context-dependent DE with spatial type I error control; niche-LR for mechanisms |
| 4 | Morphology mapping | TESLA / METI | H&E integration at pixel-level; detects structures expression-only methods miss |
| 5 | Cross-platform validation | CellCharter on V1 | Re-run niche ID on CosMx + CODEX to test if discovered niches replicate |

**Discovery on D1 + D4 (Xenium + H&E) → Validation on V1 (CosMx + CODEX)**
