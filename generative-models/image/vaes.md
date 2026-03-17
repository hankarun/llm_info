# Variational Autoencoders (VAEs)

## Overview

A **Variational Autoencoder (VAE)** is a generative model that learns a structured **latent representation** of data. Introduced by Kingma & Welling (2013) in *"Auto-Encoding Variational Bayes"*, VAEs combine ideas from autoencoders and Bayesian inference.

VAEs are foundational to modern image generation: they are the **compression backbone** of Latent Diffusion Models (e.g., Stable Diffusion), and directly powered early image generation models like DALL-E 1.

## Autoencoder vs. VAE

### Standard Autoencoder
- **Encoder** maps input `x` to latent vector `z`.
- **Decoder** reconstructs `x̂` from `z`.
- Trained to minimize reconstruction loss.
- Latent space is **deterministic** and often discontinuous — cannot sample new points.

### Variational Autoencoder
- Encoder maps `x` to a **distribution** `q(z|x) = N(μ, σ²)` rather than a point.
- At training, sample `z ~ q(z|x)`.
- Decoder reconstructs from sampled `z`.
- Latent space is **continuous and structured** — new points can be sampled from `p(z) = N(0, I)`.

## Architecture

```
Input x
  ↓
Encoder → [μ, log σ²]    (two output heads)
  ↓
Reparameterization: z = μ + σ * ε  (ε ~ N(0,1))
  ↓
Decoder → Reconstructed x̂
```

### Reparameterization Trick

Direct sampling from `N(μ, σ²)` is non-differentiable. The reparameterization trick makes it differentiable:

```
z = μ + σ ⊙ ε,  ε ~ N(0, I)
```

Gradients can now flow through `μ` and `σ` via backpropagation.

## Training Objective: ELBO

The **Evidence Lower Bound (ELBO)** is the VAE training loss:

```
L = E_q[log p(x|z)] - KL(q(z|x) || p(z))
    ↑ reconstruction       ↑ regularization
```

- **Reconstruction term:** Penalize poor reconstruction (cross-entropy for binary data, MSE for continuous).
- **KL divergence term:** Force the approximate posterior `q(z|x)` to stay close to the prior `p(z) = N(0,I)`. This regularizes the latent space.

Minimizing `-ELBO = reconstruction_loss + β * KL`.

### β-VAE

Setting `β > 1` increases the weight on the KL term, encouraging a more **disentangled** latent space where individual dimensions have interpretable meanings.

## Properties of the Latent Space

A well-trained VAE latent space:

1. **Continuity** — Close points in latent space decode to similar images.
2. **Completeness** — Sampling from `p(z) = N(0,I)` produces valid outputs.
3. **Disentanglement** (β-VAE) — Individual latent dimensions correspond to independent generative factors (shape, color, orientation).

## Limitations

- **Blurry reconstructions** — The pixel-space reconstruction loss averages over possible outputs, leading to blurry generated images. This motivated the GAN and diffusion approaches.
- **Posterior collapse** — The decoder may learn to ignore `z` if it's powerful enough; KL goes to 0.
- **Limited expressiveness** — The Gaussian prior and approximate posterior are simple.

## Key Variants

### VQ-VAE (Vector-Quantized VAE)
*van den Oord et al., 2017.*

Uses a **discrete** (codebook-based) latent space instead of continuous Gaussian:
- Encoder outputs are quantized to the nearest entry in a learned codebook.
- Decoder uses the quantized vectors.
- No KL term; uses commitment loss instead.
- Enables high-quality reconstructions without blurriness.

**VQ-VAE-2** adds a hierarchical latent structure for high-resolution generation.

### DALL-E 1 dVAE

DALL-E 1 (OpenAI, 2021) used a **discrete VAE (dVAE)** with Gumbel-softmax relaxation:
- Images encoded as 32×32 grids of discrete tokens.
- A Transformer (GPT-style) generates sequences of image tokens conditioned on text.

### KL-regularized Autoencoder for LDMs

The VAE used in **Latent Diffusion Models** (Rombach et al., 2022) and **Stable Diffusion** is a convolutional VAE with:
- Downsampling factor of 4–8× (e.g., 512×512 → 64×64 latent).
- Very small KL weight (nearly a standard autoencoder) — optimized for reconstruction quality, not sampling from prior.
- The **diffusion model** handles generation in latent space; the VAE just provides efficient compression/decompression.

### Stable Diffusion VAE

- **Encoder:** Maps 512×512 RGB image → 64×64×4 latent.
- **Decoder:** Maps 64×64×4 latent → 512×512 RGB image.
- 8× spatial compression, 3→4 channel change.
- Available as a standalone component; can be fine-tuned for better color reproduction.

## VAEs for Text

**Text VAEs** encode sentences into continuous latent vectors, enabling:
- Interpolation between sentences.
- Controlled text generation.
- Style transfer.

Examples: Optimus (Li et al., 2020), sentence-VAE.

Harder than image VAEs due to discrete tokens; requires techniques like:
- KL annealing (slowly increase KL weight during training).
- Aggressive posterior inference.
- Improved ELBO bounds (IWAE).

## VQGAN

**VQGAN** (Esser et al., 2021) combines VQ-VAE with GAN training:
- Uses a discriminator loss to improve perceptual quality.
- Trained on perceptual loss (VGG features) + adversarial loss + codebook loss.
- Much sharper reconstructions than standard VAE.
- Used as the image tokenizer in **Taming Transformers** and inspires LDM autoencoders.

## Summary

| Model | Latent type | Loss | Use case |
|---|---|---|---|
| Standard AE | Continuous (point) | Reconstruction | Compression, denoising |
| VAE | Continuous (Gaussian) | ELBO (recon + KL) | Generation, interpolation |
| β-VAE | Continuous | β-ELBO | Disentanglement |
| VQ-VAE | Discrete (codebook) | Commitment + recon | High-quality recon, tokenization |
| VQGAN | Discrete | Perceptual + adv | Image tokenization for generation |

## See Also

- [Diffusion Models](diffusion-models.md)
- [GANs](gans.md)
- [Generative Models Overview](../overview.md)
