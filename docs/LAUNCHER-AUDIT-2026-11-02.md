# RR AI — Vulkan Launcher Audit & Refresh

**Date:** 2026-11-02   **Scope:** the three public **Vulkan** launchers (4 GB, 8 GB, 12 GB).
**Verdict:** the latest Vulkan set on hand was **build 2026-10-12c** — one revision behind the private CUDA line, and the 8 GB launcher carried a setting that will not fit an 8 GB card.  Corrected files are attached, brought to parity with the current CUDA builds.

---

## What I found (the "latest" Vulkan launchers)

| Tier | Latest Vulkan file located | Its build |
|------|----------------------------|-----------|
| 4 GB | `c26390db-RR-AI-4GB.bat`  | 2026-10-12c |
| 8 GB | `fb776912-RR-AI-8GB.bat`  | 2026-10-12c |
| 12 GB| `ae4a9e55-RR-AI-12GB.bat` | 2026-10-12c |

Older duplicates also exist (the `1791652439xxx_`-prefixed set) — those are a prior generation (generic `RR-Engine` window title, no `taskkill`, no context flags).  Ignore them.

The private **CUDA** launchers are further along: **4 GB / 12 GB at 2026-10-13a**, **8 GB at 2026-10-13b**.  The 10-13 revisions added model aliases and — critically — fixed the 8 GB 27B memory footprint.  Those changes had **not** been ported to the Vulkan line.

---

## Audit results

| Tier | Status | What was out of date |
|------|--------|----------------------|
| **8 GB** | **Needed a real fix** | 27B + Vision launched with `-c 16384` and a **full-precision KV cache** — too large to fit 8 GB on-GPU.  Also missing model aliases. |
| 12 GB | Minor | Missing model aliases.  (27B at `-c 16384` with full KV is correct here — 12 GB has the room.) |
| 4 GB | Minor | Missing model alias. |

Everything else audited **clean and current** on all three: model filenames, `--no-mmproj-offload` (vision runs on CPU — as intended on the 8 GB box), `-ngl 99`, `-fa on`, `--parallel 1`, `--image-max-tokens 1024`, the image-model lineup, ports (8080 / 7860), and the launch/wait/boot-browser logic.

---

## Changes made (now build 2026-10-13a / -13b, Vulkan)

**8 GB — `RR-AI-8GB.bat` — build 2026-10-13b**
- 27B/Vision line, context + KV cache corrected to fit 8 GB on-GPU:

  *was:* `… -ngl 99 -fa on -c 16384 --parallel 1 --no-mmproj-offload …`
  *now:* `… -ngl 99 -fa on -c 8192 -ctk q8_0 -ctv q8_0 --parallel 1 --no-mmproj-offload …`
- Added `--alias` to all three text models (Bonsai 8B, Qwen2.5-Coder 7B, Bonsai 2 27B).

**12 GB — `RR-AI-12GB.bat` — build 2026-10-13a**
- Added `--alias` to all three text models (Bonsai 8B, Qwen2.5-Coder 14B, Bonsai 2 27B).  No context change.

**4 GB — `RR-AI-4GB.bat` — build 2026-10-13a**
- Added `--alias "Bonsai 8B"`.

The friendly `--alias` names make the model show up by name (e.g. "Bonsai 2 27B") in the web UI instead of the raw `.gguf` filename.

---

## One thing to verify on the 8 GB box ⚠️

The 8 GB fix mirrors the validated **CUDA** setting, but one flag is backend-sensitive:

- `-ctk q8_0 -ctv q8_0` (quantized KV cache) requires the **Vulkan** build of `llama-server` to support quantized KV with flash-attention.  Recent llama.cpp Vulkan builds do; an older one may not.
- **Test:** launch option **[2] Assistant + Vision** on an actual 8 GB card and confirm the 27B loads and answers.
- **If it fails to load:** drop `-ctk q8_0 -ctv q8_0` and keep `-c 8192` (full KV at half the context still roughly halves the KV footprint vs. the old 16 K); if it's still tight, step down to `-c 6144`.

Also note the user-facing trade: on 8 GB the assistant's context drops from 16 K to 8 K tokens.  That's the deliberate cost of fitting the 27B + vision on-GPU; the 12 GB box keeps the full 16 K.

---

## Not changed (flagged for a separate decision)

- **LibreWolf detection.**  The CUDA line uses `where librewolf.exe` + a Program Files fallback; the Vulkan files use a Program Files path check only.  Both work for a standard install; I left the Vulkan block as-is to keep the diff minimal.  Say the word and I'll align it.

*Renewable Revolt — v7.8 family · Vulkan (public) launchers.*
