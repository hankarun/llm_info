# Reinforcement Learning from Human Feedback (RLHF)

## Overview

**Reinforcement Learning from Human Feedback (RLHF)** is a training technique that aligns language model outputs with human preferences. Rather than relying solely on labeled examples (SFT), RLHF uses human comparisons to train a **reward model**, which then guides the LLM using reinforcement learning.

RLHF was popularized by **InstructGPT** (Ouyang et al., OpenAI 2022) and is central to how ChatGPT, Claude, and Gemini are aligned.

## The RLHF Pipeline

### Step 1: Supervised Fine-Tuning (SFT)

A pretrained base model is fine-tuned on a curated dataset of prompt-response pairs written by human annotators. This gives the model a starting policy that can generate reasonable responses.

### Step 2: Reward Model Training

Human annotators compare multiple model responses for the same prompt and **rank** them by preference:

```
Prompt: "What is photosynthesis?"
Response A: [better]  ✓
Response B: [worse]
```

A **reward model (RM)** is trained to predict human preference scores. Typically:
- Same architecture as the SFT model (often with a scalar output head).
- Trained with a **pairwise ranking loss** (Bradley-Terry model):

```
L = -log σ(r(prompt, response_A) - r(prompt, response_B))
```

### Step 3: RL Fine-Tuning (PPO)

The SFT model (now called the **policy**) is fine-tuned using **Proximal Policy Optimization (PPO)** to maximize rewards from the reward model:

```
objective = E[r_RM(prompt, response)] - β * KL(π_RL || π_SFT)
```

The **KL divergence** penalty prevents the policy from drifting too far from the SFT baseline, which would lead to reward hacking (generating responses that score well on the RM but are actually low quality).

## Challenges with RLHF

- **Reward hacking** — Policy learns to exploit flaws in the reward model.
- **Reward model collapse** — RM becomes unreliable out-of-distribution.
- **Expensive** — Requires large amounts of human comparison data.
- **PPO instability** — RL training is notoriously sensitive to hyperparameters.
- **Annotator disagreement** — Human preferences are subjective and vary.

## Alternatives and Extensions

### Direct Preference Optimization (DPO)

DPO (Rafailov et al., 2023) eliminates the need for a separate reward model and RL loop. It directly optimizes the policy using preference pairs:

```
L_DPO = -E[log σ(β log(π_θ(y_w|x)/π_ref(y_w|x)) - β log(π_θ(y_l|x)/π_ref(y_l|x)))]
```

Where `y_w` = preferred response, `y_l` = rejected response.

DPO is simpler, stable, and achieves comparable or better results than PPO in many settings. Widely adopted by open-source models (Zephyr, Tulu 3, etc.).

### GRPO (Group Relative Policy Optimization)

Used in **DeepSeek-R1**. Instead of a separate reward model, generates multiple responses per prompt, scores them with a rule-based reward, and uses the group's relative scores to compute advantages. Eliminates the critic/value model, reducing memory.

### SimPO (Simple Preference Optimization)

A DPO variant that normalizes rewards by response length and eliminates the reference model, reducing memory requirements further.

### Constitutional AI (CAI)

Developed by **Anthropic** for **Claude**. Uses a set of principles ("constitution") to guide the model:

1. **Critique** — Model critiques its own response against constitutional principles.
2. **Revision** — Model revises the response.
3. **RLAIF** — AI-generated preference data (from the critique process) replaces human comparisons.

This scales alignment without requiring massive human labeling.

### RLAIF (RL from AI Feedback)

Use a stronger AI model (e.g., GPT-4) as the preference labeler instead of humans. Reduces cost while maintaining quality. Used in **Alpaca**, **Ultrafeedback**, and many open-source recipes.

## Preference Data Formats

Preference datasets consist of triplets:

```json
{
  "prompt": "Explain gravity.",
  "chosen": "Gravity is a fundamental force that attracts...",
  "rejected": "Gravity is when things fall down."
}
```

Common open-source preference datasets:
- **HH-RLHF** (Anthropic) — Helpfulness and harmlessness comparisons.
- **UltraFeedback** — GPT-4 rated responses to 64k instructions.
- **Preference 700K** — Aggregated from multiple sources.
- **StackExchange Preferences** — Code and technical Q&A with votes.

## Alignment Goals

RLHF and its variants aim to produce models that are:

- **Helpful** — Accurately answer questions, follow instructions.
- **Harmless** — Avoid producing harmful, offensive, or dangerous content.
- **Honest** — Acknowledge uncertainty; don't hallucinate confidently.

This **HHH** framework was articulated by Anthropic.

## Safety Considerations

Alignment is an active research area. Known failure modes:

- **Sycophancy** — Model agrees with the user even when wrong.
- **Specification gaming** — Optimizes a proxy reward rather than true preference.
- **Jailbreaking** — Adversarial prompts that bypass alignment.
- **Shallow alignment** — Fine-tuning may not deeply change model values; a few fine-tuning steps can often undo alignment.

## See Also

- [Fine-Tuning](fine-tuning.md)
- [Pretraining](pretraining.md)
- [Prompting Techniques](../features/prompting.md)
