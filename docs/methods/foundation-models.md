# Foundation Models for Spatial Niches

**Niche definition: learned representation from large-scale pre-training.**

Foundation models learn what a niche is from data rather than imposing a definition. They are trained on large datasets and produce cell embeddings that incorporate spatial context. The niche definition is implicit in the learned representation.

## Key Methods

### Nicheformer

- **Paper:** [Nature Methods, 2025](https://doi.org/10.1038/s41592-025-02610-7)
- **Code:** [github.com/theislab/nicheformer](https://github.com/theislab/nicheformer)
- **Niche definition:** Transformer-based model trained on both dissociated and spatial single-cell data; learns niche-aware cell embeddings by incorporating spatial neighborhood context during pre-training.
- **Key innovation:** A 49M-parameter model that beats the 444M TranscriptFormer on spatial tasks, demonstrating that incorporating spatial context during training matters more than parameter count.
- **Strengths:** Pre-trained on diverse tissues, spatial-aware by design (not adapted from non-spatial models), produces embeddings useful for multiple downstream tasks.
- **Limitations:** Niche definition is opaque — difficult to interpret what the model has learned to define as "niche." The learned representation may not align with any specific biological concept of niche.
- **Verdict:** The first foundation model designed specifically for spatial single-cell data. The architecture vs scale lesson is important for the field.

### scNiche

- **Paper:** [Nature Communications, 2025](https://doi.org/10.1038/s41467-025-56090-0)
- **Code:** [github.com/wanglabtongji/scNiche](https://github.com/wanglabtongji/scNiche)
- **Niche definition:** Graph attention network that produces niche-aware cell embeddings by attending to spatial neighbors with learned importance weights.
- **Key innovation:** Combines graph attention (which cells to attend to) with expression modeling for integrated niche representation.
- **Strengths:** Attention weights provide some interpretability — you can see which neighbors the model considers important for each cell.
- **Limitations:** Smaller pre-training scale than Nicheformer.

### Novae

- **Paper:** [Nature Methods, 2025](https://doi.org/10.1038/s41592-025-02624-1)
- **Code:** [github.com/MICS-Lab/novae](https://github.com/MICS-Lab/novae)
- **Niche definition:** Graph-based foundation model that learns spatial domain and niche representations via self-supervised learning on spatial graphs.
- **Key innovation:** Zero-shot inference — can identify spatial domains and niches on new datasets without retraining. Works across technologies (Visium, MERFISH, Xenium).
- **Strengths:** No fine-tuning needed for new tissues or platforms. Broad pre-training dataset.
- **Limitations:** Zero-shot performance may be lower than fine-tuned specialist models on specific tissues. Like other foundation models, the niche definition is implicit.

Note: ONTraC, previously listed here, is a niche trajectory method rather than a foundation model. See the README's Niche-Aware Downstream Analysis section.

## The Interpretability Challenge

Foundation models face a fundamental tension with the thesis of this site: every method defines "niche" differently, and understanding that definition is key to choosing the right tool. Foundation models define niches implicitly through their learned representations, making it difficult to know exactly what biological concept of "niche" they have captured.

This is not necessarily a weakness — if the learned definition captures biologically meaningful structure, it may be more accurate than any hand-crafted definition. But it means researchers must validate foundation model niches against known biology rather than relying on the definition itself to guide interpretation.

## When to Use Foundation Models

**Best for:**

- Large-scale analyses across many samples or tissues where pre-trained embeddings save compute.
- Exploratory analysis where you want the data to define niche structure without imposing assumptions.
- Transfer learning — applying a model trained on well-annotated data to new datasets.

**Not ideal for:**

- When you need an interpretable niche definition for a specific biological hypothesis.
- Small datasets where pre-training advantages are minimal.
- Tissues or conditions poorly represented in the training data.
