# Text-to-Speech Research

Educational notebooks for learning TTS — all designed to run on **Kaggle GPU**.

## Projects

| # | Folder | Model | What you'll learn |
|---|--------|-------|-------------------|
| 01 | `01_parler_tts/` | Parler-TTS | Control voice with natural language descriptions |
| 02 | `02_bark_tts/` | Bark (Suno) | Multilingual + expressive speech (laughs, singing) |
| 03 | `03_xtts_v2/` | XTTS-v2 | Voice cloning from a 6-second audio clip |
| 04 | `04_xtts_finetune/` | XTTS-v2 | Fine-tune on your own voice data |
| 05 | `05_vietnamese_tts/` | viXTTS | Vietnamese TTS with voice cloning |

## Kaggle Integration

### MCP Server (preferred)

This repo uses the **Kaggle MCP server** (`.mcp.json`) for agent-driven workflows.
The agent can search datasets, models, notebooks, and manage content via MCP tools.

```
# .mcp.json connects to https://www.kaggle.com/mcp
# MCP tools: search_datasets, search_notebooks, search_models, save_notebook, etc.
```

**MCP limitations**: The remote MCP endpoint currently has restricted permissions for
kernel operations (`kernels.get` denied). For push/status/output, fall back to the CLI.

### CLI Push (for notebook execution)

Each project folder contains a `kernel-metadata.json` for direct CLI push:

```bash
# Push any notebook to Kaggle (auto-selects GPU T4 x2)
kaggle kernels push -p 01_parler_tts/

# Check status
kaggle kernels status dstudy213/parler-tts-control-voice-with-natural-language

# Download output
kaggle kernels output dstudy213/parler-tts-control-voice-with-natural-language -p ./output
```

No manual UI steps needed — the agent handles everything automatically.

### Priority Order

1. **Kaggle MCP server** — search, discovery, notebook content operations
2. **Kaggle CLI** — push, status, output (when MCP lacks permissions)
3. **Kaggle Web UI** — last resort, manual inspection

## GPU Selection (kernel-metadata.json)

Each notebook includes `kernel-metadata.json` with `"machine_shape": "NvidiaTeslaT4"` for automatic GPU T4 x2 selection.

Available accelerators for the `machine_shape` field:

| Value | Accelerator |
|-------|-------------|
| `NvidiaTeslaP100` | Tesla P100 (legacy default) |
| `NvidiaTeslaT4` | Tesla T4 x2 (recommended) |
| `NvidiaTeslaT4Highmem` | Tesla T4 high memory |
| `NvidiaTeslaA100` | A100 (competition-only) |
| `NvidiaL4` / `NvidiaL4X1` | NVIDIA L4 |
| `NvidiaH100` | H100 (limited access) |
| `TpuV38` / `Tpu1VmV38` | TPU v3-8 |

**Important**: Setting only `"enable_gpu": "true"` defaults to P100. Always set `"machine_shape"` explicitly.

## Model Comparison

| Feature | Parler-TTS | Bark | XTTS-v2 |
|---------|-----------|------|---------|
| Voice control | Text description | Presets | Reference audio |
| Languages | English | 13+ | 17 (not Vietnamese) |
| Voice cloning | No | No | Yes |
| Expressiveness | Medium | High | Medium |
| Speed | Fast | Slow | Slow |
| Fine-tuning | Possible | No | Easy |

## Kaggle Environment Notes

- **Python**: 3.12 (Kaggle default as of 2026)
- **Coqui TTS**: Use `pip install coqui-tts` (community fork), NOT `TTS==0.22.0` (Python <3.12 only)
- **protobuf**: Must force-reinstall `protobuf>=5.26.1` after installing TTS packages (they pull in 4.x which breaks tensorflow)
- **Coqui TOS**: Set `COQUI_TOS_AGREED=1` env var before importing TTS for batch mode
- **Cross-lingual deps**: Chinese needs `pypinyin`, Japanese needs `cutlet`

## Kaggle Datasets Used

- `luizfelipebjcosta/libri-tts-train-clean-100` — English audiobook speech (7.7GB)
