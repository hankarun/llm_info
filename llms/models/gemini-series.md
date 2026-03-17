# Gemini Series

## Overview

**Gemini** is a family of multimodal AI models developed by **Google DeepMind**, announced in December 2023. Gemini models are designed from the ground up to be **natively multimodal** — capable of understanding and generating text, images, audio, video, and code in a unified model.

Gemini succeeded Google's earlier **PaLM 2** model family and represents Google's primary frontier AI effort.

---

## Gemini 1.0 (December 2023)

**Paper:** *"Gemini: A Family of Highly Capable Multimodal Models"*

Three sizes:

| Variant | Description |
|---|---|
| **Gemini Ultra** | Largest and most capable; designed for complex tasks |
| **Gemini Pro** | Balanced; available via API |
| **Gemini Nano** | On-device; runs on Pixel 8 and Android |

- **Multimodal:** Text, images, audio, and video inputs from the start.
- **Context:** 32K tokens (Pro).
- **Performance:** Gemini Ultra surpassed GPT-4 on MMLU (90.0% vs 86.4%) using chain-of-thought prompting — the first model to do so.

---

## Gemini 1.5 (February 2024)

### Gemini 1.5 Pro

- **Context:** **1,000,000 tokens** — one million tokens, a dramatic breakthrough.
- Based on a **Mixture of Experts (MoE)** architecture.
- Can process ~1 hour of video, ~11 hours of audio, 30,000 lines of code, or 700,000 words.
- Strong "needle-in-a-haystack" recall across the full 1M context.
- Multimodal: text, image, audio, video, PDF, code.

### Gemini 1.5 Flash

- Lighter, faster, and more cost-effective than 1.5 Pro.
- 1M token context.
- Designed for high-volume, latency-sensitive applications.

---

## Gemini 2.0 (December 2024)

### Gemini 2.0 Flash

- **Multimodal input and output:** Can generate both text and images; native audio output.
- Improved speed and cost efficiency.
- Context: 1M tokens.
- **Agentic capabilities:** Grounding via Google Search, code execution, function calling.

### Gemini 2.0 Flash Thinking (Experimental)

- Reasoning model with visible thinking/chain-of-thought.
- Strong on AIME, GPQA, and other hard reasoning benchmarks.
- Comparable to o1 in many math and science tasks.

---

## Gemini 2.5 (2025)

### Gemini 2.5 Pro

- **Context:** 1M tokens (2M in preview).
- Top performance on coding benchmarks: #1 on **WebDev Arena** and **SWE-Bench Verified** at release.
- Strong mathematical and scientific reasoning.
- Integrated with **Gemini App**, **Google AI Studio**, and **Vertex AI**.
- Hybrid reasoning model: can activate extended thinking.

### Gemini 2.5 Flash

- Efficient variant of 2.5 Pro; fast and cost-effective.
- Best-in-class performance per token.

---

## Architecture Highlights

### Native Multimodality

Unlike models that bolt on vision (e.g., GPT-4V via CLIP), Gemini is pretrained on interleaved sequences of text, images, audio, and video from the start. This enables richer cross-modal reasoning.

### Mixture of Experts (MoE)

Gemini 1.5+ uses MoE, allowing efficient scaling: only a subset of parameters are activated per input token.

### Long Context

Google's research on efficient attention mechanisms (e.g., ring attention, memory-efficient implementations) enables Gemini's breakthrough context lengths.

---

## Products Built on Gemini

| Product | Gemini Model | Notes |
|---|---|---|
| **Gemini** (app) | 2.5 Pro/Flash | Consumer chatbot |
| **Google Search AI Overviews** | Gemini Flash | Search summaries |
| **Google Workspace** | Gemini 1.5 Pro | Docs, Gmail, Sheets |
| **Google AI Studio** | All models | Developer playground |
| **Vertex AI** | All models | Enterprise API |
| **NotebookLM** | Gemini | Research assistant |
| **Project Astra** | Gemini multimodal | Universal AI assistant |

---

## Google DeepMind Context

Gemini was developed after Google merged **Google Brain** and **DeepMind** into **Google DeepMind** in April 2023. This brought together teams responsible for:
- AlphaFold (protein structure prediction)
- AlphaCode (competitive programming)
- PaLM (Pathways Language Model)
- Imagen (image generation)

---

## Benchmark Performance (Gemini 2.5 Pro, 2025)

| Benchmark | Score | Notes |
|---|---|---|
| MMLU | ~90%+ | General knowledge |
| GPQA Diamond | ~84% | PhD-level science |
| AIME 2025 | ~86% | Hard math competition |
| SWE-Bench Verified | ~63% | Software engineering |
| HumanEval | ~90%+ | Code generation |

---

## See Also

- [GPT Series](gpt-series.md)
- [Claude Series](claude-series.md)
- [Transformer Architecture](../architecture/transformer.md)
- [Multimodal Models](../../generative-models/multimodal/overview.md)
