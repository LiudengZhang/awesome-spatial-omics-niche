# Candidate Papers: End-to-End Spatial Niche Analysis in Cancer (2023-2026)

Searched 2026-04-08. Focus: papers that chain multiple analysis steps (niche ID -> communication -> downstream biology), not single-tool papers.

---

## 1. Spatial predictors of immunotherapy response in triple-negative breast cancer

- **Authors:** Wang XQ et al.
- **Journal:** Nature (2023), 621(7980):868-876
- **Cancer type:** Triple-negative breast cancer (TNBC)
- **Platforms:** Imaging mass cytometry (IMC)
- **Tools/methods:** IMC phenotyping, spatial interaction analysis, multivariate modeling
- **Workflow:** Cell phenotyping -> spatial interaction quantification (tumor-immune proximity) -> temporal dynamics (3 timepoints: pre-treatment, on-treatment, surgery) -> multivariate prediction of immunotherapy response
- **Key findings:** Fractions of proliferating CD8+TCF1+ T cells and MHCII+ cancer cells + spatial interactions with B cells predict ICB response. Sensitive vs resistant tumors distinguishable early on-treatment.
- **Cross-platform validation:** Two cohorts from randomized clinical trial
- **Why it fits:** Full pipeline from spatial phenotyping -> niche interactions -> therapy response prediction. Clinical trial context. High impact.
- **URL:** https://www.nature.com/articles/s41586-023-06498-3

---

## 2. Spatial immune scoring system predicts hepatocellular carcinoma recurrence

- **Authors:** (Nature 2025 team)
- **Journal:** Nature (2025), 640(8060)
- **Cancer type:** Hepatocellular carcinoma (HCC)
- **Platforms:** Spatial transcriptomics + spatial proteomics
- **Tools/methods:** NK cell spatial distribution analysis, TIMES score (5-biomarker spatial model)
- **Workflow:** Spatial immune cell mapping -> NK cell distribution at invasive front vs tumor center -> biomarker discovery (SPON2, ZFP36L2, etc.) -> TIMES recurrence prediction score -> validation across 5 cohorts (n=231)
- **Key findings:** SPON2+ NK cells at invasive front lower recurrence risk (HR=88.2). TIMES score outperforms TNM and BCLC staging.
- **Cross-platform validation:** 5 multi-center cohorts, 82.2% real-world accuracy
- **Why it fits:** Full niche -> biology -> clinical prediction pipeline. Multi-cohort validation. Nature main journal.
- **URL:** https://www.nature.com/articles/s41586-025-08668-x

---

## 3. Spatial proximity of tumor-immune interactions predicts patient outcome in hepatocellular carcinoma

- **Authors:** (Hepatology 2024 team)
- **Journal:** Hepatology (2024), 79(4)
- **Cancer type:** Hepatocellular carcinoma (HCC)
- **Platforms:** CODEX multiplexed immunofluorescence
- **Tools/methods:** CODEX phenotyping, spatial interaction clustering, survival modeling
- **Workflow:** Multiplexed protein imaging -> cell phenotyping -> spatial interaction pattern identification (4 patient groups IC1-IC4) -> tumor-CD8+ T cell proximity quantification -> survival prediction -> validation in independent cohort
- **Key findings:** 4 distinct tumor-immune interaction patterns; tumor-CD8+ T cell proximity strongly predicts survival. Validated in 2 cohorts (68 Thai + 190 Chinese patients).
- **Cross-platform validation:** Two independent patient cohorts (TIGER-LC and LCI)
- **Why it fits:** Niche interaction patterns -> survival prediction -> cross-cohort validation. Strong clinical translation.
- **URL:** https://journals.lww.com/hep/abstract/2024/04000/spatial_proximity_of_tumor_immune_interactions.5.aspx

---

## 4. Multimodal spatial-omics reveal co-evolution of alveolar progenitors and proinflammatory niches in progression of lung precursor lesions

- **Authors:** Kadara H et al. (MD Anderson)
- **Journal:** Cancer Cell (2026), 44(2):321-339
- **Cancer type:** Lung precancer -> lung adenocarcinoma (LUAD)
- **Platforms:** Spatial transcriptomics (Visium-like)
- **Tools/methods:** Spatial transcriptomics, niche identification, ligand-receptor analysis, mouse validation models
- **Workflow:** Spatial transcriptomic mapping of 56 lesions (486K spots, 5.4M cells) -> alveolar progenitor identification -> proinflammatory niche characterization (IL1B-high macrophages) -> niche co-evolution tracking across disease stages -> therapeutic validation (IL-1beta inhibition + ICB in mice)
- **Key findings:** Epithelial-proinflammatory niches are prevalent in precancer but decrease in LUAD. Targeting IL-1beta signaling in precancerous stage disrupts transformation.
- **Cross-platform validation:** Discovery cohort (25 patients) + independent cohort (19 patients) + mouse models
- **Why it fits:** Exemplary end-to-end: niche discovery -> mechanistic biology -> therapeutic intervention validated in vivo. Cancer Cell.
- **URL:** https://www.cell.com/cancer-cell/fulltext/S1535-6108(25)00445-3

---

## 5. Integrative spatial omics reveals distinct tumor-promoting multicellular niches and immunosuppressive mechanisms in Black American and White American patients with TNBC

- **Authors:** (Nature Communications 2025 team)
- **Journal:** Nature Communications (2025)
- **Cancer type:** Triple-negative breast cancer (TNBC)
- **Platforms:** Imaging mass cytometry (IMC) + spatial transcriptomics
- **Tools/methods:** IMC cell phenotyping, spatial transcriptomics re-analysis, niche identification, cell-cell interaction mapping
- **Workflow:** IMC spatial protein profiling -> cell phenotyping -> spatial interaction mapping -> multicellular niche identification (race-specific niches) -> immunosuppressive mechanism characterization -> survival association
- **Key findings:** Black American TME enriched in endothelial-macrophage-mesenchymal niches (immunosuppressive), associated with reduced survival. White American TME has different niche composition.
- **Cross-platform validation:** IMC + spatial transcriptomics integration (multimodal)
- **Why it fits:** Multimodal spatial omics -> niche identification -> health disparity biology -> survival. Addresses clinical equity.
- **URL:** https://www.nature.com/articles/s41467-025-61034-3

---

## 6. Single-cell spatial multiomics reveals tumor microenvironment vulnerabilities in cancer resistance to immunotherapy

- **Authors:** (Duke team)
- **Journal:** Cell Reports (2024), 43(7):114392
- **Cancer type:** Metastatic melanoma
- **Platforms:** CITE-seq + 40-plex PhenoCycler tissue imaging
- **Tools/methods:** CITE-seq, PhenoCycler spatial imaging, multimodal integration toolkit, longitudinal analysis
- **Workflow:** Longitudinal multimodal profiling (pre/post therapy) -> transcriptomic + epitope + spatial integration -> TME characterization (immune-striving vs resistant) -> lymphoid aggregate identification -> melanoma subclone emergence tracking -> survival association
- **Key findings:** Peri-tumor lymphoid aggregates with B cell signatures associated with better survival. MITF+SPARCL1+ and CENPF+ melanoma subclones emerge after therapy.
- **Cross-platform validation:** CITE-seq + PhenoCycler (multimodal on same tumors)
- **Why it fits:** True multimodal (transcriptomic + protein + spatial), longitudinal, resistance mechanisms -> survival. Cell Reports.
- **URL:** https://www.cell.com/cell-reports/fulltext/S2211-1247(24)00720-4

---

## 7. High resolution mapping of the tumor microenvironment using integrated single-cell, spatial and in situ analysis

- **Authors:** (Nature Communications 2023 team)
- **Journal:** Nature Communications (2023), 14:8353
- **Cancer type:** Breast cancer (DCIS -> invasive carcinoma)
- **Platforms:** 10x Xenium + single-cell RNA-seq + additional spatial technologies
- **Tools/methods:** Xenium in situ, scRNA-seq, integrated spatial analysis
- **Workflow:** Multi-technology profiling of FFPE breast cancer sections -> cell type mapping -> molecular characterization of tumor regions -> boundary cell identification (myoepithelial border) -> progression biomarker discovery (DCIS -> invasive)
- **Key findings:** Identified rare boundary cells at the myoepithelial border confining malignant spread. Molecular differences between tumor regions characterized.
- **Cross-platform validation:** Multiple spatial + single-cell technologies on serial sections
- **Why it fits:** Multi-platform integration, progression biology, FFPE clinical samples. Good example of technology integration workflow.
- **URL:** https://www.nature.com/articles/s41467-023-43458-x

---

## 8. Spatial transcriptomics reveals distinct and conserved tumor core and edge architectures that predict survival and targeted therapy response

- **Authors:** Kumar M et al. (U Calgary)
- **Journal:** Nature Communications (2023), 14:5029
- **Cancer type:** Oral squamous cell carcinoma (OSCC), with pan-cancer validation (GBM, colorectal)
- **Platforms:** Spatial transcriptomics (Visium) + scRNA-seq
- **Tools/methods:** Spatial transcriptomics, scRNA-seq integration, ligand-receptor analysis, survival modeling
- **Workflow:** Spatial + single-cell profiling -> tumor core vs leading edge transcriptional architecture -> neighboring cell composition mapping -> ligand-receptor interaction analysis -> survival signature derivation -> cross-cancer validation -> drug sensitivity prediction
- **Key findings:** Leading edge signature conserved across cancers and associated with worse outcomes; core signature is tissue-specific with better prognosis. Drug sensitivity compartmentalized by distance from tumor core.
- **Cross-platform validation:** Cross-cancer type validation (OSCC, GBM, colorectal adenocarcinoma)
- **Why it fits:** Full pipeline from spatial architecture -> communication -> survival -> therapy response, validated across cancer types. Nat Commun.
- **URL:** https://www.nature.com/articles/s41467-023-40271-4

---

## 9. Combining spatial transcriptomics and ECM imaging in 3D for mapping cellular interactions in the tumor microenvironment

- **Authors:** Rajewsky lab (Max Delbruck Center)
- **Journal:** Cell Systems (2025), Article 101261
- **Cancer type:** Lung adenocarcinoma
- **Platforms:** CosMx spatial transcriptomics + second harmonic generation (SHG) imaging + immunofluorescence
- **Tools/methods:** CosMx, SHG ECM imaging, 3D alignment, neighborhood analysis, receptor-ligand interaction mapping
- **Workflow:** Serial section CosMx profiling -> SHG ECM composition imaging -> 3D alignment -> cellular neighborhood identification -> receptor-ligand interaction mapping in 3D -> immune escape and tumor invasion mechanism identification -> druggable target nomination
- **Key findings:** 3D multimodal atlas reveals immune escape and invasion mechanisms. ECM remodeling adds dimension beyond transcriptomics alone.
- **Cross-platform validation:** CosMx + SHG + IF on serial sections (multimodal)
- **Why it fits:** Novel 3D multimodal approach. Niche characterization with ECM context, druggable targets. Cell Systems.
- **URL:** https://www.cell.com/cell-systems/fulltext/S2405-4712(25)00094-8

---

## 10. Integrating 12 Spatial and Single Cell Technologies to Characterise Tumour Neighbourhoods and Cellular Interactions in three Skin Cancer Types

- **Authors:** (bioRxiv 2025, multi-institution)
- **Journal:** bioRxiv preprint (2025), PMC available
- **Cancer type:** Cutaneous squamous cell carcinoma (cSCC), basal cell carcinoma (BCC), melanoma
- **Platforms:** 12 technologies including spatial transcriptomics, proteomics, glycomics
- **Tools/methods:** Multi-platform integration, neighborhood analysis, ligand-receptor analysis (>700 cell-type combinations, >1.5M interactions)
- **Workflow:** 12-technology profiling -> cell signature identification (6 validated keratinocyte markers) -> cancer community discovery -> melanocyte-fibroblast-T-cell colocalization -> ligand-receptor analysis (CD44-FGF2 as therapeutic target) -> public atlas creation
- **Key findings:** CD44-FGF2 emerged as potential therapeutic target. Cancer communities enriched in specific cell colocalization patterns. Resource at skincanceratlas.com.
- **Cross-platform validation:** Extreme cross-platform validation (12 technologies). Strongest validation story of all candidates.
- **Why it fits:** Maximum cross-platform validation. Three cancer types. Niche -> communication -> therapeutic targets. Preprint but very comprehensive.
- **URL:** https://www.biorxiv.org/content/10.1101/2025.07.25.666708v1

---

## 11. NicheCompass: Quantitative characterization of cell niches in spatially resolved omics data

- **Authors:** Moinfar AA et al. (Helmholtz Munich / Wellcome Sanger)
- **Journal:** Nature Genetics (2025)
- **Cancer type:** Breast cancer + non-small cell lung cancer (NSCLC)
- **Platforms:** 10x Xenium (breast cancer, 286K cells) + NanoString CosMx (NSCLC, 800K cells)
- **Tools/methods:** NicheCompass (graph deep learning), communication pathway modeling
- **Workflow:** Spatial transcriptomics profiling -> graph deep learning cell embedding -> communication pathway-based niche identification -> tumor-immune interaction niche characterization -> cross-patient niche comparison -> neutrophil chemoattractant feature discovery
- **Key findings:** Identified conserved and patient-specific tumor-immune niches in NSCLC. Outperforms BANKSY, GraphST, CellCharter in spatial consistency. Cross-platform (Xenium + CosMx).
- **Cross-platform validation:** Xenium (breast) + CosMx (lung) -- different platforms, different cancers
- **Why it fits:** Tool paper but with substantial cancer biology application. Cross-platform niche characterization. Nature Genetics.
- **URL:** https://www.nature.com/articles/s41588-025-02120-6

---

## 12. Systematic benchmarking of high-throughput subcellular spatial transcriptomics platforms across human tumors

- **Authors:** (Nature Communications 2025 team)
- **Journal:** Nature Communications (2025)
- **Cancer type:** Colon adenocarcinoma, hepatocellular carcinoma, ovarian cancer
- **Platforms:** Stereo-seq v1.3, Visium HD FFPE, CosMx 6K, Xenium 5K, CODEX (protein), scRNA-seq
- **Tools/methods:** Multi-platform benchmarking, cell segmentation, spatial clustering, concordance analysis
- **Workflow:** 4-platform spatial transcriptomics + CODEX protein + scRNA-seq on same samples -> systematic comparison (sensitivity, specificity, segmentation, annotation, clustering) -> concordance with CODEX ground truth -> SPATCH visualization portal
- **Key findings:** Platform-specific strengths identified. CODEX protein data serves as ground truth. SPATCH portal for community access.
- **Cross-platform validation:** 4 spatial transcriptomics + 1 protein + 1 scRNA-seq platform (6 total). Maximum technical validation.
- **Why it fits:** Less end-to-end biology but critical for validating niche findings across platforms. Resource paper. If building a pipeline list, this is the validation reference.
- **URL:** https://www.nature.com/articles/s41467-025-64292-3

---

## 13. Single-cell and spatial transcriptomics implicate a prognostic function of tertiary lymphoid structures in gastric cancer

- **Authors:** (Nature Communications 2025 team)
- **Journal:** Nature Communications (2025)
- **Cancer type:** Gastric cancer
- **Platforms:** Spatial transcriptomics + scRNA-seq
- **Tools/methods:** TLS classification, spatial transcriptomics, scRNA-seq integration, prognostic signature derivation
- **Workflow:** Spatial + single-cell profiling -> TLS type classification -> transcriptomic differences by TLS type -> immune niche characterization (CXCL13, lymphotoxin-beta signaling) -> prognostic signature generation -> outcome prediction
- **Key findings:** TLS types have distinct transcriptomic profiles. TLS-based prognostic signature predicts gastric cancer outcomes. TLS acts as organized immune niches for antigen presentation.
- **Cross-platform validation:** Spatial + scRNA-seq integration
- **Why it fits:** End-to-end niche (TLS) -> biology (immune activation) -> prognosis. Gastric cancer is underrepresented in spatial omics.
- **URL:** https://www.nature.com/articles/s41467-025-65421-8

---

## 14. Spatial transcriptomics in breast cancer reveals tumour microenvironment-driven drug responses and clonal therapeutic heterogeneity

- **Authors:** (NAR Cancer 2024)
- **Journal:** NAR Cancer (2024), 6(4):zcae046
- **Cancer type:** Breast cancer
- **Platforms:** Spatial transcriptomics
- **Tools/methods:** Spatial transcriptomics, drug sensitivity modeling (1200+ drugs), clonal heterogeneity analysis
- **Workflow:** Spatial transcriptomics profiling -> TME characterization -> drug sensitivity prediction across >1200 drugs -> spatial compartmentalization of drug response -> clonal heterogeneity mapping -> therapeutic niche identification
- **Key findings:** Drug sensitivity gradients relative to tumor core distance. Intra-tumoral therapeutic heterogeneity with some niches showing decreased sensitivity to standard treatments. TME interactions trigger transcriptomic states affecting drug response.
- **Cross-platform validation:** Not explicitly multi-platform
- **Why it fits:** Unique angle: niche -> drug sensitivity prediction across 1200 drugs. Therapeutic heterogeneity is clinically actionable.
- **URL:** https://academic.oup.com/narcancer/article/6/4/zcae046/7928162

---

# Summary: Top Candidates by Selection Criteria

## Best for "full end-to-end workflow" (niche -> communication -> clinical):
1. **#1 Wang et al. Nature 2023** (TNBC, IMC, immunotherapy response)
2. **#4 Kadara et al. Cancer Cell 2026** (lung precancer, niche co-evolution, therapeutic validation)
3. **#8 Kumar et al. Nat Commun 2023** (OSCC pan-cancer, core/edge, survival + therapy)
4. **#2 Nature 2025** (HCC, NK cell niches, TIMES recurrence score)

## Best for cross-platform/multimodal validation:
1. **#10 Skin Cancer Atlas** (12 technologies, 3 cancer types)
2. **#12 Benchmarking study** (6 platforms, 3 cancer types)
3. **#11 NicheCompass** (Xenium + CosMx, 2 cancer types)
4. **#6 Cell Reports 2024** (CITE-seq + PhenoCycler, melanoma)

## Best for clinical translation:
1. **#1 Wang et al.** (clinical trial context, ICB prediction)
2. **#2 Nature 2025** (TIMES score, outperforms TNM/BCLC)
3. **#3 Hepatology 2024** (CODEX, 2 independent cohorts, survival)

## Best for biological mechanism:
1. **#4 Cancer Cell 2026** (niche co-evolution + mouse validation)
2. **#9 Cell Systems 2025** (3D + ECM, druggable targets)
3. **#5 Nat Commun 2025** (health disparity biology, TNBC)
