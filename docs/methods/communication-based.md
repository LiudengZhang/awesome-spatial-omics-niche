# Communication-Based Methods

**Niche definition: active ligand-receptor signaling between spatial neighbors.**

These methods answer the question: what are cells saying to each other? They define neighborhoods by intercellular communication rather than co-localization, making them the closest current methods to functional niche definitions.

## Key Methods

### NicheCompass

- **Paper:** [Nature Genetics, 2025](https://doi.org/10.1038/s41588-025-02067-y)
- **Code:** [github.com/Lotfollahi-lab/nichecompass](https://github.com/Lotfollahi-lab/nichecompass)
- **Niche definition:** Graph neural network that explicitly encodes ligand-receptor interactions as features of the spatial graph, learning niche embeddings that reflect both cell identity and intercellular signaling.
- **Key innovation:** Builds a unified niche atlas across samples and conditions, enabling cross-sample niche comparison with communication-aware embeddings.
- **Strengths:** Combines the best of composition-based (who is nearby) and communication-based (what are they saying) approaches. Cross-sample integration.
- **Limitations:** Depends on the L-R database used; niche definitions are constrained by known interactions.
- **Verdict:** The most complete niche method currently available — integrates cell identity, spatial context, and signaling into a single framework.

### COMMOT

- **Paper:** [Nature Methods, 2022](https://doi.org/10.1038/s41592-022-01728-4)
- **Code:** [github.com/zcang/COMMOT](https://github.com/zcang/COMMOT)
- **Niche definition:** Optimal transport framework that infers spatially-constrained cell-cell communication without requiring single-cell resolution.
- **Key innovation:** Works with spot-level data (Visium) where multiple cells are in each spot — uses optimal transport to handle mixed spots.
- **Strengths:** Mathematically principled, handles spot-level data, provides directionality of signaling.
- **Limitations:** Computational cost scales with dataset size; optimal transport can be expensive for large datasets.

### CellChat v2

- **Paper:** [Nature Communications, 2021](https://doi.org/10.1038/s41467-021-21246-9) (v2 update 2024)
- **Code:** [github.com/jinworks/CellChat](https://github.com/jinworks/CellChat)
- **Niche definition:** Infers intercellular communication networks from expression data using curated L-R databases with spatial extensions.
- **Key innovation:** Most widely used cell-cell communication tool; v2 adds spatial awareness and comparison across conditions.
- **Strengths:** Large user community, extensive L-R database (CellChatDB), rich visualization, condition comparison.
- **Limitations:** Originally designed for non-spatial scRNA-seq; spatial extensions are added rather than native.

### SpaTalk

- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-022-32111-8)
- **Code:** [github.com/ZJUFanLab/SpaTalk](https://github.com/ZJUFanLab/SpaTalk)
- **Niche definition:** Graph network modeling of spatially resolved L-R interactions at single-cell resolution.
- **Key innovation:** Explicitly designed for single-cell resolution spatial data; models the spatial proximity constraint on L-R interactions.
- **Strengths:** Single-cell resolution, integrates downstream signaling pathway activity.

### SpatialDM

- **Paper:** [Nature Communications, 2023](https://doi.org/10.1038/s41467-023-39608-w)
- **Code:** [github.com/StatBiomed/SpatialDM](https://github.com/StatBiomed/SpatialDM)
- **Niche definition:** Statistical testing for spatially co-expressed ligand-receptor pairs using bivariate Moran's I.
- **Key innovation:** Provides both global (tissue-wide) and local (per-spot) tests for spatial co-expression of L-R pairs.
- **Strengths:** Statistical rigor with p-values, distinguishes local hotspots from global trends.

### NicheNet

- **Paper:** [Nature Methods, 2020](https://doi.org/10.1038/s41592-019-0667-5)
- **Code:** [github.com/saeyslab/nichenetr](https://github.com/saeyslab/nichenetr)
- **Niche definition:** Predicts which ligands from sender cells drive gene expression changes in receiver cells by integrating prior knowledge of signaling and gene regulatory networks.
- **Key innovation:** Focuses on the downstream effect of signaling rather than just L-R co-expression. multinichenetr extends to multi-sample, multi-condition spatial designs.
- **Strengths:** Prior knowledge integration, focuses on functional impact of signaling.
- **Limitations:** Heavily dependent on prior knowledge quality; original version not spatially aware (multinichenetr adds spatial context).

## When to Use Communication-Based Methods

**Best for:**

- Understanding intercellular signaling in spatial context.
- Identifying functionally relevant niches defined by active communication rather than just proximity.
- Comparing signaling networks across conditions (e.g., responder vs non-responder).

**Not ideal for:**

- Tissues where the main spatial organization is driven by composition rather than signaling.
- Datasets with few measured genes that may not cover key L-R pairs.
- When you want niche definitions independent of L-R database completeness.
