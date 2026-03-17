# Claude Series

## Overview

**Claude** is a family of AI assistants developed by **Anthropic**, a safety-focused AI company founded in 2021 by former OpenAI researchers including Dario Amodei, Daniela Amodei, and others. Anthropic's mission is "the responsible development and maintenance of advanced AI for the long-term benefit of humanity."

Claude models are known for their **long context windows**, **strong instruction following**, **low hallucination rates**, and a unique safety approach called **Constitutional AI (CAI)**.

---

## Constitutional AI (CAI)

Anthropic's core alignment technique, described in *"Constitutional AI: Harmlessness from AI Feedback"* (Bai et al., 2022).

Instead of relying entirely on human feedback:
1. **A set of principles (constitution)** is defined (e.g., "be helpful, harmless, and honest").
2. The model **critiques and revises** its own outputs against these principles.
3. **AI-generated preference data** (RLAIF) is used to train the reward model.

This allows scalable alignment without requiring massive human labeling for every behavior.

---

## Claude 1 (March 2023)

- Anthropic's first public model.
- Available via API (limited access).
- Notable for helpfulness, longer context (9K tokens), and careful refusals.
- Two variants: **Claude** (capable) and **Claude Instant** (faster, cheaper).

---

## Claude 2 (July 2023)

- **Context:** 100,000 tokens — a landmark at the time.
- Improved coding, mathematics, and reasoning.
- Better at following complex multi-step instructions.
- Still two variants: **Claude 2** and **Claude Instant**.
- Introduced on Claude.ai (Anthropic's consumer product).

---

## Claude 3 (March 2024)

Three-tier model family:

| Model | Intelligence | Speed | Use case |
|---|---|---|---|
| **Claude 3 Haiku** | ↓ | Fastest | Cost-effective, near-instant tasks |
| **Claude 3 Sonnet** | ↑↑ | Balanced | General use, enterprise |
| **Claude 3 Opus** | Highest | Slower | Complex analysis, research |

- **Context:** 200,000 tokens across all three.
- **Multimodal:** All Claude 3 models accept image inputs.
- **Performance:** Claude 3 Opus surpassed GPT-4 on multiple benchmarks at release (MMLU, HumanEval, MATH).
- **Vision:** Can analyze documents, charts, photos, and screenshots.

---

## Claude 3.5 Sonnet (June 2024, October 2024)

- Significantly better than Claude 3 Opus despite being a "Sonnet" (mid-tier) model.
- **Coding:** Set new state-of-the-art on SWE-Bench (software engineering tasks) at release.
- **Computer Use (beta):** Claude 3.5 Sonnet can interact with computers — move the mouse, click, type — enabling autonomous computer control.
- Faster and cheaper than Claude 3 Opus.

---

## Claude 3.5 Haiku (November 2024)

- Fastest and most affordable Claude 3.5 model.
- Matches or exceeds Claude 3 Opus on many benchmarks.
- Designed for high-throughput production use cases.

---

## Claude 3.7 Sonnet (February 2025)

- Anthropic's **first hybrid reasoning model**.
- **Extended thinking mode:** The model can "think" for longer before answering, similar to OpenAI's o1.
- Thinking budget is configurable (token count).
- Significant improvements on coding, math, and multi-step reasoning.
- **Context:** 200K tokens.
- Maintains strong performance on instruction-following and writing tasks.

---

## Claude 4 Series (2025)

- **Claude Sonnet 4** and **Claude Opus 4** released in 2025.
- Further improvements in reasoning, coding, and agentic capabilities.
- Claude Opus 4 targets top performance for complex research and engineering tasks.

---

## Key Capabilities

### Long Context
Claude has consistently led the industry in context window size:
- Claude 2: 100K → Claude 3+: 200K tokens
- Strong recall and retrieval within the context ("needle-in-a-haystack" benchmarks).

### Coding
- Strong performance on SWE-Bench, HumanEval, and real-world engineering tasks.
- Used as the backbone for **Cursor**, **GitHub Copilot**, and other coding tools.

### Safety & Refusals
- More nuanced refusals than earlier models — avoids both over-refusal and unsafe outputs.
- Designed to refuse clearly harmful requests while being genuinely helpful.

### Computer Use
- Claude 3.5 Sonnet and later models can operate computers via screenshot + action loop.
- Enables automating GUI tasks without APIs.

---

## Claude.ai

Anthropic's consumer product at [claude.ai](https://claude.ai):
- Free tier: Claude 3.5 Haiku
- Pro tier: Access to Sonnet and Opus models; higher limits; Projects feature.
- **Projects:** Persistent context and custom instructions per project.
- **Artifacts:** Creates and previews code, documents, and visualizations in-browser.

---

## API Access

Available through [Anthropic's API](https://www.anthropic.com/api) with:
- Streaming, vision, tool use, and extended thinking.
- Also available via **Amazon Bedrock** and **Google Cloud Vertex AI**.

---

## See Also

- [GPT Series](gpt-series.md)
- [Gemini Series](gemini-series.md)
- [RLHF](../training/rlhf.md)
- [Tool Use & Function Calling](../features/tool-use.md)
