# Tokenization

## What is Tokenization?

**Tokenization** is the process of converting raw text into a sequence of discrete units called **tokens** that a language model can process. A token typically represents a word, sub-word, or character, depending on the tokenization scheme.

A model's **vocabulary** is the fixed set of all possible tokens. Every input is represented as a sequence of vocabulary indices.

## Why Not Just Use Words or Characters?

| Approach | Problem |
|---|---|
| **Word-level** | Huge vocabulary; unknown words (OOV); inflections treated as different words |
| **Character-level** | Very long sequences; poor semantic density per token |
| **Sub-word** | Best of both: compact vocab, handles rare/unseen words, semantic richness |

Most modern LLMs use **sub-word tokenization**.

## Common Tokenization Algorithms

### Byte-Pair Encoding (BPE)

Originally a data-compression algorithm, adapted for NLP (Sennrich et al., 2016) and used in **GPT-2, GPT-3, GPT-4, LLaMA, Mistral**, etc.

**Training algorithm:**
1. Start with a vocabulary of individual bytes (or characters).
2. Count all adjacent token pairs in the corpus.
3. Merge the most frequent pair into a new token.
4. Repeat until vocabulary size is reached.

**Inference:** Apply learned merge rules greedily to encode new text.

### WordPiece

Used in **BERT**, **RoBERTa**.

Similar to BPE but merges pairs that maximize the likelihood of the training data rather than raw frequency:

```
score(A, B) = freq(AB) / (freq(A) * freq(B))
```

Unknown tokens are represented with a `[UNK]` token; subword pieces are prefixed with `##`.

### Unigram Language Model

Used in **SentencePiece** (which in turn is used by **LLaMA**, **T5**, **mBART**).

Starts with a large candidate vocabulary and iteratively prunes tokens that minimize the loss of a unigram language model. Probabilistic — can produce multiple tokenizations with different probabilities.

### Byte-Level BPE

Operates on raw bytes instead of Unicode characters, so it can represent *any* byte sequence without UNK tokens. Used in **GPT-2, GPT-4, LLaMA (via SentencePiece byte-fallback)**.

## Vocabulary Size

| Model | Tokenizer | Vocab size |
|---|---|---|
| GPT-2 | BPE | 50,257 |
| GPT-4 / GPT-4o | tiktoken (BPE) | 100,277 |
| LLaMA 2 | SentencePiece (BPE) | 32,000 |
| LLaMA 3 | tiktoken (BPE) | 128,256 |
| Mistral | SentencePiece | 32,000 |
| Gemma 2 | SentencePiece | 256,000 |
| Qwen 2.5 | tiktoken | 151,643 |

Larger vocabularies improve token efficiency (fewer tokens per string) and multilingual coverage but increase embedding table size.

## Special Tokens

Most tokenizers add special tokens for model control:

| Token | Purpose |
|---|---|
| `<BOS>` / `<s>` | Beginning of sequence |
| `<EOS>` / `</s>` | End of sequence |
| `<PAD>` | Padding to batch sequences |
| `<UNK>` | Unknown token (BPE rarely uses this) |
| `[CLS]` | Classification token (BERT) |
| `[SEP]` | Separator (BERT) |
| `<\|im_start\|>` | Chat turn start (ChatML) |

## Tokenization Pitfalls

### Numbers and Arithmetic
Numbers are often split into multiple tokens, making arithmetic difficult:
- `"1234567"` → `["12", "34", "56", "7"]` (example, varies by tokenizer)

### Non-English Languages
Languages with different scripts (Chinese, Arabic, Hebrew) or agglutinative languages (Turkish, Finnish) are often less efficiently tokenized than English, requiring more tokens for the same amount of information.

### Whitespace Sensitivity
Many tokenizers are sensitive to leading/trailing spaces:
- `" hello"` and `"hello"` may map to different tokens.

### Tokenization != Word Boundaries
A single English word might become multiple tokens:
- `"unhappiness"` → `["un", "happiness"]` or `["un", "happin", "ess"]`

## Token Efficiency

**Tokens per word** is a useful proxy for tokenizer efficiency. English averages ~1.3 tokens/word with modern BPE vocabularies of ~100k tokens.

Larger vocabulary → fewer tokens per piece of text → longer effective context → lower inference cost.

## tiktoken

OpenAI's open-source tokenizer library. Fast (Rust core with Python bindings). Used with:
- `cl100k_base` — GPT-3.5, GPT-4
- `o200k_base` — GPT-4o, GPT-4o mini

```python
import tiktoken
enc = tiktoken.get_encoding("cl100k_base")
tokens = enc.encode("Hello, world!")
print(tokens)  # [9906, 11, 1917, 0]
```

## SentencePiece

Google's language-agnostic tokenizer library. Treats text as a raw stream of Unicode characters without pre-tokenization. Widely used in open-source models.

```python
import sentencepiece as spm
sp = spm.SentencePieceProcessor()
sp.load("tokenizer.model")
tokens = sp.encode("Hello, world!", out_type=str)
```

## See Also

- [Transformer Architecture](transformer.md)
- [Pretraining](../training/pretraining.md)
- [Context Windows](../features/context-window.md)
