# Multimodal Models

## Overview

**Multimodal models** can understand and/or generate content across multiple modalities — text, images, audio, video, and more. This contrasts with unimodal models that only handle a single type of data.

Modern frontier AI models are increasingly multimodal. The goal is to build models that perceive and interact with the world as humans do, where information comes in multiple forms simultaneously.

## Modalities

| Modality | Input examples | Output examples |
|---|---|---|
| **Text** | Instructions, documents, code | Answers, summaries, code |
| **Image** | Photos, diagrams, screenshots, PDFs | Descriptions, analysis |
| **Audio** | Speech, music, environmental sounds | Transcriptions, speech, music |
| **Video** | Recordings, screen captures | Descriptions, captions |
| **Documents** | PDFs with mixed text/images | Analysis, Q&A |
| **3D** | Point clouds, meshes | 3D descriptions, geometry |

## Approaches to Multimodality

### Late Fusion
Separate models process each modality; outputs are combined (concatenated or averaged) for the final prediction.
- Simple but doesn't model cross-modal interactions well.

### Early/Mid Fusion
Modality-specific encoders produce embeddings that are concatenated or interleaved into a joint sequence, processed by a shared (usually Transformer) backbone.
- More powerful; enables cross-modal attention.

### Natively Multimodal
Model is trained from the start on interleaved multimodal data, with a single unified architecture processing all modalities.
- Best cross-modal understanding; requires massive multimodal datasets.
- Examples: Gemini, GPT-4o.

## Vision-Language Models (VLMs)

The most common type of multimodal model: accepts text + images as input.

### Architecture Pattern

The standard architecture (used in LLaVA, Qwen-VL, InternVL, etc.):

1. **Vision encoder** — A pretrained vision model (usually ViT, CLIP) encodes the image to patch embeddings.
2. **Projection/adapter layer** — Maps vision embeddings to the LLM's token embedding space.
3. **LLM backbone** — Processes the interleaved sequence of vision tokens and text tokens.

```
Image → [Vision Encoder] → [Projection] → Visual tokens
Text  → [Tokenizer]                    → Text tokens
                                              ↓
                                         [LLM]
                                              ↓
                                        Text output
```

### CLIP (OpenAI, 2021)

**Contrastive Language-Image Pretraining** — the most influential vision-language model.

- Jointly trains an **image encoder** and a **text encoder** to align their representations.
- Training objective: maximize cosine similarity of matching image-text pairs; minimize for non-matching.
- Trained on 400M image-text pairs from the web.
- Zero-shot image classification: no task-specific training needed.
- Powers many downstream VLMs as a frozen vision backbone.

### LLaVA (Large Language and Vision Assistant, 2023)

*Liu et al., 2023.*
- Connects a CLIP vision encoder to LLaMA/Mistral with a simple linear projection.
- Instruction-tuned on GPT-4-generated vision-language data.
- Highly influential; spawned LLaVA-1.5, LLaVA-NeXT (1.6), and many variants.
- Open source.

### GPT-4V / GPT-4o (Vision)

- GPT-4 with vision: CLIP or native image understanding.
- GPT-4o: natively multimodal end-to-end (text + image + audio).
- Can analyze charts, photos, screenshots, documents.

### Gemini (Google DeepMind)

- Natively multimodal from pretraining.
- Accepts: text, images, audio, video, PDFs, code.
- Can reason across modalities (e.g., "what sound does this animal make?" from an image).

### Claude 3+ (Anthropic)

- All Claude 3 models accept image inputs.
- Excellent document understanding (charts, diagrams, scanned text).
- Claude 3.5 Sonnet: computer use (screenshot → action).

### Qwen-VL / Qwen2-VL (Alibaba)

- Strong open-weight vision-language model.
- Qwen2-VL supports high-resolution images and videos.
- Excellent multilingual + vision capabilities.

### InternVL 2 (Shanghai AI Lab)

- Open-source VLM; very strong benchmarks.
- Multiple sizes: 1B to 108B.
- InternViT vision encoder trained at very high resolution.

## Audio-Language Models

### Whisper (OpenAI, 2022)

- **Speech recognition** model trained on 680,000 hours of supervised data.
- Highly robust; handles multiple languages, accents, noisy audio.
- Tasks: transcription, translation, language identification.
- Open source; widely used.

### Audio-Visual Speech Recognition

Combines lip movements (video) with audio for noise-robust transcription.

### GPT-4o Audio

- Processes audio natively without a separate ASR step.
- Captures emotion, tone, speaking style.
- Can generate expressive speech.
- Enables real-time voice conversations.

### Gemini Audio

- Understands speech, music, and environmental sounds.
- Can answer questions about audio content.
- Native part of Gemini 2.0 Flash.

## Video Understanding

### Video-LLMs

Extend VLMs to video by sampling frames and/or using temporal encoders:

| Model | Notes |
|---|---|
| **Video-LLaVA** | Frame sampling + LLaVA |
| **VideoChat** | Conversational video understanding |
| **Qwen2-VL** | Strong video understanding |
| **Gemini 1.5 Pro** | Native video; 1M context (≈1 hour of video) |
| **GPT-4o** | Video input (frames) |

### Key Challenges
- Temporal reasoning (what happens before/after?)
- Long-form video (many hours)
- Dense video captioning

## Interleaved/Unified Multimodal Generation

The newest generation of models can **both understand and generate** across modalities:

| Model | Input | Output |
|---|---|---|
| GPT-4o | Text, image, audio | Text, image, audio |
| Gemini 2.0 Flash | Text, image, audio, video | Text, image, audio |
| Janus / Janus-Pro (DeepSeek) | Text, image | Text, image |
| Show-o | Text, image | Text, image |
| Chameleon (Meta) | Text, image | Text, image |
| DALL-E 3 in ChatGPT | Text (image) | Text + image |

## Benchmark Tasks

| Benchmark | Task | Notes |
|---|---|---|
| **VQA v2** | Visual question answering | Standard image QA |
| **MMMU** | Multi-discipline questions with images | University-level |
| **MMBench** | Comprehensive VLM evaluation | Multiple subtasks |
| **ScienceQA** | Science questions with diagrams | K-12 level |
| **DocVQA** | Document understanding | OCR + reasoning |
| **TextVQA** | Reading text in images | OCR-dependent |
| **Video-MME** | Video understanding | Long and short video |
| **SEED-Bench** | Comprehensive multimodal | 19K questions |

## See Also

- [Diffusion Models](../image/diffusion-models.md)
- [Text-to-Speech](../audio/text-to-speech.md)
- [GPT Series](../../llms/models/gpt-series.md)
- [Gemini Series](../../llms/models/gemini-series.md)
- [Tool Use](../../llms/features/tool-use.md)
