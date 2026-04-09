# Tool Selection for the Hackathon

This page documents tool selection for the Colin Stroud Hackathon, organized by analysis task. For each task, we compare candidate tools based on recent benchmarks, **verified against source papers and codebases**, and recommend one for our pipeline using **D1 + D4 (discovery) and V1 (validation)**.

!!! info "Verification methodology"
    Every compatibility claim below was checked against the tool's published paper, GitHub repo, and source code. We use: :white_check_mark: = paper-tested, :warning: = listed/likely but untested, :x: = incompatible.

---

## Step 1: Niche Identification

**Question:** What are the spatial neighborhoods in the tissue?

!!! note "Key insight"
    Domain detection ≠ niche identification. Most spatial domain methods (GraphST, STAGATE, BayesSpace) fail at true niche identification in default configurations (bioRxiv 2026 niche benchmark). The tools below are designed for niches specifically.

| Tool | Niche Definition | Strengths | Limitations |
|------|-----------------|-----------|-------------|
| **[CellCharter](../deep-reads/cellcharter.md)** (Nature Genetics, 2024) | Composition-based: cell-type proportions in spatial neighborhoods | :white_check_mark: Paper-tested on CODEX, CosMx, MERSCOPE, Visium, IMC; works on both RNA + protein | Xenium listed but not paper-tested; Visium HD untested |
| **[BANKSY](../deep-reads/banksy.md)** (Nature Genetics, 2024) | Expression + spatial lag: gene expression weighted by neighbor expression | Scales to large datasets; fast on CosMx; top DLPFC benchmark performer | DLPFC tests domains, not niches; less biologically interpretable |
| **[NicheCompass](../deep-reads/nichecompass.md)** (Nature Genetics, 2025) | Communication-based: ligand-receptor signals across spatial graph | :white_check_mark: Paper-tested on Xenium + CosMx; cross-sample atlas; mechanistic interpretation | No protein modality (CODEX incompatible); small panels lose L-R gene programs |
| **[Nicheformer](../deep-reads/nicheformer.md)** (Nature Methods, 2025) | Foundation model: pre-trained on spatial and dissociated data | Transfer learning; flexible downstream tasks; lightweight (49M params) | Black box; needs fine-tuning per tissue type |

**Benchmark references:**

- Chen et al., iMeta 2025 — 14 methods across ~600 datasets, 10 technologies, 8 organs
- Yuan et al., Nature Methods 2024 — 13 spatial clustering methods on 34 datasets
- Kang & Zhang, Nucleic Acids Research 2025 — 19 methods across 30 datasets
- Li et al., Genome Biology 2024 — 16 clustering + 5 alignment + 5 integration methods

**Our pick: [CellCharter](../deep-reads/cellcharter.md)** — the only tool paper-verified on both RNA platforms (CosMx, MERSCOPE) and protein platforms (CODEX, IMC). Covers all three datasets: D1 (Xenium + Protein), D4 (Xenium), V1 (CosMx + CODEX).

---

## Step 2: Niche Communication

**Question:** What signaling drives each niche?

!!! note "Key insight"
    Non-spatial cell-cell communication methods detect biologically implausible tissue-spanning signals. Spatial-aware methods (COMMOT, NicheCompass) far outperform them by constraining interactions to local neighborhoods.

| Tool | Approach | Strengths | Limitations |
|------|----------|-----------|-------------|
| **[NicheCompass](../deep-reads/nichecompass.md)** (Nature Genetics, 2025) | Variational model with L-R prior on spatial graph | :white_check_mark: Tested on Xenium (breast cancer) + CosMx (NSCLC, 800K cells); niche + communication jointly modeled | No protein modality — CODEX incompatible; small Xenium panels (~300 genes) lose many gene programs |
| **[COMMOT](../deep-reads/commot.md)** (Nature Methods, 2023) | Optimal transport with spatial distance constraint | :white_check_mark: Tested on Visium, MERFISH, seqFISH, STARmap, Slide-seq; rejects implausible long-range interactions | Xenium/CosMx untested; no niche output (CCC only); scalability concern for large cell counts |
| **CellNEST** (Nature Methods, 2025) | Attention mechanism for relay signaling | Captures A→B→C relay chains; best CCL19-CCR7 localization in benchmark | Very new (2025); limited validation datasets |
| **CellChat v2** (Nature Communications, 2024) | Statistical L-R inference + spatial mode | Largest L-R database; most widely used; rich visualization | Originally non-spatial; spatial mode is an add-on |

**Benchmark references:**

- CellNEST, Nature Methods 2025 — 8 CCC tools compared on spatial localization
- COMMOT, Nature Methods 2023 — showed non-spatial CCC detects implausible signals
- Briefings in Bioinformatics 2025 — comprehensive review of ~100 CCC tools

**Our pick: [NicheCompass](../deep-reads/nichecompass.md)** — the only communication tool paper-tested on both Xenium and CosMx. Discovery on D1 + D4 Xenium, validation on V1 CosMx only (CODEX skipped — protein-only, incompatible). Communication is a discovery insight; cross-platform replication of niche ID (Step 1) is more important than replicating communication.

---

## Step 3: Niche Gene Programs

**Question:** Which genes are expressed because of niche context, not cell type alone?

!!! warning "False positive alert"
    Standard differential expression methods have severely inflated type I error rates (86-95%) when applied to spatial data because they ignore spatial autocorrelation (smiDE, Genome Biology 2025). Spatial-aware DE methods are essential.

| Tool | What It Does | Verified Platforms | Caveats for Our Data |
|------|-------------|-------------------|---------------------|
| **[Niche-DE](../deep-reads/niche-de.md)** (Genome Biology, 2024) | Tests whether gene expression depends on niche context | :white_check_mark: Visium, Slide-seq, CosMx, Xenium | niche-LR extension **cannot run** on Xenium (~300 genes — panel too small for L-R matching). Core niche-DE still works. |
| **smiDE** (Genome Biology, 2025) | Spatial correlation-aware differential expression | Simulated + real spatial data | Newer; complementary to Niche-DE, not a replacement |

**Benchmark reference:**

- smiDE, Genome Biology 2025 — spatial DE benchmark showing non-spatial methods have 86-95% false positive rates

**Our pick: [Niche-DE](../deep-reads/niche-de.md)** — paper-tested on both Xenium and CosMx. Discovery on D1 + D4, validation on V1 CosMx. The niche-LR mechanistic extension is unavailable on small panels, but core niche-DE (context-dependent gene identification) works fully.

---

## Step 4: RNA vs Protein Niche Concordance

**Question:** Do niches defined by RNA agree with niches defined by protein?

!!! tip "Why this replaces morphology mapping"
    TESLA and METI (Linghua Wang's tools) only work on **spot-level Visium data** — verified against source code. They cannot process single-cell Xenium data natively. Since D1 has no Visium and V1 has no Visium, morphology mapping covers only D4's Visium v2 — too narrow for the pipeline. Instead, we leverage D1's unique multi-modal structure for a more novel analysis.

This is the most novel step in the pipeline. D1 provides **both Xenium (RNA) and Protein panel on the same tissue**, and V1 provides **CosMx (RNA) and CODEX (protein)**. Running CellCharter independently on each modality tests whether niche definitions are robust across data types.

**Analysis design:**

| Dataset | RNA Platform | Protein Platform | Comparison |
|---------|-------------|-----------------|------------|
| D1 | Xenium | Protein panel | Same tissue, two modalities |
| V1 | CosMx | CODEX | Cross-platform + cross-modality |

**Why CellCharter:** It is the only niche tool verified on both RNA (CosMx, MERSCOPE) and protein (CODEX, IMC) platforms, making it uniquely suited for cross-modality comparison.

**Possible outcomes:**

- **Niches agree** → robust biological signal, modality-independent
- **Niches disagree** → reveals modality-specific niche features (RNA captures transcriptional programs; protein captures surface markers and signaling states)
- Either result is a discovery that directly fits the hackathon thesis: *"every method defines niche differently"*

---

## Recommended Pipeline

```
D1 + D4 (Xenium)                           V1 (CosMx + CODEX)
       │                                          │
  ┌────┴────┐                                     │
  │ Step 1  │  CellCharter ──────────────────► CellCharter
  │ Niche ID│  (composition-based niches)      (CosMx + CODEX)
  └────┬────┘                                     │
       │                                          │
  ┌────┴────┐                                     │
  │ Step 2  │  NicheCompass ─────────────────► NicheCompass
  │ Comms   │  (L-R signaling per niche)       (CosMx only)
  └────┬────┘                                     │
       │                                          │
  ┌────┴────┐                                     │
  │ Step 3  │  Niche-DE ─────────────────────► Niche-DE
  │ Programs│  (context-dependent genes)        (CosMx only)
  └────┬────┘                                     │
       │                                          │
  ┌────┴────┐                                ┌────┴────┐
  │ Step 4  │  CellCharter ×2               │ Step 4  │  CellCharter ×2
  │ RNA vs  │  D1: Xenium vs Protein        │ RNA vs  │  CosMx vs CODEX
  │ Protein │                                │ Protein │
  └─────────┘                                └─────────┘
```

| Step | Task | Tool | Datasets | Verified? |
|------|------|------|----------|-----------|
| 1 | Niche identification | CellCharter | D1 Xenium + Protein, D4 Xenium, V1 CosMx + CODEX | :white_check_mark: CODEX, CosMx, MERSCOPE, Visium, IMC paper-tested |
| 2 | Niche communication | NicheCompass | D1 + D4 Xenium, V1 CosMx only | :white_check_mark: Xenium + CosMx paper-tested; CODEX incompatible |
| 3 | Niche gene programs | Niche-DE | D1 + D4 Xenium, V1 CosMx only | :white_check_mark: Xenium + CosMx paper-tested; niche-LR unavailable (small panel) |
| 4 | RNA vs protein niches | CellCharter ×2 | D1: Xenium vs Protein, V1: CosMx vs CODEX | :white_check_mark: Both RNA and protein modalities paper-tested |
| 5 | Cross-platform validation | Steps 1-3 on V1 | V1 CosMx (all tools) + CODEX (CellCharter only) | All tools verified on CosMx |

**Discovery on D1 + D4 → Validation on V1 (CosMx for all tools, CODEX for CellCharter only)**
