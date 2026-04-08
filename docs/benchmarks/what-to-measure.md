# What to Measure

## Beyond ARI

Adjusted Rand Index (ARI) is the default metric for spatial clustering benchmarks. It measures agreement between predicted and ground-truth labels. For domain detection with layer annotations, ARI is reasonable. For niche identification, it is insufficient.

## Why ARI Fails for Niches

1. **ARI requires bijective label mapping.** If a method finds 12 niches and the ground truth has 9, ARI penalizes the finer granularity even if the 12-niche partition is biologically more accurate.
2. **ARI is symmetric in errors.** Splitting one true niche into two predicted niches is penalized the same as merging two true niches into one. Biologically, these errors have very different consequences.
3. **ARI ignores spatial distribution.** Two methods might achieve the same ARI, but one correctly identifies distributed niches while the other just finds contiguous regions that happen to overlap with ground truth.

## Better Metrics for Niche Evaluation

### Biological Consistency

- **Marker gene enrichment:** Do identified niches show enrichment for known niche-associated genes?
- **Cell-type composition homogeneity:** Are the cells within each niche consistent in their cell-type composition?
- **Functional coherence:** Do cells within a niche share functional signatures (pathway activity, signaling state)?

### Clinical Relevance

- **Outcome association:** Do niche proportions or compositions predict patient outcome?
- **Treatment response:** Do niches stratify patients by treatment response?
- **Known biology recovery:** Does the method recover known niches (e.g., tertiary lymphoid structures, perivascular niches)?

### Robustness

- **Cross-platform stability:** Does the same tissue analyzed by different platforms yield consistent niches?
- **Parameter sensitivity:** How much do results change with neighborhood size, number of clusters, or other hyperparameters?
- **Subsampling stability:** Do niches remain stable when cells are randomly removed?

### Distribution-Aware Metrics

- **Spatial distribution score:** Do predicted niche labels correctly appear at multiple disconnected locations, or are they confined to contiguous regions?
- **Cross-location consistency:** Are instances of the same niche type at different locations truly similar in composition/expression?

## A Practical Evaluation Framework

For a new niche method, we recommend evaluating along three axes:

| Axis | Question | Metrics |
|------|----------|---------|
| **Accuracy** | Do niches match known biology? | Marker enrichment, known niche recovery, ARI (if ground truth available) |
| **Utility** | Do niches enable downstream discovery? | Niche-DE gene count, outcome association, novel hypothesis generation |
| **Robustness** | Are niches reliable? | Cross-platform consistency, parameter sensitivity, subsampling stability |

No single metric captures niche quality. A method that scores high on accuracy but low on robustness is fragile. A method that is robust but finds no biologically meaningful niches is useless. Evaluate all three axes.
