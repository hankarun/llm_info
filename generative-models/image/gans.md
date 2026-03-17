# Generative Adversarial Networks (GANs)

## Overview

**GANs** were introduced by Ian Goodfellow et al. in 2014 (*"Generative Adversarial Nets"*). They are a framework in which two neural networks — a **Generator** and a **Discriminator** — compete in a minimax game, driving each other to improve.

GANs dominated image generation from 2014 to ~2022, when diffusion models surpassed them in quality and diversity. However, GANs remain relevant for their speed and efficiency advantages.

## Core Framework

### Generator (G)
Takes a random noise vector `z ~ p_z(z)` (usually Gaussian) and maps it to a sample in data space:

```
G: z → x̂ (fake sample)
```

### Discriminator (D)
Takes a sample (real or fake) and outputs the probability that it is real:

```
D: x → [0, 1]  (1 = real, 0 = fake)
```

### Training Objective

The minimax game:

```
min_G max_D  E[log D(x)] + E[log(1 - D(G(z)))]
```

- **D** tries to maximize: correctly classify real as real and fake as fake.
- **G** tries to minimize: fool D into thinking fakes are real.

In practice, G maximizes `E[log D(G(z))]` to avoid vanishing gradients early in training.

## Training Dynamics

GANs are notoriously difficult to train:

### Mode Collapse
The generator learns to produce only a small subset of possible outputs (e.g., one type of face) that consistently fools the discriminator. Diversity is lost.

### Training Instability
The generator and discriminator need to improve in balance. If D becomes too strong, G gets no useful gradient. If G improves too fast, D provides no useful signal.

### Non-convergence
Standard GAN training may oscillate without converging.

## Key Architectural Advances

### DCGAN (2015)
*"Unsupervised Representation Learning with Deep Convolutional GANs"*
- Replaced fully-connected layers with convolutional layers.
- Batch normalization throughout.
- Established modern GAN architecture norms.
- First GANs to generate recognizable images reliably.

### ProGAN / Progressive Growing of GANs (2018)
*Karras et al., NVIDIA.*
- Start training at low resolution (4×4), progressively add layers to increase resolution.
- First to generate high-quality 1024×1024 faces (CelebA-HQ).

### StyleGAN (2019) / StyleGAN2 (2020) / StyleGAN3 (2021)
*Karras et al., NVIDIA.*
- **Style-based generator:** Inject style information at each layer via AdaIN (Adaptive Instance Normalization).
- **Mapping network:** Map `z` to intermediate latent space `w`.
- **Progressive growing** replaced with skip connections.
- Enables fine-grained control: separate control of coarse (pose) and fine (texture) features.
- **StyleGAN2:** Removes characteristic StyleGAN1 artifacts; improved FID.
- **StyleGAN3:** Alias-free generation; removes texture sticking artifacts.
- Powers the [thispersondoesnotexist.com](https://thispersondoesnotexist.com) website.

### BigGAN (2018)
*Brock et al., DeepMind.*
- Class-conditional generation at high resolution (256×256, 512×512).
- Very large batch sizes + truncation trick.
- State of the art on ImageNet generation for several years.

### Pix2Pix (2017)
*Isola et al.*
- **Conditional GAN** for image-to-image translation.
- Paired training data: translate between image domains (edges→photo, sketch→face, etc.).

### CycleGAN (2017)
*Zhu et al.*
- Unpaired image-to-image translation using cycle consistency loss.
- No need for paired datasets; learns domain mapping (photo↔painting, horse↔zebra).

### GauGAN / SPADE (2019)
*Park et al., NVIDIA.*
- Semantic image synthesis from segmentation maps.
- **SPADE** normalization layers.

## Loss Function Improvements

### Wasserstein GAN (WGAN, 2017)
Uses **Wasserstein distance** instead of JS divergence as the loss, providing more meaningful gradients and more stable training:

```
min_G max_D  E[D(x)] - E[D(G(z))]
```

Requires weight clipping or gradient penalty (WGAN-GP) to enforce Lipschitz constraint.

### WGAN-GP (2017)
Gradient penalty instead of weight clipping. More stable, widely adopted.

### Hinge Loss
```
D loss: E[max(0, 1 - D(x))] + E[max(0, 1 + D(G(z)))]
G loss: -E[D(G(z))]
```
Commonly used in BigGAN and other state-of-the-art models.

### Spectral Normalization
Normalize discriminator weights to stabilize training by controlling the Lipschitz constant.

## Evaluation Metrics

### FID (Fréchet Inception Distance)
Most widely used GAN metric. Measures distance between distributions of real and generated images in feature space (InceptionV3):

```
FID = ||μ_r - μ_g||² + Tr(Σ_r + Σ_g - 2(Σ_r Σ_g)^{1/2})
```

Lower FID = more similar to real data distribution.

### IS (Inception Score)
Measures quality (sharp, recognizable images) and diversity:
```
IS = exp(E_x[KL(p(y|x) || p(y))])
```
Higher = better. Doesn't compare to real data distribution.

### Precision and Recall
- **Precision** — Are generated samples realistic?
- **Recall** — Does the generator cover the full data diversity?

## GAN Applications

| Application | Method |
|---|---|
| **Photorealistic face generation** | StyleGAN2/3 |
| **Super-resolution** | SRGAN, ESRGAN, Real-ESRGAN |
| **Image-to-image translation** | Pix2Pix, CycleGAN |
| **Inpainting** | DeepFill, LaMa |
| **Video generation** | VideoGAN, DVD-GAN |
| **Data augmentation** | Generate synthetic training data |
| **Domain adaptation** | Adapt between visual domains |
| **3D generation** | π-GAN, EG3D |

## GANs vs. Diffusion Models

| Aspect | GANs | Diffusion Models |
|---|---|---|
| Training stability | Difficult | More stable |
| Image quality | High (especially faces) | Higher (general) |
| Diversity | Prone to mode collapse | Higher diversity |
| Sampling speed | Very fast (single forward pass) | Slow (many steps); improving |
| Text conditioning | Difficult | Natural |
| Training data needs | Large | Large |
| Current usage | Niche; video, real-time | Dominant for image gen |

## Current Status (2025)

GANs have largely been superseded by diffusion models for general image generation. However, they remain important for:
- **Real-time applications** (no iterative inference needed).
- **Video synthesis** components.
- **Super-resolution** (e.g., Real-ESRGAN widely used for upscaling).
- **Research** in adversarial training.

Some hybrid approaches combine GANs with diffusion (e.g., adversarial diffusion distillation in SDXL-Turbo).

## See Also

- [Diffusion Models](diffusion-models.md)
- [VAEs](vaes.md)
- [Generative Models Overview](../overview.md)
