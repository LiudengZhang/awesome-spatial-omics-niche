# Awesome Spatial Omics Niche [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Every method defines "niche" differently. This list tells you how — and what each definition can and cannot find.

A niche is the local microenvironment around a cell — who is nearby, what they express, what signals they exchange. There is no consensus computational definition. Every method on this list implicitly defines what a niche is through its design choices: the input features it uses, the neighborhood it considers, and the structure it imposes. That definition determines what the method can discover and what it will miss. This list organizes methods by their niche definition rather than chronologically, so you can choose the right tool for your biological question.

## Contents

- [The Niche Definition Problem](#the-niche-definition-problem)
- [Surveys and Reviews](#surveys-and-reviews)
- [Composition-Based Methods](#composition-based-methods)
- [Expression Neighborhood Methods](#expression-neighborhood-methods)
- [Covariance-Based Methods](#covariance-based-methods)
- [Communication-Based Methods](#communication-based-methods)
- [Spatial Domain Methods](#spatial-domain-methods)
- [Morphology-Integrated Methods](#morphology-integrated-methods)
- [Foundation Models](#foundation-models)
- [Niche-Aware Downstream Analysis](#niche-aware-downstream-analysis)
- [Benchmarks and Evaluation](#benchmarks-and-evaluation)
- [Datasets](#datasets)
- [Tools and Infrastructure](#tools-and-infrastructure)
- [Companion Site](#companion-site)

## The Niche Definition Problem

The same tissue section analyzed by different methods will yield different niches — not because of bugs, but because each method defines "niche" differently. A composition-based method sees cell-type proportions in a neighborhood. An expression method sees averaged transcriptomes. A communication method sees active ligand-receptor pairs. A covariance method sees correlation structure. These are genuinely different biological lenses, and the right choice depends on whether you care about who is nearby, what is expressed nearby, or what is being communicated nearby.

## Surveys and Reviews

- [Spatial Components of Molecular Tissue Biology](https://doi.org/10.1038/s41587-021-01182-1) - Comprehensive framework for spatial omics analysis including neighborhood concepts and tissue architecture (Nature Biotechnology, 2022).
- [Profiling Immune Cell Tissue Niches](https://doi.org/10.1016/j.jaci.2024.03.005) - Review of spatial methods for characterizing immune niches in tissue with focus on clinical applications (JACI, 2024).
- [Tissue Niches Through Spatial Tissue Profiling](https://doi.org/10.1002/eji.202451049) - Review of how spatial profiling technologies reveal tissue niche organization and immunopathology (EJI, 2024).
- [Deciphering Normal and Cancer Stem Cell Niches by Spatial Transcriptomics](https://doi.org/10.1101/gad.302307.124) - How spatial transcriptomics reveals stem cell niche organization in normal and tumor contexts (Genes & Development, 2025).
- [Best Practices: Neighborhood Analysis](https://www.sc-best-practices.org/spatial/neighborhood.html) - Practical guide to neighborhood analysis from the sc-best-practices project, covering method selection and implementation.

## Composition-Based Methods

Niche = cell-type proportions in a spatial neighborhood. These methods count who is nearby.

- [CellCharter](https://github.com/CSOgroup/cellcharter) - Graph neural network clustering spatial neighborhoods by cell-type composition; handles multiple spatial resolutions and works across imaging and sequencing platforms (Nature Genetics, 2024).
- [UTAG](https://github.com/ElementoLab/utag) - Unsupervised spatial domain detection combining gene expression with spatial connectivity via message passing on cell graphs (Nature Methods, 2022).
- [CytoCommunity](https://github.com/huBioinfo/CytoCommunity) - Identifies tissue cellular neighborhoods as communities via supervised graph neural networks; uniquely uses condition labels to find disease-relevant niches (Nature Methods, 2024).
- [SpatialLDA](https://github.com/KChen-lab/SpatialLDA) - Topic modeling applied to spatial neighborhoods, treating each region as a document and cell types as words to discover latent tissue microenvironments (Genome Biology, 2022).
- [SOTIP](https://github.com/bm2-lab/SOTIP) - Spatial Omics mulTI-task analysis Pipeline using optimal transport to compare and cluster spatial neighborhoods across samples (Nature Communications, 2023).
- [CNTools](https://github.com/schwartzlab-methods/CNTools) - Cellular neighborhood identification using kernel density estimation and network analysis (Bioinformatics, 2024).

## Expression Neighborhood Methods

Niche = gene expression patterns aggregated over spatial neighbors. These methods measure what is expressed nearby.

- [BANKSY](https://github.com/prabhakarlab/Banksy_py) - Augments each cell's expression with the mean and gradient of its spatial neighborhood; the gradient captures expression boundaries, making it uniquely sensitive to tissue interfaces (Nature Genetics, 2024).
- [SpaGCN](https://github.com/jianhuupenn/SpaGCN) - Graph convolutional network integrating gene expression, spatial location, and histology for spatial domain identification (Nature Methods, 2021).
- [SpiceMix](https://github.com/kharchenkolab/SpiceMix) - Probabilistic model decomposing spatial transcriptomics into metagenes with spatial priors; handles subcellular resolution (Nature Genetics, 2023).
- [SpatialPCA](https://github.com/shangll123/SpatialPCA) - Spatially-aware dimension reduction that explicitly models spatial correlations in gene expression (Nature Communications, 2022).
- [NichePCA](https://github.com/niche-pca/NichePCA) - Principal component analysis of spatial neighborhoods, providing interpretable niche axes aligned with biological gradients (Bioinformatics, 2025).

## Covariance-Based Methods

Niche = gene-gene covariance structure across spatial neighbors. These methods capture how expression patterns co-vary in local neighborhoods, not just their averages.

- [ENVI/COVET](https://github.com/dpeerlab/ENVI) - COVET computes niche as a gene-gene covariance tensor over spatial neighbors; ENVI uses this to impute missing genes from spatial data via a conditional VAE (Nature Biotechnology, 2024).
- [SIMVI](https://github.com/KlugerLab/SIMVI) - Spatially-informed variational inference separating intrinsic cell state from extrinsic niche effects; jointly models both to prevent confounding (Nature Communications, 2025).
- [SPACE](https://github.com/dpeerlab/SPACE) - Identifies spatial gene expression patterns through spatially-aware clustering that captures local covariance structure (Cell Systems, 2023).

## Communication-Based Methods

Niche = active ligand-receptor signaling between spatial neighbors. These methods define neighborhoods by what cells are saying to each other.

- [NicheCompass](https://github.com/Lotfollahi-lab/nichecompass) - Graph neural network that explicitly models ligand-receptor interactions as niche features; learns a unified niche atlas across samples and conditions (Nature Genetics, 2025).
- [COMMOT](https://github.com/zcang/COMMOT) - Optimal transport framework for cell-cell communication in spatial transcriptomics; infers spatially-constrained signaling without requiring single-cell resolution (Nature Methods, 2022).
- [DeepLinc](https://github.com/xryanglab/DeepLinc) - Deep generative model (VGAE) for inferring cell interaction landscapes from spatial transcriptomics; used as a benchmark baseline in NicheCompass evaluation (Genome Biology, 2022).
- [CellChat v2](https://github.com/jinworks/CellChat) - Widely-used cell-cell communication toolkit with spatial extensions; infers, visualizes, and compares intercellular signaling networks (Nature Communications, 2021; v2 update 2024).
- [SpaTalk](https://github.com/ZJUFanLab/SpaTalk) - Cell-cell communication analysis using graph networks to model spatially-resolved ligand-receptor interactions at single-cell resolution (Nature Communications, 2022).
- [SpatialDM](https://github.com/StatBiomed/SpatialDM) - Global and local statistical tests for spatially co-expressed ligand-receptor pairs using bivariate Moran's I statistics (Nature Communications, 2023).
- [NicheNet](https://github.com/saeyslab/nichenetr) - Predicts ligands driving gene expression changes in receiver cells by integrating prior knowledge of signaling and gene regulatory networks; multinichenetr extends this to multi-sample multi-condition spatial designs (Nature Methods, 2020).

## Spatial Domain Methods

Niche != domain. Spatial domain methods find contiguous regions of similar expression. While related to niches, domains are defined by spatial continuity rather than recurring cellular motifs. A niche (e.g., perivascular niche) can appear at many locations; a domain (e.g., cortical layer 3) is a single place. We include these because they are frequently confused with niche methods.

- [GraphST](https://github.com/JinmiaoChenLab/GraphST) - Self-supervised contrastive learning on spatial graphs for domain identification; strong benchmark performance but finds regions, not niches (Nature Communications, 2022).
- [STAGATE](https://github.com/zhanglabtools/STAGATE) - Graph attention autoencoder for spatial domain detection using adaptive graph construction (Nature Communications, 2022).
- [BayesSpace](https://github.com/edward130603/BayesSpace) - Bayesian spatial clustering that enhances resolution of Visium spots via neighbor-informed priors (Nature Biotechnology, 2021).
- [BASS](https://github.com/zhengli09/BASS) - Multi-scale Bayesian framework for simultaneous cell-type and spatial domain identification (Nature Methods, 2022).
- [SEDR](https://github.com/JinmiaoChenLab/SEDR) - Variational autoencoder with deep graph embedding for spatial domain identification (Bioinformatics, 2022).
- [SpaceFlow](https://github.com/hongleir/SpaceFlow) - Spatially-regularized deep graph network for domain segmentation with pseudo-spatiotemporal trajectory inference (Nature Communications, 2022).
- [MENDER](https://github.com/yuanzhiyuan/MENDER) - Multi-range cell context decoding for spatial domain identification across different spatial resolutions (Nature Methods, 2024).
- [STACI](https://github.com/MaCompBio/STACI) - Statistical approach to spatial domain identification using spatial autocorrelation and clustering (Bioinformatics, 2023).

## Morphology-Integrated Methods

Niche = joint histological and transcriptomic features. These methods combine H&E morphology with molecular data.

- [TESLA](https://github.com/jianhuupenn/TESLA) - Annotates spatial transcriptomics at finer resolution by integrating H&E image features with gene expression; developed by Linghua Wang's group at MD Anderson (Cell Systems, 2023).
- [METI](https://github.com/jianhuupenn/METI) - Maps tumor microenvironment interactions by combining H&E morphology with spatial gene expression for niche characterization; also from Linghua Wang's group (Nature Communications, 2024).
- [GigaTIME](https://github.com/prov-gigatime/GigaTIME) - H&E to virtual multiplex immunofluorescence translation across 14,256 patients; the most practically impactful virtual tissue application, succeeding because spatial pattern translation is a correlation task where deep learning excels (Cell, 2025).

## Foundation Models

Niche = learned representation from large-scale pre-training. These models learn what a niche is from data rather than imposing a definition.

- [Nicheformer](https://github.com/theislab/nicheformer) - 49M-parameter model that beats the 444M TranscriptFormer on spatial tasks by incorporating spatial context; demonstrates that architecture and data type matter more than parameter count (Nature Methods, 2025).
- [Novae](https://github.com/MICS-Lab/novae) - Graph-based foundation model for spatial transcriptomics with zero-shot spatial domain and niche inference; works across technologies without retraining (Nature Methods, 2025).
- [scNiche](https://github.com/wanglabtongji/scNiche) - Foundation model for single-cell spatial niche analysis combining graph attention with expression modeling for niche-aware cell embeddings (Nature Communications, 2025).

## Niche-Aware Downstream Analysis

Methods that use niche context as input to downstream biological questions rather than defining niches as their primary output.

- [MISTy](https://github.com/saezlab/mistyR) - Multiview Intercellular Spatial modeling framework using explainable machine learning to model intra- and intercellular spatial relationships at multiple scales (Genome Biology, 2022).
- [NCEM](https://github.com/theislab/ncem) - Node-Centric Expression Models that learn how spatial neighborhood composition influences gene expression via graph neural networks (Nature Biotechnology, 2022).
- [Niche-DE](https://github.com/ccb-hms/NicheDE) - Detects niche-associated differential expression by testing whether a gene's expression depends on neighboring cell-type composition; separates niche effects from cell-intrinsic expression (Genome Biology, 2024).
- [NicheFlow](https://github.com/theislab/nicheflow) - Flow-based generative model for niche trajectory analysis across spatial slides; orders niches along continuous biological gradients (ICLR, 2025).
- [NICHES](https://github.com/msraredon/NICHES) - Computes cell-cell interaction scores in spatial neighborhoods, generating niche-specific interaction profiles per cell (Bioinformatics, 2022).
- [ONTraC](https://github.com/wwang-chcn/ONTraC) - Ordered Niche Trajectory Construction using graph neural networks to order spatial niches along continuous biological gradients (Genome Biology, 2025).
- [Squidpy nhood_enrichment](https://github.com/scverse/squidpy) - Tests whether cell-type co-localization patterns deviate from random, providing a statistical measure of niche preference; part of the broader Squidpy spatial analysis toolkit (Nature Methods, 2022).
- [MESA](https://github.com/FunctionLab/MESA) - Measures spatial association between gene expression and cell-type neighborhoods to find genes with spatially structured expression patterns (Nature Methods, 2024).

## Benchmarks and Evaluation

- [NAR 2025 Benchmark](https://doi.org/10.1093/nar/gkae1143) - Systematic comparison of 19 spatial domain methods across 30 datasets; exposes that most benchmarks test domain detection, not niche identification (NAR, 2025).
- [Niche Identification Benchmark](https://doi.org/10.1101/2026.01.20.632894) - Dedicated evaluation of 16 methods for niche identification via domain segmentation across multiple tissues and platforms (bioRxiv, 2026).
- [Yuan et al. Spatial Clustering](https://doi.org/10.1038/s41592-024-02215-8) - Comparison of 13 spatial clustering methods on 34 datasets with practical recommendations for method selection (Nature Methods, 2024).
- [Spatial Integration Benchmark](https://doi.org/10.1186/s13059-024-03361-0) - Evaluation of 16 clustering, 5 alignment, and 5 integration methods for spatial transcriptomics data (Genome Biology, 2024).

## Datasets

- [Human DLPFC](https://doi.org/10.1038/s41593-020-00787-0) - The standard benchmark dataset for spatial methods with 12 tissue sections and manual layer annotations; note that it tests domain detection, not niche identification (Maynard et al., Nature Neuroscience, 2021).
- [CRC CODEX](https://doi.org/10.1016/j.cell.2020.07.005) - The origin of computational cellular neighborhoods; 9 niches identified in colorectal cancer via CODEX multiplexed imaging — the most influential niche paper (Schurch et al., Cell, 2020).
- [Mouse Brain MERFISH](https://doi.org/10.1126/science.aau5324) - Spatially resolved transcriptomics of the mouse hypothalamus with cell-type annotations suitable for niche analysis (Moffitt et al., Science, 2018).
- [Mouse Embryo seqFISH](https://doi.org/10.1038/s41587-021-01006-2) - Subcellular-resolution spatial transcriptomics of mouse embryo development with rich niche structure (Lohoff et al., Nature Biotechnology, 2022).
- [Hepatic Niches](https://doi.org/10.1016/j.cell.2021.12.018) - Spatial proteomics and transcriptomics atlas of human liver defining zonated hepatic niches (Guilliams et al., Cell, 2022).
- [Cardiac Niches](https://doi.org/10.1038/s41586-023-06311-1) - Spatial multi-omic atlas of the human heart mapping cardiac cellular neighborhoods and their disease associations (Kanemaru et al., Nature, 2023).

## Tools and Infrastructure

- [Squidpy](https://github.com/scverse/squidpy) - Comprehensive spatial omics analysis framework with graph construction, neighborhood enrichment, spatial statistics, and image analysis (Nature Methods, 2022).
- [Giotto](https://github.com/drieslab/Giotto) - End-to-end spatial omics analysis toolkit with spatial network construction, niche clustering, and cell-cell communication analysis (Genome Biology, 2021).
- [Scanpy](https://github.com/scverse/scanpy) - Standard Python toolkit for single-cell analysis providing the preprocessing pipeline most spatial methods depend on (Genome Biology, 2018).
- [AnnData](https://github.com/scverse/anndata) - The data structure underlying the scverse ecosystem; standardized format for single-cell and spatial data (Nature Biotechnology, 2024).

## Companion Site

For in-depth paper analyses, the niche definition taxonomy, method comparisons, and synthesis essays, visit the [Awesome Spatial Omics Niche companion site](https://liudengzhang.github.io/awesome-spatial-omics-niche/).

## Contributing

Contributions are welcome. Please read the [contribution guidelines](contributing.md) before submitting a pull request.
