# GPT Series

## Overview

The **GPT (Generative Pre-trained Transformer)** series is developed by **OpenAI**. It pioneered large-scale unsupervised pretraining followed by fine-tuning, and introduced the world to capable conversational AI with ChatGPT.

All GPT models use a **decoder-only** Transformer architecture trained with **causal language modeling**.

---

## GPT-1 (2018)

**Paper:** *"Improving Language Understanding by Generative Pre-Training"*

- **Parameters:** 117M
- **Layers:** 12
- **Context:** 512 tokens
- **Data:** BooksCorpus (~800M words)
- **Key insight:** Pretraining on unlabeled text + task-specific fine-tuning improves NLP benchmarks.

---

## GPT-2 (2019)

**Paper:** *"Language Models are Unsupervised Multitask Learners"*

- **Parameters:** 117M → 1.5B (4 sizes)
- **Context:** 1,024 tokens
- **Data:** WebText (~8M web pages, ~40GB)
- **Key insight:** A large enough language model is a **zero-shot multitask learner** — no task-specific fine-tuning needed.
- **Notable:** Initially not released in full due to misuse concerns (later released). Generated fluent, coherent long text.

---

## GPT-3 (2020)

**Paper:** *"Language Models are Few-Shot Learners"* (Brown et al.)

- **Parameters:** 175B
- **Context:** 2,048 tokens
- **Data:** ~300B tokens (Common Crawl, WebText2, Books, Wikipedia)
- **Key insights:**
  - **Few-shot in-context learning** — Provide examples in the prompt; no weight updates needed.
  - **Emergent capabilities** at scale.
- **Impact:** Demonstrated that scale alone unlocks powerful generalization.

---

## InstructGPT / ChatGPT (2022)

**Paper:** *"Training language models to follow instructions with human feedback"* (Ouyang et al.)

- Built on GPT-3 with **SFT + RLHF** (PPO).
- **ChatGPT** (Nov 2022) was the public-facing product; reached 100M users in 2 months.
- Key insight: RLHF-aligned models are far more helpful and less harmful than raw GPT-3 despite fewer parameters.

---

## GPT-4 (2023)

**Technical report:** *"GPT-4 Technical Report"* (OpenAI, 2023)

- **Architecture:** Details not fully disclosed; estimated ~1.7T parameters (MoE, 8 experts).
- **Context:** 8K tokens (later 128K with GPT-4 Turbo).
- **Capabilities:**
  - Passes bar exam (~90th percentile), SAT, AP exams.
  - Multimodal (image understanding via GPT-4V).
  - Strong coding (HumanEval ~67%).
  - Improved instruction following and safety.
- **Training:** GPT-4 was pretrained then aligned with RLHF.

---

## GPT-4o (2024)

- **o** = "omni" — natively multimodal (text, audio, image, video).
- End-to-end audio: processes audio directly without separate ASR/TTS stages, preserving tone and emotion.
- Faster and cheaper than GPT-4 Turbo.
- **Context:** 128K tokens.
- Available in ChatGPT (free tier) and API.

---

## GPT-4o mini (2024)

- Smaller, faster, cheaper version of GPT-4o.
- Replaces GPT-3.5 Turbo as the default efficient model.
- Strong performance on coding and reasoning relative to cost.

---

## o1 / o1-mini (2024)

- OpenAI's first **"reasoning model"** series.
- Uses **chain-of-thought (CoT) reasoning** at inference time via extended internal thinking.
- Significantly better on STEM benchmarks (AIME, GPQA, MATH) than GPT-4o.
- Slower than GPT-4o; optimized for complex, multi-step problems.
- **o1-mini** — Optimized for reasoning with smaller footprint.

---

## o3 / o3-mini (2025)

- Successor to o1 with significantly improved reasoning.
- **o3** achieves near-human performance on ARC-AGI (87.5%).
- **o3-mini** — Efficient variant; strong at math and coding.

---

## o4-mini / GPT-4.1 (2025)

- **GPT-4.1** — Improved coding, instruction following, and long-context (1M token context).
- **o4-mini** — Fast reasoning model; capable of using tools (web search, code execution) during chain-of-thought.

---

## Key Trends Across GPT Series

| Model | Year | Params | Context | Key advance |
|---|---|---|---|---|
| GPT-1 | 2018 | 117M | 512 | Pretraining + fine-tuning |
| GPT-2 | 2019 | 1.5B | 1,024 | Zero-shot learning |
| GPT-3 | 2020 | 175B | 2,048 | Few-shot in-context learning |
| ChatGPT/InstructGPT | 2022 | ~175B | 4,096 | RLHF alignment |
| GPT-4 | 2023 | ~1.7T (est.) | 128K | Multimodal, exam-level reasoning |
| GPT-4o | 2024 | — | 128K | Native multimodal (omni) |
| o1 | 2024 | — | 128K | Chain-of-thought reasoning |
| o3 | 2025 | — | 200K | Near-human reasoning |
| GPT-4.1 | 2025 | — | 1M | Long context, improved coding |

## See Also

- [LLaMA Series](llama-series.md)
- [Claude Series](claude-series.md)
- [RLHF](../training/rlhf.md)
- [Prompting Techniques](../features/prompting.md)
