# Morphology-Based Methods

**Niche definition: joint histological and molecular features.**

These methods integrate H&E staining images with molecular data (transcriptomics or proteomics) to define niches that reflect both tissue architecture and gene expression. They leverage the fact that H&E images are universally available in clinical settings and contain rich spatial information about tissue organization.

## Key Methods

### TESLA

- **Paper:** [Cell Systems, 2023](https://doi.org/10.1016/j.cels.2022.12.006)
- **Code:** [github.com/jianhuupenn/TESLA](https://github.com/jianhuupenn/TESLA)
- **Niche definition:** Combines H&E image features with spatial gene expression to annotate tissue at finer resolution than the original spatial transcriptomics spots.
- **Key innovation:** Super-resolution annotation — uses histology to subdivide spatial transcriptomics spots, enabling finer-grained niche characterization. Developed by Linghua Wang's group at MD Anderson.
- **Strengths:** Enhances resolution of Visium-scale data, integrates morphological features that are invisible in expression data alone, clinically relevant (H&E is standard).
- **Limitations:** Requires paired H&E + spatial transcriptomics; the resolution enhancement is learned from training data and may not generalize across tissues or staining protocols.

### METI

- **Paper:** [Nature Communications, 2024](https://doi.org/10.1038/s41467-024-44835-y)
- **Code:** [github.com/jianhuupenn/METI](https://github.com/jianhuupenn/METI)
- **Niche definition:** Maps tumor microenvironment interactions by combining morphological features from H&E with spatial gene expression to characterize cell-cell interactions at the tissue level.
- **Key innovation:** Explicitly targets TME characterization by integrating morphological phenotyping with molecular spatial data. Also from Linghua Wang's group.
- **Strengths:** Directly addresses tumor niche characterization, clinically actionable output, builds on TESLA's framework.
- **Limitations:** Cancer-focused; applicability to non-tumor tissues not fully demonstrated.

### GigaTIME

- **Paper:** [Cell, 2025](https://doi.org/10.1016/j.cell.2024.11.041)
- **Code:** [github.com/prov-gigatime/GigaTIME](https://github.com/prov-gigatime/GigaTIME)
- **Niche definition:** Translates H&E images into virtual multiplex immunofluorescence, enabling niche characterization from routine clinical slides.
- **Key innovation:** Trains on paired H&E and multiplex IF data from 14,256 patients to predict protein marker expression from H&E alone. Enables retrospective niche analysis of any H&E-stained slide.
- **Strengths:** Massive clinical applicability — most existing tissue samples only have H&E. Succeeds because spatial pattern translation is a correlation task where deep learning excels.
- **Limitations:** Predictions are correlative — the model learns H&E-to-IF mapping, not causal relationships. Accuracy varies across tissue types and markers.

## The Linghua Wang Connection

TESLA and METI are both developed by Linghua Wang's group at MD Anderson Cancer Center. Wang's research focuses on integrating computational pathology with spatial transcriptomics to characterize tumor microenvironments. Her group pioneered the approach of using H&E morphology as a lens into molecular spatial organization — the insight that tissue architecture visible under the microscope encodes information about the cellular niche.

This work is directly relevant to the Colin Stroud Hackathon, which Wang organizes, themed "Decoding Functional Niches in Cancer by Leveraging Spatial Omics and AI."

## When to Use Morphology-Based Methods

**Best for:**

- Clinical datasets where H&E is the primary or only available modality.
- Enhancing resolution of Visium or other spot-level spatial transcriptomics.
- Retrospective analysis of archived tissue with existing H&E slides.
- Tumor microenvironment characterization integrating pathological features.

**Not ideal for:**

- Datasets without paired histology images.
- Questions about molecular mechanisms that are not reflected in tissue morphology.
- Non-epithelial tissues where H&E morphology is less informative.
