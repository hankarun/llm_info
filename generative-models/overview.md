# Generative Models — Overview

## What Are Generative Models?

**Generative models** are machine learning models that learn the underlying distribution of training data and can generate new samples from that distribution. In contrast to discriminative models (which predict labels), generative models can produce new text, images, audio, video, and other content.

## Major Families of Generative Models

| Family | Core mechanism | Primary output | Examples |
|---|---|---|---|
| **LLMs** | Autoregressive token prediction | Text, code | GPT-4, Claude, LLaMA |
| **Diffusion models** | Iterative denoising | Images, audio, video | Stable Diffusion, DALL-E 3, Sora |
| **GANs** | Generator vs. discriminator | Images, video | StyleGAN, BigGAN |
| **VAEs** | Encoder-decoder + latent space | Images, embeddings | VQVAE, DALL-E 1 |
| **Flow models** | Invertible transformations | Images, audio | Glow, RealNVP |
| **State space models** | Selective state compression | Text, sequences | Mamba, RWKV |

## Text Generation

Handled primarily by **LLMs** — see [LLMs Overview](../llms/overview.md).

## Image Generation

### Historical Progression

| Era | Approach | Examples |
|---|---|---|
| Pre-2014 | Handcrafted features | — |
| 2014–2018 | GANs | DCGAN, ProGAN, CycleGAN |
| 2016–2020 | VAEs | VQVAE, DALL-E 1 |
| 2020–present | Diffusion models | DALL-E 2/3, Midjourney, SD |

Today, **diffusion models** dominate image generation due to their superior quality, diversity, and training stability compared to GANs.

## Audio Generation

- **TTS (Text-to-Speech):** ElevenLabs, Bark, Tortoise-TTS, XTTS, Parler-TTS.
- **Music generation:** Suno, Udio, MusicGen, AudioCraft.
- **Voice cloning:** Clone a voice from a few seconds of audio.
- **Speech synthesis:** Native audio LLMs (GPT-4o, Gemini) generate expressive speech end-to-end.

## Video Generation

An emerging and fast-moving space:

| Model | Organization | Notes |
|---|---|---|
| **Sora** | OpenAI | Up to 60s, photorealistic |
| **Veo 2** | Google DeepMind | High quality, physics understanding |
| **Kling** | Kuaishou | Popular in Asia |
| **Runway Gen-3** | Runway | Creative video generation |
| **CogVideo** | Zhipu AI | Open-source |
| **Wan** | Alibaba | Open-source |

## 3D Generation

- **NeRF** (Neural Radiance Fields) — Reconstruct 3D scenes from 2D images.
- **3D Gaussian Splatting** — Faster 3D scene representation and rendering.
- **Text-to-3D:** Shap-E, DreamFusion, One-2-3-45.
- **CAD / mesh generation:** Emerging for engineering applications.

## Multimodal Generation

Models that both understand and generate across modalities:
- **Image + text → text** (vision-language models).
- **Text → image + text** (interleaved generation).
- **Text + image + audio → text + audio** (GPT-4o, Gemini 2.0 Flash).

See [Multimodal Models Overview](multimodal/overview.md).

## Evaluation Metrics

### Image Quality
- **FID (Fréchet Inception Distance)** — Measures distribution distance between real and generated images. Lower = better.
- **IS (Inception Score)** — Quality and diversity. Higher = better.
- **CLIP Score** — Measures text-image alignment.
- **Human evaluation** — Still the gold standard.

### Text Quality
- **Perplexity** — How well the model predicts held-out text. Lower = better.
- **BLEU, ROUGE** — N-gram overlap metrics for translation/summarization.
- **Human preference** — Preferred by human raters.

### Audio Quality
- **MOS (Mean Opinion Score)** — Human rating of naturalness (1–5).
- **MUSHRA** — Multi-stimulus test for audio quality.

## Ethical Considerations

Generative models raise important concerns:

- **Deepfakes** — Realistic but fake images/videos of people.
- **Copyright** — Models trained on copyrighted data; generated outputs may infringe.
- **Misinformation** — Synthetic media can be used for propaganda.
- **Bias** — Models may perpetuate or amplify biases from training data.
- **Environmental cost** — Training frontier models requires significant energy.

Mitigation efforts:
- Content provenance (C2PA watermarking standard).
- Safety classifiers and filters.
- Model cards and usage policies.
- Dataset licensing and consent.

## See Also

- [LLMs Overview](../llms/overview.md)
- [Diffusion Models](image/diffusion-models.md)
- [GANs](image/gans.md)
- [Multimodal Models](multimodal/overview.md)
