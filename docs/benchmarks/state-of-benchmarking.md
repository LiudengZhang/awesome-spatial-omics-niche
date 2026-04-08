# State of Benchmarking

## The Core Problem

The most commonly used benchmark in spatial omics — the human DLPFC dataset — tests **domain detection**, not **niche identification**. Methods optimized for DLPFC learn to find contiguous cortical layers. This is a valid task, but it is not the same as finding recurring cellular microenvironments.

As a result, we have extensive benchmarks for a task that is adjacent to, but distinct from, the one most niche researchers care about.

## Existing Benchmarks

### Domain-Focused Benchmarks

| Benchmark | Methods | Datasets | What It Tests |
|-----------|---------|----------|---------------|
| NAR 2025 | 19 | 30 | Spatial domain detection (ARI against layer annotations) |
| Yuan et al. (Nature Methods, 2024) | 13 | 34 | Spatial clustering (ARI, NMI against manual annotations) |
| Genome Biology 2024 | 16 clustering + 5 alignment + 5 integration | Multiple | Clustering, alignment, and integration |

These benchmarks consistently use DLPFC layer annotations as ground truth. A method that scores high on DLPFC may or may not identify distributed niches in tumor tissue.

### Niche-Focused Benchmarks

| Benchmark | Methods | Datasets | What It Tests |
|-----------|---------|----------|---------------|
| Niche ID benchmark (bioRxiv, 2026) | 16 | Multiple | Niche identification via domain segmentation — closer to niche testing but still relies on domain-like annotations |
| CellCharter (Nature Genetics, 2024) | 6 | Cross-platform | Multi-resolution niche detection |
| NicheCompass (Nature Genetics, 2025) | 5 | Cross-sample | Communication-aware niche atlas building |

These are closer to evaluating niche methods but still limited by the availability of ground-truth niche annotations.

## The Annotation Problem

True niche benchmarking requires ground-truth niche labels — and those barely exist. The CRC CODEX dataset (Schurch et al., 2020) is the closest thing: 9 cellular neighborhoods manually validated by pathologists. But:

- 9 neighborhoods is a small number of classes.
- The neighborhoods were defined by the same composition-based approach being benchmarked (circular).
- The dataset is multiplexed protein imaging (CODEX), not spatial transcriptomics.

No spatial transcriptomics dataset has expert-validated niche annotations suitable for benchmarking.

## What a Good Niche Benchmark Would Look Like

1. **Distributed ground truth:** Niche annotations at multiple disconnected locations (not contiguous regions).
2. **Multiple niche types:** More than binary (niche vs not-niche) — a hierarchy of niche definitions tested simultaneously.
3. **Cross-definition evaluation:** Test whether methods using different niche definitions (composition, expression, communication) find consistent structure.
4. **Biological validation:** Functional readouts (perturbation data, clinical outcome) to validate that identified niches are biologically meaningful, not just statistically separable.
5. **Multi-platform:** Evaluate across imaging and sequencing platforms.

## Current State

We are in a situation where:

- **Domain benchmarks are mature** but test the wrong task for niche researchers.
- **Niche benchmarks are nascent** and lack proper ground truth.
- **Cross-method comparison is almost impossible** because different methods define niches differently — comparing ARI scores across methods that use different niche definitions is not meaningful.

The field needs a DLPFC-equivalent for niches: a well-annotated dataset with expert-validated niche labels that the community agrees to benchmark against. The Schurch CRC dataset is the closest candidate but has limitations.
