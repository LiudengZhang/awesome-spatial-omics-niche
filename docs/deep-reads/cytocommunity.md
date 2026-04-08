# CytoCommunity

**Verdict:** The only niche method that directly incorporates clinical labels into niche discovery — finds niches that matter for patient outcome, not just niches that exist.

**Citation:** Hu Y, et al. "Unsupervised and supervised discovery of tissue cellular neighborhoods from cell phenotypes." *Nature Methods* 21:267-278, 2024. [DOI](https://doi.org/10.1038/s41592-024-02283-w)

## Problem Setup

Most niche methods find niches in an unsupervised manner — they identify recurring spatial patterns without knowing whether those patterns are biologically or clinically relevant. Can we find niches that are specifically associated with a condition of interest (e.g., treatment response)?

## Niche Definition

**Composition-based (supervised):** Niche = a spatial cellular community identified via graph partitioning, where the partitioning is guided by clinical condition labels.

The supervision means the method finds niches that differ between conditions, not just niches that recur in the tissue.

## Architecture

1. Build a spatial graph from cell coordinates.
2. Assign cell-type labels to nodes.
3. **Unsupervised mode:** Partition the graph into communities using graph neural network-based community detection.
4. **Supervised mode:** Train the GNN with condition labels (e.g., responder vs non-responder) as supervision, guiding the partitioning to find communities that distinguish conditions.
5. The communities are "tissue cellular neighborhoods" (TCNs).

## Key Innovation

Supervised niche discovery is conceptually distinct from: (1) finding niches unsupervised, then testing for condition association, and (2) standard differential abundance testing. CytoCommunity jointly optimizes for both spatial coherence and condition relevance.

## Evaluation

- Validated on CODEX (CRC, breast cancer) and MIBI-TOF (triple-negative breast cancer) datasets.
- Supervised TCNs showed stronger association with patient outcome than unsupervised TCNs.
- Compared against Schurch et al. cellular neighborhoods, UTAG, and other methods.

## Honest Assessment

**Strengths:**

- Unique approach — no other method directly uses clinical labels to guide niche discovery.
- Finds clinically relevant niches that unsupervised methods may miss or rank as unimportant.
- Both unsupervised and supervised modes available; the unsupervised mode works as a standard niche method.

**Limitations:**

- Supervised mode requires condition labels — cannot discover niches in exploratory settings without outcome data.
- Risk of overfitting to the condition labels, especially with small sample sizes. The method may find spurious niches that happen to correlate with condition in the training set.
- Cross-validation and held-out validation are essential but may be challenging with the small sample sizes typical of spatial datasets (tens of patients, not thousands).
- The supervision introduces a circularity risk: niches are defined to predict outcome, then used to explain outcome.

**Design Decision:** CytoCommunity bets that the most useful niches are the ones that predict clinical outcome. This is pragmatically correct for translational research but philosophically different from the exploratory goal of understanding tissue organization. Both approaches are valid for different purposes.
