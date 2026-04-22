---
language:
  - en
license: other
license_name: aether-nc-1.0
license_link: LICENSE.md
base_model: google/gemma-4-E4B-it
tags:
  - roleplay
  - creative-writing
  - interactive-fiction
  - gemma4
  - lora
  - qlora
  - unsloth
  - fine-tuned
model_type: gemma4
pipeline_tag: text-generation
---

# Aether-4B

**A fine-tuned Gemma 4 E4B model for high-quality interactive fiction and character roleplay.**

By [Aether AI](https://github.com/aether-ai)

## Overview

Aether-4B is a QLoRA fine-tune of Google's Gemma 4 E4B-it (4B dense parameters)
trained on ~530 multi-turn roleplay conversations. It produces literary-quality
interactive fiction with consistent character voice, rich sensory prose, and
natural emotional progression.

The model excels at:
- First-person and third-person character roleplay
- Maintaining consistent character voice across long conversations
- Emotionally complex scenes (conflict, jealousy, tenderness, tension)
- Proper RP formatting (asterisks for actions, backticks for thoughts, em-dashes for dialogue)
- Respecting user agency (no godmoding)

## Model Details

| | |
|---|---|
| **Base model** | `google/gemma-4-E4B-it` (4B dense, Gemma 4 architecture) |
| **Fine-tuning method** | QLoRA (4-bit NF4, rank 64, alpha 128) |
| **Training framework** | Unsloth 2026.4.6 + TRL 0.24.0 |
| **Hardware** | NVIDIA RTX 5090 (32 GB VRAM) |
| **Training time** | ~81 minutes |
| **Context trained** | 8192 tokens |
| **Loss** | 2.19 (final) |
| **Epochs** | 2 |



### Training Hyperparameters

```
lora_r: 64
lora_alpha: 128
lora_dropout: 0.05
target_modules: [q_proj, k_proj, v_proj, o_proj, gate_proj, up_proj, down_proj]
learning_rate: 2e-4
lr_scheduler: cosine
warmup_steps: 50
batch_size: 1
gradient_accumulation: 16
optimizer: adamw_8bit
gradient_checkpointing: unsloth
max_seq_length: 8192
train_on_responses_only: true
```

## Formats Available

| Format | Size | Use Case |
|---|---|---|
| `Aether-4B-BF16.gguf` | ~8 GB | Maximum quality, requires 10+ GB VRAM |
| `Aether-4B-Q4_K_M.gguf` | ~2.5 GB | Recommended for most users |
| `model.safetensors` (merged BF16) | ~8 GB | For further fine-tuning or conversion |

## Usage

### With llama.cpp / llama-server

```bash
llama-server -m Aether-4B-Q4_K_M.gguf -c 8192 --port 8080
```

### With SillyTavern

1. Start llama-server as above
2. In SillyTavern: API Type → Text Completion → llama.cpp
3. Server URL: `http://localhost:8080`
4. Import character cards (JSON format)

### Chat Format

The model uses Gemma's chat template:

```
<start_of_turn>user
[system prompt + character card + scenario]

[user message]<end_of_turn>
<start_of_turn>model
[character response]<end_of_turn>
```

System prompt is prepended to the first user message (Gemma convention).

### Recommended Inference Settings

```
temperature: 0.85-0.95
top_p: 0.92
top_k: 40
repetition_penalty: 1.05
max_tokens: 1024-2048
```

## Limitations

- **Context**: Trained on 8192 tokens. Performance may degrade beyond this.
- **Size**: 4B parameters. For more complex narratives, a larger model (like
  the planned Aether-31B) will perform better.
- **Languages**: English only. Some characters use native-language interjections
  (Romanian, Swedish, Korean, Italian, French, etc.) but the model is
  English-dominant.
- **Content**: May generate mature fictional content including emotional
  intensity, conflict, and interpersonal tension. Not suitable for all
  audiences.

## License

**Aether Non-Commercial License v1.0** — see [LICENSE.md](LICENSE.md).

- Personal, academic, and research use: **allowed**
- Commercial use: **not allowed**
- Derivative works (fine-tuning, merging, remixing, distilling): **not allowed**
- Redistribution (unmodified, with license): **allowed**

The base model (Gemma 4 E4B-it) is subject to [Google's Gemma Terms of Use](https://ai.google.dev/gemma/terms).

## Acknowledgments

- **Google** for Gemma 4 E4B-it
- **Unsloth** for fast QLoRA fine-tuning

## Citation

```
@misc{aether4b2026,
  title={Aether-4B: Fine-tuned Gemma 4 for Interactive Fiction},
  author={Aether AI},
  year={2026},
  url={https://github.com/aether-ai/aether-4b}
}
```

---

*Aether-4B is a proof-of-concept. An Aether-31B version trained on 1500+
conversations with extended context is planned.*
