---
name: CPU inference limits for MoE models (qwen35moe / GatedDeltaNet)
description: Q8_0 quantization of 35B MoE models is not viable on Xeon E5-2690 v4 — bandwidth-bound and SSM-unfriendly
type: project
date: 2026-04-18
tags: [ollama, llm, cpu-inference, moe, hardware]
---

## Lesson

**Q8_0 of qwen35moe (35B) on Xeon E5-2690 v4 produces zero usable throughput.**

Root cause: memory bandwidth ceiling (~100 GB/s for dual-socket DDR4-2133) divided by model size (35 GB) gives theoretical max of ~2-3 tok/s — but the GatedDeltaNet hybrid SSM layers in qwen35moe architecture add sequential computation overhead that makes CPU inference even worse in practice.

## Rules Derived

1. **Model size / bandwidth = tok/s ceiling.** For any CPU inference attempt: `bandwidth_GB_s / model_size_GB = max_tok_s`. If max is < 3 tok/s, expect unusable for interactive.
2. **Check architecture before downloading.** qwen35moe (GatedDeltaNet hybrid) ≠ standard MoE. SSM layers are not CPU-optimized. Stick to transformer-only MoE or dense for CPU inference.
3. **Q4 is the minimum viable quantization for 35B on this machine.** Q4_K_M (22 GB): ~100/22 = 4.5 tok/s theoretical — marginal but testable.
4. **Ollama version must match model release.** New architectures need new ollama. Always check: `ollama --version` vs model release date.
5. **`/mnt/data` for models, never `/` (92% full).** The 2 TB SSD at `/mnt/data` is the correct model storage location.

## Hardware Profile

- CPU: Xeon E5-2690 v4 × 2 (56 threads, AVX2, no AVX-512)
- RAM: 251 GiB DDR4-2133, 8 channels
- Bandwidth: ~100-110 GB/s real-world
- SSD: 1.6 TB free at `/mnt/data`
- GPU: none currently
