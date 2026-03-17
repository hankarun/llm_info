# Transformer Architecture

## Introduction

The **Transformer** was introduced in the paper *"Attention Is All You Need"* (Vaswani et al., 2017) and has become the dominant architecture for LLMs. It replaced recurrent networks (RNNs/LSTMs) with **self-attention**, enabling highly parallelizable training on large datasets.

## Core Components

### 1. Input Embedding + Positional Encoding

Each input token is mapped to a dense vector (embedding). Because attention has no built-in notion of order, **positional encodings** are added to convey sequence positions.

- **Sinusoidal positional encoding** (original paper) — Fixed, based on sine/cosine functions.
- **Learned positional embeddings** — Trained parameters (GPT-2 style).
- **Rotary Position Embedding (RoPE)** — Encodes relative positions; used in LLaMA, Mistral.
- **ALiBi** — Attention bias based on token distance; enables length extrapolation.

### 2. Multi-Head Self-Attention (MHSA)

Self-attention allows each token to attend to all other tokens in the sequence.

For a single attention head:

```
Attention(Q, K, V) = softmax(QK^T / sqrt(d_k)) * V
```

Where **Q** (query), **K** (key), and **V** (value) are linear projections of the input. `d_k` is the dimension of the key vectors.

**Multi-head attention** runs `h` attention heads in parallel, then concatenates and projects the outputs:

```
MultiHead(Q, K, V) = Concat(head_1, ..., head_h) * W_O
```

This lets the model attend to information from different representation subspaces simultaneously.

### 3. Feed-Forward Network (FFN)

Each Transformer layer contains a position-wise FFN applied independently to each token:

```
FFN(x) = activation(x * W_1 + b_1) * W_2 + b_2
```

Common activation functions:
- **ReLU** — Original paper.
- **GELU** — Used in GPT and BERT.
- **SwiGLU** — Used in LLaMA, PaLM; often improves performance.

The FFN hidden dimension is typically 4× the model dimension.

### 4. Layer Normalization

Normalizes activations to stabilize training. Two common placements:

- **Post-LN** — Original paper; applied after residual addition. Can be unstable at large scale.
- **Pre-LN** — Applied before the sublayer; more stable training. Used by most modern LLMs.

**RMSNorm** (Root Mean Square Layer Normalization) is a simpler alternative used in LLaMA and T5.

### 5. Residual Connections

Each sublayer (attention + FFN) wraps its output in a residual (skip) connection:

```
output = LayerNorm(x + Sublayer(x))
```

Residuals allow gradients to flow directly through the network, enabling very deep stacks.

## Encoder vs. Decoder vs. Encoder-Decoder

| Variant | Attention type | Examples | Typical use |
|---|---|---|---|
| **Encoder-only** | Bidirectional self-attention | BERT, RoBERTa | Classification, embeddings |
| **Decoder-only** | Causal (masked) self-attention | GPT, LLaMA, Mistral | Text generation |
| **Encoder-decoder** | Bidirectional encoder + cross-attention decoder | T5, BART | Translation, summarization |

In **causal (masked) attention**, each token can only attend to previous tokens (and itself), preventing the model from "seeing the future" during autoregressive generation.

## Modern Architectural Improvements

### Grouped Query Attention (GQA)

Reduces KV cache memory by sharing key/value heads across multiple query heads. Used in LLaMA 3, Mistral, Gemma.

- **MHA** — Each query head has its own K/V.
- **MQA** — All query heads share one K/V head.
- **GQA** — Query heads are grouped; each group shares one K/V head.

### Sliding Window Attention (SWA)

Each token attends only to a fixed window of recent tokens, reducing attention complexity from O(n²) to O(n·w). Used in Mistral.

### Mixture of Experts (MoE)

Replaces the dense FFN with multiple "expert" FFNs. A **router** selects the top-k experts per token. Only a fraction of parameters is active for each token, enabling efficient scaling.

Examples: Mixtral 8x7B, DeepSeek-V3, GPT-4 (rumored).

## Scaling Laws

From *"Scaling Laws for Neural Language Models"* (Kaplan et al., 2020) and the Chinchilla paper (Hoffmann et al., 2022):

- Model performance scales predictably with compute, parameters, and data.
- **Chinchilla optimal**: for a given compute budget, train a smaller model on more data rather than a large model on fewer tokens.
- Rule of thumb: **~20 tokens per parameter** for compute-optimal training.

## See Also

- [Attention Mechanisms](attention-mechanism.md)
- [Tokenization](tokenization.md)
- [Pretraining](../training/pretraining.md)
