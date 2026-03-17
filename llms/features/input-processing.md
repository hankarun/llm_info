# How an LLM Processes User Input

## Overview

When a user sends a message to an LLM, the input goes through a multi-stage pipeline before a response is generated. At no point does the model read raw text — everything is converted to numbers, processed through many layers of computation, and converted back to text at the end.

The full pipeline:

```
User text
   ↓  1. Input Formatting & Chat Template
Formatted prompt string
   ↓  2. Tokenization
Token ID sequence
   ↓  3. Token Embedding
Token vectors (floats)
   ↓  4. Positional Encoding
Position-aware vectors
   ↓  5. Transformer Forward Pass (N layers)
Contextual hidden states
   ↓  6. Output Head (logits)
Probability distribution over vocabulary
   ↓  7. Sampling / Decoding
Next token ID
   ↓  (repeat steps 3–7 until <EOS>)
   ↓  8. Detokenization
Response text
```

---

## Step 1: Input Formatting & Chat Template

Modern LLMs are fine-tuned on conversations structured with a **chat template**. Raw user text is wrapped in a template that adds special tokens and role markers before anything else happens.

### Chat Roles

A typical conversation has three roles:

| Role | Purpose |
|---|---|
| **system** | Sets the model's persona, constraints, or task context |
| **user** | The human's message |
| **assistant** | The model's previous turns (for multi-turn conversations) |

### Common Chat Template Formats

**ChatML** (used by OpenAI models and many open-source models):

```
<|im_start|>system
You are a helpful assistant.<|im_end|>
<|im_start|>user
What is the capital of France?<|im_end|>
<|im_start|>assistant
```

**Llama 3 format:**

```
<|begin_of_text|><|start_header_id|>system<|end_header_id|>

You are a helpful assistant.<|eot_id|><|start_header_id|>user<|end_header_id|>

What is the capital of France?<|eot_id|><|start_header_id|>assistant<|end_header_id|>
```

**Mistral/Alpaca instruct format:**

```
[INST] What is the capital of France? [/INST]
```

The formatted string — including all special tokens and prior conversation history — becomes the model's full input, often called the **prompt**.

> **Why this matters:** The model was trained on conversations in this exact format. Using the wrong template causes degraded performance because the model has learned to associate role boundaries with the special tokens it was fine-tuned on.

---

## Step 2: Tokenization

The formatted prompt string is converted into a sequence of **token IDs** — integers that index into the model's fixed vocabulary.

### What is a Token?

A token is typically a sub-word unit. The word `"unhappiness"` might become:

```
["un", "happiness"]  →  [1726, 25763]
```

A single space, punctuation mark, or even a partial word can be its own token.

### Process

1. **Byte-Pair Encoding (BPE)** or another sub-word algorithm splits the text.
2. Each piece is looked up in the vocabulary dictionary → integer ID.
3. Special tokens (BOS, EOS, role markers) are inserted at defined positions.

**Example:**

```
Input:  "<|im_start|>user\nWhat is 2+2?<|im_end|><|im_start|>assistant\n"
Tokens: [198, 882, 198, 3923, 374, 220, 17, 10, 17, 30, 198, 882, 198]
         ^                                                             ^
         BOS-like marker                                               start of assistant turn
```

For more detail, see [Tokenization](../architecture/tokenization.md).

---

## Step 3: Token Embedding

Each token ID is mapped to a **dense vector** (embedding) via a learned **embedding matrix** `E` of shape `[vocab_size × d_model]`:

```
token_id  →  E[token_id]  →  vector of shape [d_model]
```

For a sequence of `n` tokens, this produces a matrix of shape `[n × d_model]`. For GPT-4 class models, `d_model` is typically 4,096–12,288.

These embeddings encode **semantic meaning**: words with similar meanings have vectors that are close together in the high-dimensional space. However, at this stage the vectors have no sense of *where* each token appears in the sequence — that comes next.

---

## Step 4: Positional Encoding

Self-attention is **permutation-invariant** — it processes all tokens simultaneously and does not inherently know their order. Positional information is injected by adding or incorporating a position signal into each token's vector.

### Rotary Position Embedding (RoPE)

Used in LLaMA, Mistral, Qwen, GPT-4o, and most modern models. Instead of adding a positional vector, RoPE **rotates** the query and key vectors in the attention computation by an angle proportional to their absolute position. This naturally encodes relative distances between tokens.

### Learned Absolute Positional Embeddings

A trainable vector `P[i]` is added to the token embedding at position `i`:

```
input[i] = E[token_id[i]] + P[i]
```

Used in the original Transformer and GPT-2.

### Sinusoidal Positional Encoding

Fixed (non-learned) vectors based on sine and cosine functions of different frequencies. From the original *"Attention Is All You Need"* paper.

After this step, each vector in the `[n × d_model]` matrix carries both **what** the token is and **where** it is in the sequence.

---

## Step 5: Transformer Forward Pass

The position-aware token vectors pass through `L` stacked **Transformer decoder layers** (typically 32–96 for large models). Each layer refines the representation of every token in light of all other tokens.

### Inside a Single Decoder Layer

```
x  →  LayerNorm  →  Causal Self-Attention  →  (+residual)  →  LayerNorm  →  FFN  →  (+residual)  →  output
```

#### 5a. Layer Normalization (Pre-LN)

Normalizes each token's vector before the sublayer to stabilize training:

```
x_norm = LayerNorm(x)   or   RMSNorm(x)
```

#### 5b. Causal Self-Attention

Each token computes **queries** (Q), **keys** (K), and **values** (V) by projecting its vector through learned weight matrices:

```
Q = x · W_Q,   K = x · W_K,   V = x · W_V
```

Attention scores are computed between every pair of tokens:

```
Attention(Q, K, V) = softmax( QK^T / sqrt(d_k) + mask ) · V
```

The **causal mask** sets all future positions to `-inf` before the softmax, ensuring token `i` can only attend to tokens at positions `≤ i`. This enforces that the model cannot "cheat" by looking ahead during generation.

Multiple heads run in parallel (**Multi-Head Attention**), each learning to focus on different aspects of the sequence (e.g., syntax, coreference, long-range dependencies).

The output of all heads is concatenated and projected:

```
MultiHead(Q, K, V) = Concat(head_1, ..., head_h) · W_O
```

A **residual connection** adds the original `x` back:

```
x = x + MultiHead(...)
```

#### 5c. Feed-Forward Network (FFN)

Applied independently to each token's vector after attention:

```
FFN(x) = activation(x · W_1 + b_1) · W_2 + b_2
```

The hidden dimension is typically `4 × d_model`. The FFN is where much of the factual knowledge learned during training is believed to be stored. Another residual connection follows.

#### 5d. KV Cache (Inference Optimization)

During generation, the K and V vectors for all previously processed tokens are **cached**. When generating the next token, only the new token needs to be processed through attention; cached K/V pairs are reused. This reduces per-step inference cost from O(n²) to O(n).

After passing through all `L` layers, each token has a rich **contextual representation** that reflects the full meaning of the sequence up to that point.

---

## Step 6: Output Head — Logits

Only the **last token's** hidden state is used to predict the next token. It is passed through a linear layer (the **language model head**) that projects it back up to the full vocabulary size:

```
logits = hidden_state[-1] · W_lm_head    shape: [vocab_size]
```

Each value in `logits` is a raw score for the corresponding vocabulary token. A **softmax** converts these to probabilities:

```
P(token_i) = exp(logit_i) / Σ exp(logit_j)
```

The result is a probability distribution over the entire vocabulary — the model's belief about which token should come next given the entire context.

---

## Step 7: Sampling / Decoding

A single token is selected from the probability distribution. The strategy used here significantly affects the character of the output.

### Temperature Scaling

Before sampling, the logits are divided by a **temperature** parameter `T`:

```
P(token_i) ∝ exp(logit_i / T)
```

| Temperature | Effect |
|---|---|
| `T → 0` | Deterministic — always picks the highest-probability token (greedy) |
| `T = 1.0` | Unchanged distribution |
| `T > 1.0` | Flatter distribution — more random and creative output |

### Greedy Decoding

Always select the single most probable token:

```
token = argmax(logits)
```

Fast and deterministic, but can produce repetitive or suboptimal sequences.

### Top-k Sampling

Keep only the `k` most probable tokens, renormalize, then sample:

```
top_k_tokens = argsort(logits, descending=True)[:k]
token = sample(top_k_tokens, probabilities)
```

Typical values: `k = 40–100`.

### Top-p (Nucleus) Sampling

Keep the smallest set of tokens whose cumulative probability ≥ `p`, then sample:

```
sorted_tokens = argsort(logits, descending=True)
cumulative_probs = cumsum(softmax(sorted_tokens))
nucleus = sorted_tokens[cumulative_probs ≤ p]
token = sample(nucleus, probabilities)
```

Typical values: `p = 0.9–0.95`. Nucleus sampling adapts the candidate pool size dynamically — tight when the model is confident, broad when it is uncertain.

### Beam Search

Maintains `b` candidate sequences (beams) simultaneously, selecting the top-`b` continuations at each step and returning the highest-scoring complete sequence. Used in translation and summarization tasks where deterministic, high-quality output is preferred over diversity.

### Min-p Sampling

A newer alternative (2023): tokens with probability below `min_p × max_probability` are filtered out. Adapts more naturally to the model's confidence level than top-k.

---

## Step 8: Autoregressive Loop

Steps 3–7 are repeated for each newly generated token. The new token is **appended to the context** and the model runs another forward pass — but only for the new token, since previous K/V vectors are cached.

```
while not done:
    token = forward_pass(context)[-1]   # get next token
    if token == EOS:
        break
    context.append(token)               # add to sequence
```

Generation stops when:
- An **end-of-sequence (EOS)** token is sampled.
- A **maximum token limit** is reached.
- A **stop sequence** (user-defined string) is detected in the output.

---

## Step 9: Detokenization

The sequence of output token IDs is decoded back into a human-readable string by the tokenizer:

```python
output_ids = [3510, 374, 220, 19, 13]
output_text = tokenizer.decode(output_ids)
# → "The answer is 4."
```

Special tokens (EOS, role markers) are stripped. The resulting string is returned to the user.

---

## End-to-End Example

**User input:** `"What is 2 + 2?"`

| Stage | Data |
|---|---|
| **Raw input** | `"What is 2 + 2?"` |
| **After chat template** | `"<\|im_start\|>user\nWhat is 2 + 2?<\|im_end\|>\n<\|im_start\|>assistant\n"` |
| **After tokenization** | `[100264, 882, 198, 3923, 374, 220, 17, 489, 220, 17, 30, 100265, 198, 100264, 78191, 198]` |
| **Embedding** | Matrix of shape `[16 × 4096]` (floating-point vectors) |
| **After transformer (32 layers)** | Matrix of shape `[16 × 4096]` (contextualized representations) |
| **Logits for next token** | Vector of shape `[100277]` (one score per vocabulary token) |
| **Top candidates** | `"The"` (12%), `"2"` (9%), `"It"` (7%), … |
| **Sampled token** | `"The"` (token ID 791) |
| **… (continues)** | Generates `"answer"`, `"is"`, `"4"`, `"."` one token at a time |
| **Final output** | `"The answer is 4."` |

---

## Summary

| Step | Input | Output |
|---|---|---|
| Chat template | Raw user text | Formatted prompt string |
| Tokenization | Formatted string | Integer token ID sequence |
| Embedding | Token IDs | Float vectors `[n × d_model]` |
| Positional encoding | Float vectors | Position-aware vectors |
| Transformer (L layers) | Position-aware vectors | Contextual hidden states |
| Output head | Last hidden state | Logit vector `[vocab_size]` |
| Sampling | Logit vector | Single next-token ID |
| Autoregressive loop | New token | Grows context; repeat |
| Detokenization | Output token IDs | Response text |

---

## See Also

- [Tokenization](../architecture/tokenization.md)
- [Transformer Architecture](../architecture/transformer.md)
- [Attention Mechanisms](../architecture/attention-mechanism.md)
- [Context Windows](context-window.md)
- [Prompting Techniques](prompting.md)
