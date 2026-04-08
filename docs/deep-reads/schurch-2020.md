# Schurch et al. 2020 — The Origin of Cellular Neighborhoods

**Verdict:** The paper that defined the computational niche analysis field; its 9 cellular neighborhoods in colorectal cancer remain the most cited example of spatial niche identification.

**Citation:** Schurch CM, Bhate SS, et al. "Coordinated cellular neighborhoods orchestrate antitumoral immunity at the colorectal cancer invasive front." *Cell* 182(5):1341-1359.e19, 2020. [DOI](https://doi.org/10.1016/j.cell.2020.07.005)

## Problem Setup

How are immune and stromal cells organized in the colorectal cancer tumor microenvironment, and does this spatial organization predict patient outcome?

## Niche Definition

**Composition-based:** For each cell, count the cell types within a fixed-radius neighborhood. Cluster cells by their neighborhood cell-type composition vectors. Each cluster is a "cellular neighborhood" (CN).

This is arguably the simplest possible niche definition: niche = who is nearby.

## Method

1. CODEX multiplexed imaging of 140 tissue regions from 35 CRC patients with 56 protein markers.
2. Cell segmentation and phenotyping into 28 cell types.
3. For each cell, compute the composition of cell types within a 10-cell nearest-neighbor window.
4. Cluster the composition vectors using k-means (k=9).
5. Map clusters back to tissue to visualize spatial distribution.
6. Correlate CN frequency with clinical outcome (Crohn's-like reaction, tumor grade, survival).

## Key Findings

9 cellular neighborhoods identified:

- **CN1-3:** Tumor-enriched (varying stroma content)
- **CN4-5:** Immune-enriched (follicle-like, granulocyte-enriched)
- **CN6:** Smooth muscle neighborhood
- **CN7-8:** Mixed immune-stromal
- **CN9:** Plasma cell enriched

The ratio of immune-enriched to tumor-enriched CNs at the invasive front correlated with patient prognosis.

## Honest Assessment

**Strengths:**

- Defined the computational framework that every subsequent niche method builds on or responds to.
- Clean demonstration that spatial organization, not just cell-type presence, predicts outcome.
- The simplicity of the method (k-nearest-neighbors + k-means) makes it reproducible and interpretable.

**Limitations:**

- The k=9 choice for number of neighborhoods is arbitrary — the paper does not justify why 9 and not 7 or 12.
- The 10-cell nearest-neighbor window size is a fixed parameter that implicitly determines what scale of niche is detected.
- CODEX measures proteins, not RNA — the neighborhoods may differ if defined by transcriptomic features.
- The simplicity that makes it accessible also means it misses expression gradients, signaling dynamics, and covariance structure.

**Legacy:** This paper established "cellular neighborhoods" as a concept and demonstrated that niche composition predicts clinical outcome. Nearly every niche method since 2020 benchmarks against this framework or uses its conceptual vocabulary. The CRC CODEX dataset remains one of the few true niche benchmarks (as opposed to domain benchmarks like DLPFC).
