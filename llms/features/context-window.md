# Context Windows

## What is a Context Window?

The **context window** (also called **context length** or **maximum sequence length**) is the maximum number of tokens an LLM can process in a single forward pass — both the input prompt and the generated output combined.

Everything the model "knows" about a conversation, task, or document must fit within this window. Tokens that fall outside the window are simply not visible to the model.

## Why Context Length Matters

- **Long documents:** Research papers, books, codebases, legal contracts.
- **Long conversations:** Extended multi-turn chat without losing early context.
- **In-context learning:** More examples fit in the prompt.
- **Retrieval-augmented generation (RAG):** Can retrieve and use more documents directly.
- **Agent workflows:** Longer history of actions and observations.

## Context Length Evolution

| Model | Release | Context |
|---|---|---|
| GPT-2 | 2019 | 1,024 |
| GPT-3 | 2020 | 2,048 |
| GPT-3.5 Turbo | 2022 | 4,096 → 16,385 |
| GPT-4 | 2023 | 8,192 → 128,000 |
| Claude 2 | 2023 | 100,000 |
| Claude 3+ | 2024 | 200,000 |
| Gemini 1.5 Pro | 2024 | 1,000,000 |
| Gemini 2.5 Pro | 2025 | 1,000,000 (2M preview) |
| GPT-4.1 | 2025 | 1,000,000 |
| LLaMA 4 Scout | 2025 | 10,000,000 |

## KV Cache

During inference, the key and value tensors for each token in the context are stored in the **KV (Key-Value) cache**. This avoids recomputing attention for previous tokens at each generation step.

**Memory cost:**
```
KV cache size = 2 × layers × heads × head_dim × seq_len × bytes_per_element
```

For LLaMA 3 70B with 128K context in BF16:
- ~2 × 80 × 8 × 128 × 131072 × 2 ≈ **40 GB**

KV cache memory is often the bottleneck for long-context inference.

## Technical Challenges for Long Context

### Attention Complexity

Standard attention is O(n²) in time and memory. For n = 1M tokens, this is impractical.

Solutions:
- **FlashAttention** — IO-efficient exact attention.
- **Sliding window attention** — Local attention only.
- **Ring attention** — Distributed attention across GPUs.
- **Linear attention** — Approximate but O(n).

### Positional Encoding Generalization

Models trained with a fixed context length often fail to extrapolate to longer sequences. Solutions:

- **RoPE scaling** — Adjust the base frequency of RoPE:
  - **Position interpolation** — Linearly scale positions into trained range.
  - **YaRN** — Improved scaling with dynamic NTK interpolation.
  - **LongRoPE** — Progressive context extension.
- **ALiBi** — Designed for length extrapolation.

### "Lost in the Middle" Problem

Research (Liu et al., 2023) found that LLMs tend to recall information at the **beginning and end** of the context better than the **middle**. This is a fundamental challenge for long-context tasks.

Mitigations:
- Reranking retrieved documents to place most relevant near the start/end.
- Training with specific objectives that encourage middle-context utilization.
- Newer models (Gemini 1.5, Claude 3) show improved recall throughout.

## Context Window vs. Effective Context

Having a large context window doesn't mean the model uses it all effectively:

- **Attention pattern**: Models may learn to ignore distant tokens.
- **Evaluation**: "Needle-in-a-haystack" tests insert a specific fact deep in a document and test retrieval.
- **Practical performance** often degrades somewhat at very long inputs.

## Retrieval-Augmented Generation (RAG) vs. Long Context

| Approach | Context Window | RAG |
|---|---|---|
| Mechanism | Put all docs in context | Retrieve relevant chunks |
| Recall | High (if fits) | Depends on retrieval quality |
| Cost | High (more tokens) | Lower (fewer tokens) |
| Latency | Higher | Depends on retrieval |
| Dynamic data | Requires re-run | Can update retrieval index |
| Best for | Fixed, bounded docs | Large, dynamic corpora |

Long context complements RAG — retrieved chunks can be larger, and more results can be included.

## Context Window in Practice

### Token Counting

Use a tokenizer to count tokens before sending to an API:

```python
import tiktoken
enc = tiktoken.get_encoding("cl100k_base")
n_tokens = len(enc.encode(text))
```

### Chunking Strategies for Documents

When documents exceed the context window:
- **Fixed-size chunks** with overlap (simple; most common).
- **Sentence/paragraph splitting** (preserves semantics).
- **Recursive character splitting** (LangChain's default).
- **Semantic chunking** (split by embedding similarity).

## See Also

- [Attention Mechanisms](../architecture/attention-mechanism.md)
- [Prompting Techniques](prompting.md)
- [Tool Use & Function Calling](tool-use.md)
