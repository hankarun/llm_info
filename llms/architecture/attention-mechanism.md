# Attention Mechanisms

## Introduction

**Attention** is the core operation in the Transformer architecture. It allows a model to dynamically weight the relevance of different tokens in a sequence when computing a representation for any given token.

## Scaled Dot-Product Attention

The fundamental attention operation:

```
Attention(Q, K, V) = softmax(QK^T / sqrt(d_k)) · V
```

- **Q (Query)** — "What am I looking for?"
- **K (Key)** — "What do I contain?"
- **V (Value)** — "What do I return if selected?"
- **d_k** — Dimension of key vectors; used for scaling to prevent vanishing gradients from the softmax.

### Steps
1. Compute dot products between Q and all K vectors → **attention scores**.
2. Scale by `1/sqrt(d_k)`.
3. Apply softmax to get **attention weights** (sum to 1).
4. Weighted sum of V vectors → **context vector**.

## Multi-Head Attention (MHA)

Run `h` attention heads in parallel with separate learned projections:

```
head_i = Attention(Q * W_Q_i, K * W_K_i, V * W_V_i)
MultiHead(Q, K, V) = Concat(head_1, ..., head_h) * W_O
```

Each head learns to attend to different aspects of the input (syntax, semantics, long-range dependencies, etc.).

## Self-Attention vs. Cross-Attention

| Type | Q source | K/V source | Used in |
|---|---|---|---|
| **Self-attention** | Same sequence | Same sequence | Encoder & Decoder |
| **Cross-attention** | Decoder | Encoder output | Encoder-Decoder models |

## Causal (Masked) Self-Attention

In decoder-only models, each token must only attend to **previous** tokens to preserve the autoregressive property. This is implemented via an **attention mask** that sets future positions to `-inf` before the softmax.

```
mask_{i,j} = 0 if j ≤ i, else -inf
```

## KV Cache

During **inference**, the key and value vectors of all previously generated tokens are cached and reused. This avoids recomputing them at each generation step, reducing inference from O(n²) to O(n) per step.

Memory grows linearly with sequence length, which is the primary memory bottleneck during long-context inference.

## Variants and Improvements

### Multi-Query Attention (MQA)

All query heads share **one** key-value head. Reduces KV cache size by a factor of `h` (number of heads).
- Used in: PaLM, Falcon.

### Grouped-Query Attention (GQA)

Query heads are divided into `g` groups; each group shares one K/V head. A middle ground between MHA and MQA.
- Used in: LLaMA 3, Mistral, Gemma 2, GPT-4o.

### Flash Attention

An **IO-aware** exact attention algorithm (Dao et al., 2022) that tiles computation to avoid materializing the full N×N attention matrix in HBM (high-bandwidth memory), dramatically reducing memory usage and speeding up training and inference.

- **FlashAttention-2** (2023) — Further parallelism improvements.
- **FlashAttention-3** (2024) — Optimized for Hopper (H100) GPUs.

### Sparse Attention

Limits each token to attend to only a subset of positions:

- **Local / sliding window** — Attends to a fixed window of nearby tokens. Used in Longformer, Mistral (SWA).
- **Strided** — Attends to every k-th token.
- **Global + local** — Special tokens attend globally; others attend locally.

### Linear Attention

Replaces the softmax with a kernel approximation to reduce complexity from O(n²) to O(n):

```
Attention(Q, K, V) ≈ φ(Q) (φ(K)^T V)
```

Examples: Linformer, Performer, RetNet.

### Ring Attention

Distributes the attention computation across multiple devices in a ring topology, enabling sequences of virtually unlimited length across a cluster.

## Positional Information in Attention

Standard dot-product attention is **permutation-invariant** — it has no inherent notion of position. Position must be injected:

- **Absolute positional embeddings** — Added to input embeddings.
- **Relative positional encodings** — Modify attention scores based on token distance (e.g., T5 biases).
- **RoPE (Rotary Position Embedding)** — Rotates Q/K vectors by their position angle; naturally captures relative positions. Used in LLaMA, Mistral, Qwen, etc.
- **ALiBi** — Subtracts a position-dependent bias from attention scores; enables length extrapolation without retraining.

## Attention Complexity

| Method | Time complexity | Space complexity |
|---|---|---|
| Standard attention | O(n²d) | O(n²) |
| FlashAttention | O(n²d) | O(n) |
| Sparse attention | O(n·w·d) | O(n·w) |
| Linear attention | O(nd²) | O(nd) |

Where `n` = sequence length, `d` = model dimension, `w` = window size.

## See Also

- [Transformer Architecture](transformer.md)
- [Context Windows](../features/context-window.md)
