# Large Language Models — Overview

## What is a Large Language Model?

A **Large Language Model (LLM)** is a type of neural network trained on massive amounts of text data to understand and generate human language. LLMs are built on the **Transformer** architecture and are characterized by having billions (or even trillions) of parameters.

## Key Properties

| Property | Description |
|---|---|
| **Scale** | Billions to trillions of parameters |
| **Architecture** | Transformer-based (encoder, decoder, or encoder-decoder) |
| **Training data** | Trillions of tokens from the web, books, code, etc. |
| **Primary capability** | Next-token prediction (autoregressive) |
| **Emergent abilities** | Reasoning, in-context learning, instruction following |

## How LLMs Work (High Level)

1. **Tokenization** — Input text is split into tokens (sub-word units).
2. **Embedding** — Each token is mapped to a high-dimensional vector.
3. **Transformer layers** — Stacked self-attention and feed-forward layers process the sequence.
4. **Output head** — A linear layer + softmax produces a probability distribution over the vocabulary for the next token.
5. **Autoregressive decoding** — Tokens are sampled one at a time and appended to the context until a stopping criterion is met.

## Types of LLMs

### By Architecture
- **Decoder-only** — GPT series, LLaMA, Mistral, Gemma. Used for generation tasks.
- **Encoder-only** — BERT, RoBERTa. Used for classification and retrieval.
- **Encoder-decoder** — T5, BART. Used for translation and summarization.

### By Access
- **Proprietary/closed** — GPT-4o (OpenAI), Claude (Anthropic), Gemini (Google).
- **Open-weight** — LLaMA 3, Mistral, Falcon, Phi-3.

## Emergent Capabilities

At sufficient scale, LLMs exhibit capabilities that were not explicitly trained for:

- **In-context learning (ICL)** — Learning from examples provided in the prompt without weight updates.
- **Chain-of-thought (CoT) reasoning** — Step-by-step problem solving when prompted appropriately.
- **Instruction following** — Obeying natural language instructions after fine-tuning.
- **Code generation** — Writing and debugging code across many programming languages.
- **Multilingual transfer** — Handling languages underrepresented in training data.

## Current Frontier Models (as of 2025–2026)

| Model | Organization | Parameters (approx.) | Notes |
|---|---|---|---|
| GPT-4o | OpenAI | ~200B (est.) | Multimodal |
| Claude 3.7 Sonnet | Anthropic | — | Extended thinking |
| Gemini 2.5 Pro | Google DeepMind | — | Multimodal, long context |
| LLaMA 3.3 70B | Meta | 70B | Open-weight |
| Mistral Large 2 | Mistral AI | 123B | Open-weight |
| DeepSeek-V3 | DeepSeek | 671B (MoE) | Open-weight |

## Limitations

- **Hallucination** — Generating plausible-sounding but incorrect information.
- **Context length** — Limited memory within a single prompt (though rapidly expanding).
- **Stale knowledge** — Training data has a cutoff date; no real-time information by default.
- **Bias** — Reflects biases present in training data.
- **Reasoning depth** — Still struggle with complex multi-step mathematical reasoning without aids.

## See Also

- [Transformer Architecture](architecture/transformer.md)
- [Tokenization](architecture/tokenization.md)
- [Pretraining](training/pretraining.md)
- [Prompting Techniques](features/prompting.md)
