# References & Further Reading

A curated collection of key papers, resources, and tools for learning about LLMs and generative models.

---

## Foundational Papers

### Transformer Architecture
- **Attention Is All You Need** — Vaswani et al., 2017. [[arXiv]](https://arxiv.org/abs/1706.03762)
  *Introduced the Transformer architecture.*
- **BERT: Pre-training of Deep Bidirectional Transformers** — Devlin et al., 2018. [[arXiv]](https://arxiv.org/abs/1810.04805)
- **An Image is Worth 16×16 Words: Transformers for Image Recognition at Scale** — Dosovitskiy et al., 2020. [[arXiv]](https://arxiv.org/abs/2010.11929)
  *Vision Transformer (ViT).*

### Scaling & Pretraining
- **Language Models are Few-Shot Learners (GPT-3)** — Brown et al., 2020. [[arXiv]](https://arxiv.org/abs/2005.14165)
- **Scaling Laws for Neural Language Models** — Kaplan et al., 2020. [[arXiv]](https://arxiv.org/abs/2001.08361)
- **Training Compute-Optimal Large Language Models (Chinchilla)** — Hoffmann et al., 2022. [[arXiv]](https://arxiv.org/abs/2203.15556)
- **LLaMA: Open and Efficient Foundation Language Models** — Touvron et al., 2023. [[arXiv]](https://arxiv.org/abs/2302.13971)
- **Llama 2** — Touvron et al., 2023. [[arXiv]](https://arxiv.org/abs/2307.09288)

### Alignment
- **Training language models to follow instructions with human feedback (InstructGPT)** — Ouyang et al., 2022. [[arXiv]](https://arxiv.org/abs/2203.02155)
- **Constitutional AI** — Bai et al., 2022. [[arXiv]](https://arxiv.org/abs/2212.08073)
- **Direct Preference Optimization (DPO)** — Rafailov et al., 2023. [[arXiv]](https://arxiv.org/abs/2305.18290)

### Attention & Efficiency
- **FlashAttention** — Dao et al., 2022. [[arXiv]](https://arxiv.org/abs/2205.14135)
- **FlashAttention-2** — Dao, 2023. [[arXiv]](https://arxiv.org/abs/2307.08691)
- **GQA: Training Generalized Multi-Query Transformer Models** — Ainslie et al., 2023. [[arXiv]](https://arxiv.org/abs/2305.13245)
- **Mixtral of Experts** — Jiang et al., 2024. [[arXiv]](https://arxiv.org/abs/2401.04088)

### Fine-Tuning & PEFT
- **LoRA: Low-Rank Adaptation of Large Language Models** — Hu et al., 2022. [[arXiv]](https://arxiv.org/abs/2106.09685)
- **QLoRA: Efficient Finetuning of Quantized LLMs** — Dettmers et al., 2023. [[arXiv]](https://arxiv.org/abs/2305.14314)

---

## Generative Models — Foundational Papers

### Diffusion Models
- **DDPM: Denoising Diffusion Probabilistic Models** — Ho et al., 2020. [[arXiv]](https://arxiv.org/abs/2006.11239)
- **DDIM: Denoising Diffusion Implicit Models** — Song et al., 2020. [[arXiv]](https://arxiv.org/abs/2010.02502)
- **High-Resolution Image Synthesis with Latent Diffusion Models** — Rombach et al., 2022. [[arXiv]](https://arxiv.org/abs/2112.10752)
  *Stable Diffusion paper.*
- **Scalable Diffusion Models with Transformers (DiT)** — Peebles & Xie, 2023. [[arXiv]](https://arxiv.org/abs/2212.09748)
- **DALL-E 3** — Betker et al., 2023. [[OpenAI Blog]](https://openai.com/research/dall-e-3)

### GANs
- **Generative Adversarial Nets** — Goodfellow et al., 2014. [[arXiv]](https://arxiv.org/abs/1406.2661)
- **Progressive Growing of GANs** — Karras et al., 2018. [[arXiv]](https://arxiv.org/abs/1710.10196)
- **A Style-Based Generator Architecture for GANs (StyleGAN)** — Karras et al., 2019. [[arXiv]](https://arxiv.org/abs/1812.04948)
- **Analyzing and Improving the Image Quality of StyleGAN (StyleGAN2)** — Karras et al., 2020. [[arXiv]](https://arxiv.org/abs/1912.04958)

### VAEs
- **Auto-Encoding Variational Bayes** — Kingma & Welling, 2013. [[arXiv]](https://arxiv.org/abs/1312.6114)
- **Neural Discrete Representation Learning (VQ-VAE)** — van den Oord et al., 2017. [[arXiv]](https://arxiv.org/abs/1711.00937)

### Vision-Language / Multimodal
- **CLIP: Learning Transferable Visual Models From Natural Language Supervision** — Radford et al., 2021. [[arXiv]](https://arxiv.org/abs/2103.00020)
- **Flamingo: a Visual Language Model for Few-Shot Learning** — Alayrac et al., 2022. [[arXiv]](https://arxiv.org/abs/2204.14198)
- **LLaVA: Visual Instruction Tuning** — Liu et al., 2023. [[arXiv]](https://arxiv.org/abs/2304.08485)
- **Gemini: A Family of Highly Capable Multimodal Models** — Google DeepMind, 2023. [[arXiv]](https://arxiv.org/abs/2312.11805)

### Prompting & Reasoning
- **Chain-of-Thought Prompting Elicits Reasoning in LLMs** — Wei et al., 2022. [[arXiv]](https://arxiv.org/abs/2201.11903)
- **Self-Consistency Improves Chain of Thought Reasoning** — Wang et al., 2022. [[arXiv]](https://arxiv.org/abs/2203.11171)
- **Tree of Thoughts** — Yao et al., 2023. [[arXiv]](https://arxiv.org/abs/2305.10601)
- **ReAct: Synergizing Reasoning and Acting in Language Models** — Yao et al., 2022. [[arXiv]](https://arxiv.org/abs/2210.03629)

---

## Courses & Tutorials

| Resource | Platform | Focus |
|---|---|---|
| [CS324 — Large Language Models](https://stanford-cs324.github.io/winter2022/) | Stanford | Comprehensive LLM course |
| [fast.ai Practical Deep Learning](https://course.fast.ai/) | fast.ai | Practical DL / diffusion |
| [Hugging Face NLP Course](https://huggingface.co/learn/nlp-course) | Hugging Face | Transformers in practice |
| [DeepLearning.AI Short Courses](https://www.deeplearning.ai/short-courses/) | DeepLearning.AI | LLM applications |
| [Andrej Karpathy — Neural Networks: Zero to Hero](https://karpathy.ai/zero-to-hero.html) | YouTube | Build LLMs from scratch |

---

## Key Blogs & Newsletters

| Source | Focus |
|---|---|
| [Lilian Weng's Blog (lilianweng.github.io)](https://lilianweng.github.io/) | Deep dives on LLMs, RL, diffusion |
| [Sebastian Raschka's Magazine](https://magazine.sebastianraschka.com/) | LLM training, fine-tuning |
| [The Batch (deeplearning.ai)](https://www.deeplearning.ai/the-batch/) | Weekly AI news |
| [Import AI (Jack Clark)](https://jack-clark.net/) | Weekly AI research digest |
| [AI Alignment Forum](https://www.alignmentforum.org/) | Safety research |
| [Hugging Face Blog](https://huggingface.co/blog) | Open-source ML |

---

## Tools & Libraries

### LLM Development
| Tool | Purpose |
|---|---|
| [Hugging Face Transformers](https://github.com/huggingface/transformers) | Model loading, fine-tuning |
| [vLLM](https://github.com/vllm-project/vllm) | Fast LLM inference server |
| [llama.cpp](https://github.com/ggerganov/llama.cpp) | CPU/GPU inference for LLaMA models |
| [Ollama](https://ollama.com/) | Local LLM runner |
| [LangChain](https://www.langchain.com/) | LLM application framework |
| [LlamaIndex](https://www.llamaindex.ai/) | RAG and document Q&A |
| [DSPy](https://dspy.ai/) | Programmatic prompt optimization |

### Fine-Tuning
| Tool | Purpose |
|---|---|
| [Unsloth](https://github.com/unslothai/unsloth) | Fast LoRA fine-tuning |
| [Axolotl](https://github.com/OpenAccess-AI-Collective/axolotl) | Flexible fine-tuning framework |
| [TRL (Transformers RL)](https://github.com/huggingface/trl) | SFT, DPO, PPO |

### Image Generation
| Tool | Purpose |
|---|---|
| [Diffusers](https://github.com/huggingface/diffusers) | Diffusion model library |
| [ComfyUI](https://github.com/comfyanonymous/ComfyUI) | Modular image generation UI |
| [Automatic1111 (WebUI)](https://github.com/AUTOMATIC1111/stable-diffusion-webui) | Stable Diffusion UI |

---

## Benchmarks

| Benchmark | Measures |
|---|---|
| [MMLU](https://huggingface.co/datasets/cais/mmlu) | Broad knowledge (57 subjects) |
| [HumanEval](https://github.com/openai/human-eval) | Python code generation |
| [MATH](https://github.com/hendrycks/math) | Competition mathematics |
| [GPQA](https://arxiv.org/abs/2311.12022) | PhD-level science Q&A |
| [AIME](https://artofproblemsolving.com/wiki/index.php/AMC_Problems_and_Solutions) | Math olympiad problems |
| [SWE-Bench](https://www.swebench.com/) | Real-world GitHub issue resolution |
| [Chatbot Arena](https://lmsys.org/blog/2023-05-03-arena/) | Human preference ELO ranking |
| [Open LLM Leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard) | Open-source model comparison |

---

## See Also

- [LLMs Overview](llms/overview.md)
- [Generative Models Overview](generative-models/overview.md)
