# Fine-Tuning

## What is Fine-Tuning?

**Fine-tuning** is the process of further training a pretrained base model on a smaller, task-specific or instruction-focused dataset. The goal is to adapt the model's behavior — teaching it to follow instructions, adopt a particular style, or specialize in a domain — without repeating the expensive pretraining phase.

## Why Fine-Tune?

A pretrained base model is a powerful next-token predictor but not necessarily a helpful assistant. It may:
- Complete text in unexpected ways rather than answering questions.
- Not follow instructions reliably.
- Produce harmful or unwanted outputs.

Fine-tuning (especially **instruction tuning** and **RLHF**) bridges this gap.

## Types of Fine-Tuning

### Supervised Fine-Tuning (SFT)

The model is trained on labeled examples of the desired behavior, typically in an input/output format:

```
Input:  "Explain quantum entanglement in simple terms."
Output: "Quantum entanglement is when two particles become linked..."
```

Training uses the same **causal language modeling loss** as pretraining, but applied only to the output tokens (the input/instruction tokens are often masked from the loss).

**Data sources:**
- Human-written demonstrations (e.g., InstructGPT, Dolly)
- Model-generated data filtered by quality (e.g., Alpaca, WizardLM)
- Curated datasets (e.g., FLAN, OpenAssistant)

### Instruction Tuning

A form of SFT where examples span many diverse tasks expressed as natural language instructions. Developed by:
- **FLAN** (Google, 2021) — Fine-tuned T5 on 60+ NLP tasks.
- **InstructGPT** (OpenAI, 2022) — Combined SFT + RLHF.
- **Alpaca** (Stanford, 2023) — SFT on GPT-4 generated instructions.

### Domain Fine-Tuning

Adapt a general model to a specific field:
- Medical (Med-PaLM, BioGPT)
- Legal (LegalBERT)
- Code (Code Llama, StarCoder)
- Finance (FinGPT)

## Parameter-Efficient Fine-Tuning (PEFT)

Full fine-tuning updates all model weights — expensive for large models. PEFT methods update only a small fraction of parameters.

### LoRA (Low-Rank Adaptation)

The most widely used PEFT method (Hu et al., 2022).

For a frozen weight matrix `W ∈ R^{d×k}`, LoRA adds a trainable **low-rank decomposition**:

```
W' = W + B * A
```

Where `B ∈ R^{d×r}` and `A ∈ R^{r×k}`, with rank `r << min(d, k)`.

- Only A and B are trained (~0.1–1% of parameters).
- At inference, `W' = W + BA` can be merged — no added latency.
- Typical ranks: `r = 4, 8, 16, 64`.

**QLoRA** (Dettmers et al., 2023) — LoRA fine-tuning on a 4-bit quantized base model. Enables fine-tuning 70B+ models on a single consumer GPU.

### Adapter Layers

Small bottleneck layers inserted between Transformer sublayers. Only adapter parameters are trained. (Houlsby et al., 2019)

### Prefix Tuning / Prompt Tuning

Prepend trainable **soft prompt** vectors to the input:
- **Prefix tuning** — Trainable vectors prepended to keys and values in each layer.
- **Prompt tuning** — Trainable embedding vectors prepended to the input only.

### IA³

Rescales keys, values, and FFN activations with learned vectors. Very few parameters (~0.01%).

## Comparison of PEFT Methods

| Method | Trainable params | Inference overhead | Memory | Notes |
|---|---|---|---|---|
| Full fine-tune | 100% | None | High | Best quality |
| LoRA | ~0.1–1% | None (merge) | Low | Most popular |
| QLoRA | ~0.1–1% | None | Very low | 4-bit base |
| Adapter | ~1–5% | Small | Low | |
| Prompt tuning | < 0.01% | None | Minimal | |

## Chat / Instruction Format

Fine-tuning for chat requires a consistent **template** to structure turns. Common formats:

### ChatML (OpenAI)
```
<|im_start|>system
You are a helpful assistant.<|im_end|>
<|im_start|>user
Hello!<|im_end|>
<|im_start|>assistant
Hi! How can I help you today?<|im_end|>
```

### LLaMA 3
```
<|begin_of_text|><|start_header_id|>system<|end_header_id|>
You are a helpful assistant.<|eot_id|>
<|start_header_id|>user<|end_header_id|>
Hello!<|eot_id|>
<|start_header_id|>assistant<|end_header_id|>
```

## Catastrophic Forgetting

A risk of fine-tuning: the model may "forget" general capabilities while adapting to the new task. Mitigations:

- Use PEFT (LoRA) instead of full fine-tuning.
- Replay a small fraction of pretraining data.
- Elastic Weight Consolidation (EWC).
- Use a smaller learning rate.

## Data Considerations

- **Quality > Quantity** — A few thousand high-quality examples often outperform millions of noisy ones.
- **Diversity** — Cover varied tasks, styles, and domains.
- **Format consistency** — Consistent use of the chat template avoids confusion.
- **Deduplication** — Duplicate examples are overfit easily.

## See Also

- [Pretraining](pretraining.md)
- [RLHF](rlhf.md)
- [Prompting Techniques](../features/prompting.md)
