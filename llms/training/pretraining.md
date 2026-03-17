# Pretraining

## What is Pretraining?

**Pretraining** is the initial large-scale training phase of an LLM in which the model learns general representations of language from a massive corpus of text data. The resulting **pretrained base model** (also called a **foundation model**) can be subsequently adapted for specific tasks via fine-tuning.

Pretraining accounts for the vast majority of compute cost in modern LLMs.

## Pretraining Objective

### Causal Language Modeling (CLM)

The dominant objective for decoder-only models (GPT, LLaMA, Mistral, etc.):

> **Predict the next token** given all previous tokens.

```
L = -sum_{t=1}^{T} log P(x_t | x_1, ..., x_{t-1})
```

This is also called **autoregressive language modeling**. The model is trained to minimize cross-entropy loss over the next token prediction across the entire training corpus.

### Masked Language Modeling (MLM)

Used by **BERT** and encoder-only models:

> **Predict masked tokens** given the surrounding (bidirectional) context.

~15% of tokens are randomly masked; the model must reconstruct them.

### Other Objectives
- **Span corruption** (T5) — Mask contiguous spans; predict entire spans.
- **Prefix language modeling** (UniLM) — Bidirectional attention on prefix, causal on suffix.

## Training Data

### Scale
Modern frontier models are trained on **trillions of tokens**. For reference:
- LLaMA 3 8B: 15T tokens
- LLaMA 3 70B: 15T tokens
- GPT-3: 300B tokens
- Chinchilla: 1.4T tokens (with 70B params)

### Common Data Sources

| Source | Examples | Notes |
|---|---|---|
| Web crawls | Common Crawl, C4, RefinedWeb | Largest volume; quality varies |
| Books | Project Gutenberg, Books3 | High-quality long-form text |
| Code | GitHub, The Stack | Improves reasoning and instruction following |
| Academic papers | arXiv, PubMed | Scientific reasoning |
| Wikipedia | All languages | Factual knowledge |
| Curated web | OpenWebText, FineWeb | Filtered for quality |
| Conversations | Reddit, forums | Dialogue style |

### Data Quality & Filtering

Raw web data contains noise, spam, and harmful content. Common filtering steps:

1. **Language identification** — Keep target languages.
2. **Deduplication** — Remove near-duplicate documents (MinHash, exact match).
3. **Quality filtering** — Heuristics: perplexity score, token ratio, line length, etc.
4. **Safety filtering** — Remove CSAM, toxic content.
5. **Domain weighting** — Oversample high-quality sources (books, Wikipedia, code).

### Data Mixture

The ratio of different domains significantly impacts model capability. Models trained on more code show stronger reasoning. Models trained on more multilingual data perform better on non-English tasks.

## Training Infrastructure

### Hardware
- **GPUs**: NVIDIA A100, H100, H200 — dominant choice.
- **TPUs**: Google's custom accelerators used for PaLM, Gemini.
- **Scale**: Frontier models train on 1,000–30,000+ GPUs for weeks to months.

### Parallelism Strategies

| Strategy | Description |
|---|---|
| **Data parallelism** | Same model on each GPU; different data shards. Gradients averaged. |
| **Tensor parallelism** | Split individual layers across GPUs. |
| **Pipeline parallelism** | Different layers on different GPUs; micro-batches flow through. |
| **Sequence parallelism** | Split the sequence dimension across GPUs. |
| **ZeRO (DeepSpeed)** | Shard optimizer states, gradients, and/or parameters across GPUs. |

Most large-scale training combines multiple strategies (3D or 4D parallelism).

### Mixed Precision Training

Training in **bfloat16** (BF16) or **float16** (FP16) reduces memory usage and speeds up computation. A **master copy** of weights is kept in float32 for numerical stability.

**FP8** training is increasingly used with H100 GPUs for further speedups.

### Optimizers

- **AdamW** — The standard optimizer; Adam with decoupled weight decay.
- **Adafactor** — Memory-efficient alternative; used in T5.
- **SOAP, Muon** — Newer optimizers showing promise at scale.

### Learning Rate Schedule

Typical pattern:
1. **Warmup** — Linear increase from 0 for ~1–2k steps.
2. **Cosine decay** — Gradual decrease to ~10% of peak LR.
3. **Final phase** — Some models do a short high-quality data cooldown.

## Compute Requirements

From the **Chinchilla scaling laws** (Hoffmann et al., 2022):

```
Optimal tokens = 20 * N   (where N = number of parameters)
Compute ≈ 6 * N * D      (FLOPs, where D = training tokens)
```

| Model | Params | Tokens | Approx. GPU-hours |
|---|---|---|---|
| GPT-3 | 175B | 300B | ~3.6M |
| LLaMA 2 70B | 70B | 2T | ~1.7M |
| LLaMA 3 70B | 70B | 15T | ~6M+ |

## See Also

- [Transformer Architecture](../architecture/transformer.md)
- [Fine-Tuning](fine-tuning.md)
- [RLHF](rlhf.md)
