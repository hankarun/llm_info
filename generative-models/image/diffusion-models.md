# Diffusion Models

## Overview

**Diffusion models** are a class of generative models that learn to reverse a gradual **noising process**. They are currently the dominant approach for high-quality image (and increasingly audio and video) generation, powering DALL-E 3, Midjourney v6, Stable Diffusion, Imagen, and many others.

## Core Idea

A diffusion model learns two processes:

1. **Forward process (diffusion):** Gradually add Gaussian noise to a clean data sample over `T` steps until it becomes pure noise.
2. **Reverse process (denoising):** Learn to remove noise step by step, reconstructing the original data from noise.

At inference, start from pure Gaussian noise and apply the learned reverse process to generate new samples.

## Mathematical Framework

### Forward Process

At each step `t`, add small Gaussian noise:

```
q(x_t | x_{t-1}) = N(x_t; sqrt(1-β_t) * x_{t-1}, β_t * I)
```

Where `β_t` is a small noise schedule value. After `T` steps (typically T=1000), `x_T ≈ N(0, I)`.

By reparameterization, we can sample any timestep directly:

```
x_t = sqrt(ᾱ_t) * x_0 + sqrt(1 - ᾱ_t) * ε    (where ε ~ N(0, I))
```

### Reverse Process (Denoising)

A neural network `ε_θ(x_t, t)` is trained to predict the noise added at step `t`:

```
L = E[||ε - ε_θ(x_t, t)||²]
```

This is the **denoising score matching** objective (Ho et al., 2020, DDPM).

## Architectures

### U-Net (Original DDPM)

The first successful architecture for diffusion models:
- **Encoder-decoder** with skip connections (U-shaped).
- **Residual blocks** at each resolution.
- **Self-attention layers** at lower resolutions for global context.
- **Time embedding** — sinusoidal encoding of timestep `t` added to each layer.

### DiT (Diffusion Transformer)

Peebles & Xie, 2023 — replace the U-Net with a **Transformer**:
- Input image patches are tokenized (like ViT).
- Standard Transformer blocks with adaptive layer norm conditioned on `t` and class label.
- Scales better than U-Net with model size.
- Used in **Stable Diffusion 3**, **FLUX**, **Sora** (video), **Imagen 3**.

## Key Variants

### DDPM (Denoising Diffusion Probabilistic Models)
Ho et al., 2020. The foundational paper. T=1000 steps. High quality but slow.

### DDIM (Denoising Diffusion Implicit Models)
Song et al., 2020. Deterministic sampling with fewer steps (~50). No quality loss. Enables interpolation in latent space.

### Score-Based Generative Models
Song et al., 2021. Unified framework using stochastic differential equations (SDEs). Generalizes DDPM and other variants.

## Latent Diffusion Models (LDM)

**Rombach et al., 2022.** Key insight: diffusion in **latent space** is much cheaper than in pixel space.

1. Train a **VAE** to encode images into a compressed latent representation (typically 8-16× smaller).
2. Run the diffusion process in latent space.
3. Decode latents back to images with the VAE decoder.

**Stable Diffusion** is an LDM. This innovation made diffusion models practical on consumer GPUs.

## Text-Conditioned Image Generation

To generate images from text descriptions, the model is **conditioned** on text embeddings:

1. **Text encoder** (usually CLIP or T5) encodes the text prompt to an embedding.
2. The denoising U-Net/DiT takes both `x_t` and the text embedding as input.
3. **Classifier-Free Guidance (CFG)** improves prompt adherence:

```
ε_guided = ε_uncond + w * (ε_cond - ε_uncond)
```

Higher `w` (guidance scale) → stronger prompt adherence but less diversity (and possible artifacts).

## Major Models

### Stable Diffusion (Stability AI)
- **SD 1.x** (2022) — Open source; 512×512; CLIP text encoder; U-Net.
- **SD 2.x** (2022) — OpenCLIP; 768×768.
- **SDXL** (2023) — Dual text encoders; 1024×1024; refiner model.
- **SD 3** (2024) — DiT architecture; multi-modal diffusion transformer; text rendering.
- **SD 3.5** (2024) — Improved version of SD 3.

### DALL-E (OpenAI)
- **DALL-E 1** (2021) — dVAE + Transformer (not diffusion).
- **DALL-E 2** (2022) — CLIP + diffusion; introduced inpainting/outpainting.
- **DALL-E 3** (2023) — Integrated with ChatGPT; T5 encoder; highly prompt-faithful; built on Consistency Models.

### Midjourney
- Proprietary; consistently high aesthetic quality.
- Available via Discord and web interface.
- v6 (2024) — Photorealistic, improved prompt following, text rendering.

### Imagen (Google)
- Large frozen T5 text encoder + cascaded diffusion models.
- **Imagen 2** — Powers Google's products.
- **Imagen 3** (2024) — DiT; improved photorealism and text rendering.

### FLUX (Black Forest Labs)
- Created by original Stable Diffusion researchers.
- **FLUX.1** (2024) — Three variants: Pro, Dev (open weights), Schnell (open weights, fast).
- State-of-the-art image quality and text rendering.
- Hybrid architecture: DiT + U-Net elements.

## Sampling Speed Improvements

A key challenge with diffusion models is slow sampling (T=1000 steps). Solutions:

| Method | Steps | Notes |
|---|---|---|
| DDIM | 20–50 | Deterministic; no quality loss |
| DPM-Solver | 10–20 | ODE solver; fast + high quality |
| LCM (Latency Consistency Models) | 4–8 | Distillation-based; some quality loss |
| SDXL-Turbo | 1–4 | Adversarial diffusion distillation |
| Lightning | 1–4 | Distillation; open source |

## Video Diffusion Models

Extend image diffusion to video by adding temporal attention/convolutions:

- **Sora** (OpenAI, 2024) — DiT on space-time patches ("spacetime patches"). Up to 60 seconds.
- **Veo 2** (Google, 2024) — High quality; physics understanding.
- **CogVideoX** — Open source.
- **Stable Video Diffusion** — Stability AI; image-to-video.

## See Also

- [Generative Models Overview](../overview.md)
- [GANs](gans.md)
- [VAEs](vaes.md)
- [Multimodal Models](../multimodal/overview.md)
