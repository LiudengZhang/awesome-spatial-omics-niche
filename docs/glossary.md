# Glossary

**ARI (Adjusted Rand Index):** Metric measuring agreement between two clusterings, adjusted for chance. The default metric in spatial clustering benchmarks; values range from -1 to 1, where 1 is perfect agreement.

**Cellular neighborhood (CN):** A group of cells defined by their local spatial composition, as introduced by Schurch et al. (2020). Each cell is assigned to a CN based on the cell-type proportions in its spatial neighborhood.

**Cell-cell communication (CCC):** Intercellular signaling inferred from co-expression of ligand-receptor pairs. Spatial CCC methods add distance constraints to non-spatial approaches.

**CODEX:** CO-Detection by indEXing — multiplexed protein imaging technology that measures 50+ protein markers at subcellular resolution.

**Domain (spatial):** A contiguous spatial region of similar expression or cell composition. Domains are spatially connected by definition. Contrast with niche.

**Foundation model:** A large neural network pre-trained on massive datasets that can be fine-tuned or applied zero-shot to downstream tasks. In spatial omics, models like Nicheformer learn cell representations from large-scale spatial data.

**GNN (Graph Neural Network):** Neural network that operates on graph-structured data. In spatial omics, cells are nodes and spatial proximity defines edges.

**H&E (Hematoxylin and Eosin):** Standard histological stain used universally in clinical pathology. Hematoxylin stains nuclei blue; eosin stains cytoplasm pink.

**Ligand-receptor (L-R) pair:** A pair of molecules — one secreted or membrane-bound on a sender cell, one on a receiver cell — that mediate intercellular signaling. L-R databases (CellChatDB, CellPhoneDB) catalogue known pairs.

**MERFISH:** Multiplexed Error-Robust Fluorescence In Situ Hybridization — imaging-based spatial transcriptomics technology that measures hundreds of RNA species at subcellular resolution.

**Niche (spatial):** The local microenvironment around a cell. Computationally, defined differently by different methods: cell-type composition, expression average, covariance structure, active signaling, or learned representation. See [What Is a Niche?](concepts/what-is-a-niche.md).

**NMI (Normalized Mutual Information):** Metric measuring agreement between clusterings, normalized to [0, 1]. Often used alongside ARI in benchmarks.

**Optimal transport (OT):** Mathematical framework for finding the most efficient way to transform one distribution into another. Used by COMMOT for modeling spatial cell-cell communication.

**Perturb-seq:** Technology combining CRISPR perturbation with single-cell RNA sequencing. Spatial variants (Perturb-FISH) add spatial readout.

**seqFISH:** Sequential fluorescence in situ hybridization — imaging-based spatial transcriptomics with subcellular resolution.

**Spatial autocorrelation:** The tendency for nearby locations to have similar values. Moran's I is the standard statistic; positive values indicate clustering, negative values indicate dispersion.

**Spatial transcriptomics:** Technologies that measure gene expression with spatial resolution in tissue. Two main families: sequencing-based (Visium, Slide-seq) and imaging-based (MERFISH, seqFISH, CODEX).

**TME (Tumor Microenvironment):** The complex ecosystem surrounding tumor cells, including immune cells, fibroblasts, vasculature, and extracellular matrix. A major focus of niche analysis in cancer research.

**VAE (Variational Autoencoder):** Generative model that learns a probabilistic latent representation. Used by ENVI, scVI, SIMVI for spatial and single-cell modeling.

**Visium:** 10x Genomics spatial transcriptomics platform that captures whole transcriptome at ~55 um spot resolution. Each spot contains multiple cells.
