# Abliterated Qwen3.8-27B Q6_K on Apple Silicon

**Baseline 262K Serving Profile, Memory Footprint, and Latency Analysis**

**Chengyi Xu and Nexora — Nexora Research**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Context: 262k](https://img.shields.io/badge/context-262%2C144-blue.svg)](MANIFEST.json)

---

## Overview

This repository documents the baseline local deployment and benchmark qualification of OrcaRouter's abliterated (uncensored) **Qwen3.8-27B Q6_K** GGUF at the native **262,144-token context window** on an Apple M4 Max 64 GB host.

### Runtime Configuration

- **Model:** `orcarouter/Qwen3.8-27B-Uncensored-GGUF`
- **Artifact:** `Qwen3.8-27B-Uncensored-Q6_K.gguf` (22.43 GB, SHA-256: `1a0d6fe2e48417f1571a57a7104b4d5da0c45c3d00de18fa7aa48affca5a24d7`)
- **Server:** `llama-server` (Metal, all layers offloaded to GPU)
- **Context Window:** Native 262,144 tokens
- **KV Cache:** Symmetric `q4_0` K and `q4_0` V cache to fit within unified memory
- **Security Policy:** Loopback-only (`127.0.0.1:7870`), protected by an API-key file

---

## Measured Performance & Agentic Verification

1. **Model Discovery & Authentication:** Unauthenticated requests fail-closed with HTTP 401; authenticated requests return complete model card information.
2. **Deterministic Probes:** Passed non-thinking output checks (`QWEN_LOCAL_OK`) and tool-calling function argument generation.
3. **Real Coding Agent Task:** Evaluated with a real coding agent harness on a multi-step workspace debugging task. Inspected a defective Python module, surgically edited the offending block, verified against automated test suites, and completed successfully (`AGENT_262K_OK`, exit code 0).
4. **Latency Analysis:**
   - Short prompt decode: **~15.7 tok/s**.
   - Deep prompt prefill (103k tokens): **~3.9 tok/s**.
   - Cold prefill latency for dense 27B weights highlights the necessity of prefix caching and speculative acceleration for multi-turn agent execution on local unified memory.

---

## Repository Contents

```
qwen38-uncensored-q6k-eval/
├── README.md              # Deployment overview and benchmark results
├── CITATION.cff           # Citation metadata
├── LICENSE                # MIT License
├── MANIFEST.json          # Deployment and verification specification
├── deploy/
│   └── local.llamacpp.qwen38-uncensored-q6k.plist.example # Service template
└── source/
    ├── README.md          # Upstream model documentation and card
    └── huggingface-model-metadata.json # Hugging Face revision metadata
```

---

## Citation

```bibtex
@dataset{xu2026qwen38q6keval,
  title  = {Abliterated Qwen3.8-27B Q6_K on Apple Silicon: Baseline 262K Serving Profile, Memory Footprint, and Latency Analysis},
  author = {Xu, Chengyi and {{Nexora}}},
  year   = {2026},
  month  = oct,
  url    = {https://github.com/chengyixu/qwen38-uncensored-q6k-eval}
}
```

---

## License

MIT License. Copyright (c) 2026 Chengyi Xu and Nexora.
Model weights are property of their respective creators and are not distributed in this repository.
