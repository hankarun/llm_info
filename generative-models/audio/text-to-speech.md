# Text-to-Speech Models

## Overview

**Text-to-Speech (TTS)**, also called **speech synthesis**, converts written text into natural-sounding spoken audio. Modern TTS systems are neural network-based and can produce highly natural, expressive speech — sometimes indistinguishable from a human voice.

TTS is a key component of voice assistants, accessibility tools, audiobook generation, and multimodal AI systems.

## TTS Pipeline (Traditional)

Traditional neural TTS uses a two-stage pipeline:

1. **Acoustic model (text → spectrogram):**
   - Input: Text (usually phonemes after text normalization + grapheme-to-phoneme conversion).
   - Output: Mel spectrogram (a visual representation of audio).

2. **Vocoder (spectrogram → waveform):**
   - Input: Mel spectrogram.
   - Output: Raw audio waveform (e.g., 24kHz PCM).

## Key Models

### Tacotron / Tacotron 2 (Google, 2017–2018)
- Seq2seq with attention: encodes text, decodes to mel spectrogram.
- **Tacotron 2** + **WaveNet** vocoder: near-human quality at the time.
- Slow due to autoregressive vocoder.

### FastSpeech / FastSpeech 2 (Microsoft, 2019–2020)
- **Non-autoregressive**: predicts all spectrogram frames in parallel.
- Uses a **duration predictor** to align text and speech.
- Much faster than Tacotron; comparable quality.
- FastSpeech 2 adds pitch and energy prediction for more expressive speech.

### VITS (Variational Inference TTS, 2021)
- End-to-end: text → waveform in a single model.
- Combines a flow-based spectrogram generator with a GAN vocoder.
- Very natural prosody; widely used in open-source TTS.
- Basis for many modern TTS systems.

### WaveNet (DeepMind, 2016)
- Autoregressive convolutional model that generates audio samples one by one.
- Extremely high quality but very slow (real-time factor < 1× on CPU).
- Parallel WaveNet and WaveGlow improved inference speed.

### HiFi-GAN (2020)
- GAN-based vocoder; very fast and high quality.
- Replaced WaveNet in many production systems.
- Widely used as the vocoder component in VITS and FastSpeech 2.

## Modern Neural TTS Systems

### Tortoise-TTS (2022)
- Open-source, high-quality multi-speaker TTS.
- Uses an autoregressive model + diffusion vocoder.
- Supports zero-shot voice cloning from a few seconds of audio.
- Slow but very expressive.

### XTTS / Coqui TTS
- Open-source; supports 17+ languages.
- **XTTS v2**: Cross-lingual voice cloning; speaker cloning from a 6-second sample.
- Based on VQ-VAE + autoregressive decoder.

### Bark (Suno AI, 2023)
- Open-source; GPT-style model trained on audio tokens.
- Can generate speech, music, sound effects, and laughter.
- Supports voice cloning and multilingual output.
- Relatively slow.

### Parler-TTS (HuggingFace, 2024)
- Open-source; condition speech on natural language descriptions:
  - "A clear, enthusiastic voice with fast pace and high pitch"
- Based on the MusicGen architecture.

### MetaVoice
- Open-source; zero-shot voice cloning.
- Supports emotional speech.

## Proprietary / Commercial Systems

### ElevenLabs
- Industry-leading voice cloning quality.
- Voice cloning from a few seconds of audio.
- Supports 30+ languages, emotional speech, voice design.
- API available; powers many audiobook and content creation workflows.

### OpenAI TTS
- Available via API (`tts-1`, `tts-1-hd`).
- 6 preset voices.
- Fast and high quality.
- **GPT-4o** natively generates audio end-to-end.

### Google Cloud TTS / WaveNet TTS
- WaveNet and Neural2 voices.
- 220+ voices, 40+ languages.
- Wavenet and Studio voices offer highest quality.

### Azure Neural TTS (Microsoft)
- 400+ voices, 140+ languages.
- Custom Neural Voice: clone a voice with minimal data.
- Supports SSML for fine-grained control.

### Amazon Polly
- Neural TTS with NTTS (Neural Text-to-Speech).
- Supports SSML; multiple voice styles.

## End-to-End Audio LLMs

The latest generation of speech models abandons the pipeline entirely:

### GPT-4o Audio
- Processes and generates audio natively.
- Preserves tone, emotion, laughter, and other paralinguistic features.
- Real-time conversation possible.
- No separate ASR (speech recognition) or TTS step.

### Gemini 2.0 Flash
- Native audio output.
- Expressive speech with controllable style.

## Voice Cloning

Modern TTS systems can clone a voice from very little data:

| System | Data needed | Quality |
|---|---|---|
| ElevenLabs | ~1 minute | Excellent |
| XTTS v2 | 6 seconds | Good |
| Tortoise-TTS | ~10 clips | Very good |
| Azure Custom Neural Voice | 30+ minutes | Professional |

**Ethical concerns:** Voice cloning can be used for impersonation and fraud. Mitigations:
- Consent requirements.
- Voice watermarking.
- Detection models.

## Evaluation Metrics

| Metric | Description |
|---|---|
| **MOS (Mean Opinion Score)** | Human rating of naturalness 1–5 |
| **MUSHRA** | Multi-stimulus human rating |
| **WER (Word Error Rate)** | Intelligibility via ASR |
| **Speaker similarity** | Cosine similarity of speaker embeddings |
| **UTMOS** | Automatic MOS predictor |

## SSML (Speech Synthesis Markup Language)

XML-based language for controlling TTS prosody:

```xml
<speak>
  <prosody rate="slow" pitch="+5%">
    Welcome to our service.
  </prosody>
  <break time="500ms"/>
  <emphasis level="strong">Please listen carefully.</emphasis>
</speak>
```

Supported by Google, Amazon Polly, Azure, and others.

## See Also

- [Generative Models Overview](../overview.md)
- [Multimodal Models](../multimodal/overview.md)
- [GPT Series](../../llms/models/gpt-series.md)
