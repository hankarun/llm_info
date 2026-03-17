# LLaMA Series

## Overview

The **LLaMA (Large Language Model Meta AI)** series is developed by **Meta AI**. It is the most widely used family of **open-weight** foundation models and has catalyzed a large ecosystem of fine-tuned variants, research, and derivative models.

All LLaMA models are **decoder-only** Transformers trained with **causal language modeling**.

---

## LLaMA 1 (February 2023)

**Paper:** *"LLaMA: Open and Efficient Foundation Language Models"* (Touvron et al., 2023)

- **Sizes:** 7B, 13B, 33B, 65B parameters
- **Context:** 2,048 tokens
- **Data:** ~1.4T tokens (CommonCrawl, C4, GitHub, Wikipedia, Books, ArXiv, StackExchange)
- **Architecture innovations:**
  - **Pre-normalization** with RMSNorm
  - **SwiGLU** activation
  - **Rotary Positional Embeddings (RoPE)**
- **Key insight:** A 65B model trained on more data can match or outperform GPT-3 (175B).
- **License:** Non-commercial research use only; weights leaked publicly.
- **Impact:** Triggered the open-source LLM ecosystem. Led directly to Alpaca, Vicuna, Koala, and many others.

---

## LLaMA 2 (July 2023)

**Paper:** *"Llama 2: Open Foundation and Fine-Tuned Chat Models"* (Touvron et al., 2023)

- **Sizes:** 7B, 13B, 34B, 70B
- **Context:** 4,096 tokens
- **Data:** ~2T tokens (new mix; more code, safety-focused filtering)
- **Architecture improvements:**
  - **Grouped Query Attention (GQA)** on 34B and 70B models.
  - Increased context from 2K to 4K.
- **Chat variants:** LLaMA 2-Chat — fine-tuned with SFT + RLHF (>1M human annotations).
- **License:** Free for commercial use (with restrictions for large-scale deployments).

---

## Code Llama (August 2023)

Fine-tuned from LLaMA 2 for code generation tasks:

- **Sizes:** 7B, 13B, 34B, 70B
- **Context:** Up to 100K tokens
- **Variants:**
  - **Code Llama** — General code completion.
  - **Code Llama - Python** — Python specialist.
  - **Code Llama - Instruct** — Instruction-following code model.

---

## LLaMA 3 (April 2024)

**Blog:** Meta AI, April 2024

- **Sizes:** 8B, 70B (instruct + base variants)
- **Context:** 8,192 tokens (base); extended to 128K in fine-tuned versions.
- **Data:** 15T tokens — one of the largest pretraining corpora for an openly released model.
  - 5% high-quality non-English data.
  - More code than LLaMA 2.
- **Tokenizer:** New tiktoken-based tokenizer with **128,256 vocabulary** (vs. 32,000 in LLaMA 2).
- **Architecture:** GQA on all sizes; 8B uses a larger FFN than LLaMA 2 7B.
- **Performance:** LLaMA 3 70B competitive with GPT-3.5 Turbo and earlier GPT-4 on many benchmarks.

### LLaMA 3.1 (July 2024)

- **Added:** 405B parameter model — the first truly GPT-4 competitive open-weight model.
- **Context:** Extended to **128K tokens** across all sizes (8B, 70B, 405B).
- **Tool use:** Native function calling support.
- **Multilingual:** Supports 8 languages officially.

### LLaMA 3.2 (September 2024)

- **Multimodal:** 11B and 90B **vision** models (image + text).
- **Small models:** 1B and 3B lightweight models for on-device deployment.
- **On-device:** 1B/3B designed to run on smartphones.

### LLaMA 3.3 (December 2024)

- **70B** model with **405B-level performance** due to improved training recipe.
- More efficient: near 405B quality at 70B size.

---

## LLaMA 4 (2025)

- **Architecture:** Mixture of Experts (MoE) across the lineup.
- **Models:** Scout (17B active / 109B total), Maverick (17B active / 400B total), Behemoth (2T+ total, in training).
- **Context:** Scout — 10M token context window.
- **Natively multimodal:** Text + image input.
- **Performance:** Maverick competitive with GPT-4o and Gemini 2.0 Flash.

---

## Key LLaMA-derived Models

The open-weight releases spawned a large ecosystem:

| Model | Base | Developer | Notes |
|---|---|---|---|
| Alpaca | LLaMA 1 | Stanford | Instruction-tuned with GPT-3.5 outputs |
| Vicuna | LLaMA 1 | LMSYS | Fine-tuned on ShareGPT conversations |
| Mistral 7B | Independent | Mistral AI | Sliding window attention, GQA |
| Mixtral 8x7B | Mistral | Mistral AI | MoE; 8 experts, 2 active |
| WizardLM | LLaMA | Microsoft | Evol-Instruct method |
| Orca 2 | LLaMA 2 | Microsoft | Improved reasoning via teacher distillation |
| Zephyr | Mistral | HuggingFace | DPO alignment |

---

## Model Access

- Weights available on [Meta's website](https://llama.meta.com) and [Hugging Face](https://huggingface.co/meta-llama).
- License: **LLaMA 3 Community License** — free for most commercial and research use; restrictions for services with >700M MAU.

## See Also

- [GPT Series](gpt-series.md)
- [Claude Series](claude-series.md)
- [Fine-Tuning](../training/fine-tuning.md)
- [Context Windows](../features/context-window.md)
