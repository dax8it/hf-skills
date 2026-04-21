# HF Space: whisper-small French ASR

**Date:** 2026-04-21
**Status:** Approved

## Goal

A private Hugging Face Space to test `openai/whisper-small` for French speech-to-text, primarily accessed from a phone browser.

## Architecture

Single Gradio app, three files:

- `app.py` — Gradio `gr.Interface` wiring mic audio → Whisper pipeline → transcript text
- `requirements.txt` — `transformers`, `gradio`, `torch`
- `README.md` — HF Space metadata header

## app.py

- Load `openai/whisper-small` via `transformers.pipeline("automatic-speech-recognition")`
- Force `language="fr"` via `generate_kwargs` to avoid language detection overhead
- Input: `gr.Audio(sources=["microphone"], type="filepath")`
- Output: `gr.Textbox(label="Transcription")`
- Wrap with `gr.Interface(fn=transcribe, inputs=audio, outputs=text, title="Whisper Small — French")`

## Deployment

- Space ID: `SaylorTwift/whisper-small-fr`
- Visibility: private
- SDK: gradio
- Hardware: CPU basic (free tier) — whisper-small on CPU takes ~5–10s per clip, acceptable for testing
- Create Space via `huggingface_hub.HfApi().create_repo()` then push files with `upload_folder` or individual `upload_file` calls

## Constraints

- No streaming (whisper-small is encoder-decoder, not streaming-capable out of the box)
- No transcript history — single-shot per recording
- No auth UI needed — private Space is gated by HF login
